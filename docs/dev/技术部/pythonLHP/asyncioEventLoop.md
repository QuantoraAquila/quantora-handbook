# 透彻了解事件循环
系统性地、从「宏观到底层」整理出pending、_scheduled heap、_ready queue、selector这四个核心概念在 asyncio 事件循环中的地位与交互逻辑。
## 🧩 一、asyncio 事件循环的总体架构

当我们运行：
```
asyncio.run(main())
```

时，Python 创建了一个事件循环（EventLoop），
它负责在整个程序生命周期中：

不断地从“任务池”中挑选可以执行的协程，执行一点点，再切出去，
等待 I/O 或时间条件满足后再回来继续执行。

## 🧱 二、事件循环中的核心数据结构

在 asyncio.BaseEventLoop 内部，最关键的调度结构包括：

| 名称                  | Python源码中常见字段                   | 类型                          | 职责                                               |
| ------------------- | ------------------------------- | --------------------------- | ------------------------------------------------ |
| **_ready queue**    | `_ready`                        | `collections.deque`         | 存放**立即可执行的任务/回调**（下一轮循环立刻执行）                     |
| **_scheduled heap** | `_scheduled`                    | `heapq`（小顶堆）                | 存放**延迟任务**（到期时间最早的任务在前）                          |
| **selector**        | `_selector`                     | `selectors.BaseSelector` 实例 | 监听**I/O 事件**（socket可读、可写等）                       |
| **pending tasks**   | `asyncio.Task._all_tasks`（逻辑概念） | `set`                       | 当前处于**未完成状态**的所有 Task 集合（不一定在 ready/scheduled 中） |


## 🧠 三、核心概念详解
### 1️⃣ pending（挂起任务）

**定义：** 所有创建了但未完成的协程任务（Task），都属于 pending 状态。

它们可能处于以下几种情况：

- 正在 _ready 中等待执行；

- 在 _scheduled 中等待超时唤醒；

- 注册在 selector 上等待 I/O；

- 刚创建但尚未加入事件循环。

**特点：**

- “pending” 并不是一个单独的数据结构；

- 它是 Task 的一种运行状态；

- 一旦协程执行完返回或抛出异常，它就不再是 pending。

**🧩 类比：** pending = “系统里所有未完成的工作单”。

### 2️⃣ _ready queue（就绪队列）

**定义：** 存放所有马上可以被执行的任务或回调。

- 实现：collections.deque

- 添加时机：

    - await asyncio.sleep(0) → 马上进入 ready；

    - selector 检测到 I/O 事件；

    - _scheduled 中到期任务；

- 执行逻辑：

    - 事件循环每次循环开始都会不断 popleft() 并执行；

    - 执行时会调用 Task 的 ._step() 方法，推进协程运行。

**🧩 类比：** ready queue = “现在就能开始干的任务队列”。

### 3️⃣ _scheduled heap（定时任务堆）

**定义：** 存放那些需要在未来某个时间点执行的任务。

- 实现：最小堆（heapq），按“唤醒时间”排序；

- 使用场景：

    - await asyncio.sleep(n)

    - 超时机制

    - 延迟执行任务

- 当时间到达或超过任务的唤醒时间时，它会被从 _scheduled 移到 _ready 队列。

🧩 类比： scheduled heap = “闹钟提醒表”，时间到就叫醒。

### 4️⃣ selector（I/O 事件监听器）

定义： 一个负责与操作系统交互的 I/O 事件监控器。

- 实现：selectors.BaseSelector（底层可用 epoll/kqueue/select/IOCP）

- 作用：

  - 注册 socket、pipe 等文件描述符；

  - 阻塞等待可读/可写事件；

  - 返回准备就绪的 I/O 回调；

- 当某个 I/O 就绪，loop 把对应 Task 放入 _ready 队列。

🧩 类比： selector = “监听网络端口变化的看门人”。

## 🔁 四、整个调度过程的动态示例

以你的 websocket 程序为例：
```
await websocket.connect()   # 建立连接
await websocket.send()      # 发订阅
async for msg in websocket: # 持续接收
```

事件循环的动态过程如下：

1️⃣ main() 被放入 _ready 队列；

2️⃣ loop 执行 main() → 调用 await websocket.start()；

3️⃣ start() 内部执行 create_task(self.consume())；
→ consume() 被注册为一个 pending task；

4️⃣ main() 执行到 await asyncio.sleep(3) →
被放入 _scheduled heap；

5️⃣ consume() 在等待 await websocket.recv() →
注册到 selector，等待 socket 可读；

6️⃣ 网络有数据 → selector 通知 → consume() 放入 _ready；

7️⃣ loop 下一轮执行 consume() → 调用 callback(message)；

8️⃣ 若 callback 内有 I/O（例如 redis.lpush），
又会经历同样的挂起 → selector 监听 → 唤醒。

整个循环就这样永不停息地在三个主要结构间流动：

```_ready ↔ _scheduled ↔ selector```

## 🧠 五、总结对比表

| 名称                 | 类型      | 存放内容      | 唤醒条件  | 示例             |
| ------------------ | ------- | --------- | ----- | -------------- |
| **pending**        | 状态集合    | 所有未完成协程   | 等待结束  | 任意挂起任务         |
| **ready queue**    | deque   | 可立即执行任务   | 立即执行  | create_task()  |
| **scheduled heap** | heap    | 延时任务      | 到期时间  | await sleep(n) |
| **selector**       | I/O 监控器 | 等待I/O事件任务 | I/O就绪 | await recv()   |

## ✅ 一句话总结核心思维：

asyncio 的事件循环是一个三层调度系统：

- _ready 执行“马上能干的”任务；
- _scheduled 管理“时间到再干”的任务；
- selector 监听“数据到了再干”的任务；
- 所有未完成任务都属于 pending 状态。
