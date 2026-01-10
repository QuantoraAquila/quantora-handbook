# Python `asyncio` 系统性总结

> 适用于：Python 3.4 ~ 3.12  
> 更新时间：2025-07  
> 作者：ChatGPT

---

## 📌 目录

1. [简介](#简介)
2. [各版本更新历史](#各版本更新历史)
3. [核心概念](#核心概念)
4. [关键语法与组件](#关键语法与组件)
5. [常用 API 示例](#常用-api-示例)

---

## 1、简介

`asyncio` 是 Python 从 3.4 起引入的 **异步 I/O 框架**，用于编写并发代码，适合 I/O 密集型任务，如网络请求、数据库交互等。

**特点：**

- 基于 **事件循环（Event Loop）**
- 使用 `async` / `await` 语法（Python 3.5+）
- 支持高并发的任务调度
- 适合替代传统的 `threading` / `multiprocessing`（适用于 I/O 场景）

---

## 2、各版本更新历史
Python 的 ```asyncio``` 库自 3.3 版本引入以来，经历了多次改进和增强，逐步成为 Python 中进行异步编程的核心工具。下面是一些重要的更新和语法演变：

### 2.1 Python 3.3（引入 asyncio）

- **引入 ```asyncio```**：Python 3.3 开始内置了 asyncio，使得 Python 支持了基于事件循环的异步编程模式。这个库的推出为 Python 开发者提供了一种新的并发编程模型，避免了传统的线程和进程开销。

- **基本概念**：

  - **事件循环**：```asyncio``` 的核心是事件循环，通过 ```asyncio.get_event_loop()``` 获取循环实例，管理异步任务。

  - **协程**：引入了 ```@asyncio.coroutine``` 装饰器来定义协程函数，并用 ```yield``` 来等待异步操作。

  - **任务和 Future**：```asyncio``` 提供了 ```Task``` 和 ```Future``` 对象来表示异步操作的结果，协程通过这些对象与事件循环进行交互。

### 2.2 Python 3.5（引入 async 和 await 关键字）

- **引入 ```async``` 和 ```await```**：Python 3.5 推出了 async 和 await 关键字，使协程的语法更加简洁易懂。通过这两个关键字，Python 开发者能够像编写同步代码一样编写异步代码。

  - ```async def```：用于定义协程函数。

  - ```await```：在协程内部使用 await 来等待另一个异步操作完成。

- **改进了异步 I/O**：Python 3.5 改进了 asyncio 的任务管理，并引入了 ```asyncio.create_task()``` 来创建任务对象，而不再依赖 ```@asyncio.coroutine``` 装饰器和 ```yield```。

### 2.3 Python 3.6（引入 asyncio.run()）

- **引入** ```asyncio.run()```：Python 3.6 增加了 asyncio.run() 方法，使得运行异步程序变得更加简洁。这个方法用于启动一个事件循环，执行协程，并在执行完成后关闭循环。

- **字符串插值改进**：虽然与 asyncio 无直接关系，但 Python 3.6 引入的 f-string（格式化字符串）在编写异步代码时，也大大提高了可读性。

### 2.4 Python 3.7（改进 asyncio）

- **事件循环改进**：Python 3.7 引入了对事件循环的改进，特别是 ```asyncio.run()``` 的优化，使得协程的生命周期管理更加高效。

- asyncio **队列和其他工具**：新增了 ```asyncio.Queue ```等工具，进一步增强了 asyncio 的功能，提供了类似于线程安全的队列等并发工具，方便开发者实现更复杂的异步任务。

### 2.5 Python 3.8（引入 asyncio 中的 asyncio.to_thread()）

- ```asyncio.to_thread()```：Python 3.8 引入了 asyncio.to_thread() 函数，使得调用同步阻塞操作变得更方便。你可以将同步阻塞的函数放入后台线程中执行，而不阻塞主线程的事件循环。

- **f-string 支持调试信息**：继续加强了 f-string 的支持和调试能力，尽管与 asyncio 无直接关联，但对于写异步代码的开发者来说，调试异步代码时的便利性提升显著。

### 2.6 Python 3.9（增强异步 I/O 的性能）

- **性能提升**：Python 3.9 增强了 asyncio 在多核处理器上的表现，改进了任务调度的效率和性能。

- **新的 asyncio API**：引入了 ```asyncio.TaskGroup```，使得任务组管理变得更加方便，并改善了任务的取消和错误处理。

### 2.7 Python 3.10（结构化异常处理）

- **结构化异常处理（PEP 618）**：为 asyncio 引入了更清晰的异常处理机制，能够在协程中捕获多种不同的异常，便于开发者做更细粒度的错误处理。

- asyncio **任务调度改进**：进一步改进了 asyncio 的任务调度机制，减少了性能瓶颈。

### 2.8 Python 3.11（增强性能，改进协程）

- **性能提升**：Python 3.11 对 asyncio 的性能进行了显著优化，尤其是在协程调度方面，提升了异步 I/O 的吞吐量和效率。

- **协程优先级调度**：对异步任务的调度引入了更多的控制，例如优先级调度等，增强了 asyncio 对复杂应用场景的支持。

## 3、核心概念：

| 概念              | 说明 |
|-------------------|------|
| 协程（coroutine） | 可暂停与恢复的函数，使用 `async def` 定义 |
| 事件循环（event loop） | 协调任务执行的中心，管理任务调度 |
| 任务（Task）       | 协程的封装体，由事件循环调度执行 |
| Future            | 表示未来结果的占位符对象 |
| awaitable 对象    | 可用 `await` 等待的对象，如协程、Future、Task |
---
### 3.1 事件循环
⚙️ 在 asyncio 中，事件循环内部有两个核心任务队列结构：

- _ready —— ✅ FIFO 队列（Queue-like），像队列（先进先出）；

- _scheduled —— ⏳ 小顶堆（min-heap），用来管理带延迟的任务（按时间排序）。

也就是说，
asyncio 的事件循环同时使用了「队列 + 堆」两种结构，各自负责不同类型的调度。

#### 🧩 3.1.1 事件循环内部结构总览

先看 Python 源码中的核心实现（以 asyncio/base_events.py 为例）：
```
class BaseEventLoop:
    def __init__(self):
        self._ready = collections.deque()   # 立即可执行的任务
        self._scheduled = []                # 定时任务 (heapq)
```

- ✅ _ready 是一个 collections.deque（双端队列）
- ✅ _scheduled 是一个普通 Python list，但通过 heapq 管理成 小顶堆

#### 🧠 3.1.2 两个队列的职责与特点
##### 1️⃣ _ready：就绪队列（Ready Queue）

- 存放立即可以执行的任务回调（callback、task step）。

- 按照**先进先出（FIFO）**顺序执行。

- 结构：collections.deque（双端队列）。

✅ 示例类比

假设你有多个任务刚刚被唤醒（比如 await 完成）：
```
_ready: [task1, task2, task3]
```

事件循环每次会从左边（头部）取出一个执行：
```
pop from left → execute task1
```

执行完一个，再取下一个。

- 📘 类比：_ready 就像一个「排队等待 CPU 执行的协程队列」。

##### 2️⃣ _scheduled：定时任务队列（Scheduled Queue）

- 存放未来某个时间点执行的任务，例如 ```await asyncio.sleep(3)```。

- 按执行时间（绝对时间戳）升序排序。

- 结构：heapq 实现的小顶堆（min-heap）。

✅ 示例类比
```
_scheduled:
[(168.0, handle1), (170.5, handle2), (172.3, handle3)]
```

事件循环通过 heapq.heappop() 获取堆顶最小时间值（最早需要执行的任务）。

- 📘 类比：_scheduled 是一个「按时间优先级排序的延迟任务堆」。

#### ⚙️ 3.1.3 事件循环的调度逻辑（简化版）

事件循环在内部执行大致流程如下：
```
while True:
    # 1. 检查 _scheduled 堆顶（是否有到期任务）
    now = time.monotonic()
    while _scheduled and _scheduled[0]._when <= now:
        handle = heapq.heappop(_scheduled)
        _ready.append(handle)

    # 2. 从 _ready 队列取出任务执行
    ntodo = len(_ready)
    for i in range(ntodo):
        handle = _ready.popleft()
        handle._run()  # 执行任务（推进协程）

    # 3. 等待下一轮 IO 或定时任务
    timeout = compute_next_timeout(_scheduled)
    selector.select(timeout)
```

📍关键点：

- _scheduled 控制「任务何时进入就绪态」；

- _ready 控制「任务的执行顺序」；

- 两者共同支撑事件循环的完整调度周期。
## 4、关键语法与组件

### 4.1 定义协程

```
#python

async def fetch_data():
    await asyncio.sleep(1)
    return "数据返回"
```
### 4.2 启动事件循环
```
#python

import asyncio

async def main():
    result = await fetch_data()
    print(result)

asyncio.run(main())  # Python 3.7+
```
### 4.3 任务（Task）

任务是对协程的封装，表示一个异步操作的执行，通常用来并发执行多个协程。
```
async def task1():
    await asyncio.sleep(1)
    print("Task 1 completed")

async def task2():
    await asyncio.sleep(2)
    print("Task 2 completed")

async def main():
    # 并发运行任务
    task1_obj = asyncio.create_task(task1())
    task2_obj = asyncio.create_task(task2())
    
    # 等待任务完成
    await task1_obj
    await task2_obj

asyncio.run(main())
```

在上面的例子中，```asyncio.create_task()``` 用来启动任务，任务是异步执行的，不会阻塞其他任务的执行。

### 4.4 asyncio.gather()

```asyncio.gather() ```用于并发执行多个协程，它会返回一个协程对象，该对象在所有传入的协程完成时返回结果。
```
async def foo():
    await asyncio.sleep(1)
    return "foo done"

async def bar():
    await asyncio.sleep(2)
    return "bar done"

async def main():
    # 等待所有协程完成并返回结果
    results = await asyncio.gather(foo(), bar())
    print(results)  # 输出：['foo done', 'bar done']

asyncio.run(main())
```
### 4.5 异常处理

异步程序中的异常处理与同步程序类似，但需要特别注意的是，异常可能出现在协程内部或任务中。可以使用 try-except 语句来捕获和处理异常。
```
async def risky_task():
    raise ValueError("Something went wrong")

async def main():
    try:
        await risky_task()
    except ValueError as e:
        print(f"Caught an error: {e}")

asyncio.run(main())
```
### 4.6 asyncio.to_thread()

在异步代码中，某些同步操作可能会阻塞事件循环，```asyncio.to_thread()``` 提供了一种将同步代码放入单独线程中执行的方式，避免阻塞事件循环。
```
import asyncio
import time

def blocking_task():
    time.sleep(2)
    return "Blocking task completed"

async def main():
    result = await asyncio.to_thread(blocking_task)
    print(result)

asyncio.run(main())
```
### 4.7 任务组（Task Group）

在 Python 3.11 及以后版本中，asyncio 引入了 ```asyncio.TaskGroup```，使得协程任务的管理和错误处理更加简便和一致。
```
async def task1():
    await asyncio.sleep(1)
    print("Task 1 completed")

async def task2():
    await asyncio.sleep(2)
    print("Task 2 completed")

async def main():
    async with asyncio.TaskGroup() as tg:
        tg.create_task(task1())
        tg.create_task(task2())

asyncio.run(main())
```

asyncio.TaskGroup 会自动管理任务的生命周期，所有任务都在 async with 块内启动，并且如果某个任务抛出异常，会取消其他任务并传播异常。

### 4.8 定时器与超时控制

asyncio 提供了多种方法来设置超时。例如，使用 ```asyncio.wait_for()``` 来控制协程的最大执行时间：
```
async def long_running_task():
    await asyncio.sleep(10)
    return "Task Completed"

async def main():
    try:
        result = await asyncio.wait_for(long_running_task(), timeout=5)
    except asyncio.TimeoutError:
        print("Task timed out")
    else:
        print(result)

asyncio.run(main())
```

在这个例子中，如果 long_running_task 执行超过 5 秒，asyncio.wait_for() 会抛出 TimeoutError。

### 4.9 事件和信号量

asyncio 还提供了多种同步原语来控制协程之间的同步和并发，例如 ```asyncio.Event``` 和 ```asyncio.Semaphore```。

- ```asyncio.Event```：用于协程间的通知机制，可以控制一个协程等待另一个协程的信号。
```
import asyncio

async def waiter(event):
    print("Waiting for event")
    await event.wait()  # 等待事件
    print("Event received!")

async def setter(event):
    await asyncio.sleep(1)
    print("Setting event")
    event.set()  # 设置事件

async def main():
    event = asyncio.Event()
    await asyncio.gather(waiter(event), setter(event))

asyncio.run(main())
```

- ```asyncio.Semaphore```：用于控制并发协程的数量。例如，限制同时执行的协程数。
```
import asyncio

async def limited_task(sem):
    async with sem:  # 只有在信号量允许时，才会执行
        print("Task started")
        await asyncio.sleep(1)
        print("Task finished")

async def main():
    sem = asyncio.Semaphore(2)  # 限制最多同时 2 个协程执行
    await asyncio.gather(limited_task(sem), limited_task(sem), limited_task(sem))

asyncio.run(main())
```
### 4.10 总结

asyncio 是 Python 中处理异步编程的强大工具，能够有效地处理高并发任务。关键点包括：

- 协程：通过 async def 定义，使用 await 来挂起和恢复执行。

- 任务：通过 asyncio.create_task() 和 asyncio.gather() 来并发执行多个协程。

- 事件循环：asyncio.run() 用来启动事件循环并执行协程。

- 异常处理：在异步代码中也可以使用 try-except 来处理异常。

- 同步原语：通过 Event、Semaphore 等原语实现协程间的同步和控制。

## 5、典型API示例
按“功能类别”系统列出 asyncio 的常用 API，每个都附带简短说明与一个可运行的示例。

### 🧩 5.1 事件循环管理（Event Loop）
#### 1️⃣ asyncio.run(coro)

✅ 运行顶层协程（自动创建并关闭事件循环）
```
import asyncio

async def main():
    print("Hello asyncio")
    await asyncio.sleep(1)
    print("Goodbye asyncio")

asyncio.run(main())
```

#### 📘 说明： 
```asyncio.run()``` 是运行异步程序的推荐入口（从 Python 3.7 起）。
它会自动创建事件循环、执行协程、关闭循环。

#### 2️⃣ asyncio.get_running_loop()

获取当前正在运行的事件循环（在协程内部使用）
```
import asyncio

async def main():
    loop = asyncio.get_running_loop()
    print(loop)

asyncio.run(main())
```
#### 3️⃣ asyncio.get_event_loop()（⚠️旧式用法）

在协程外部获取或创建事件循环（旧版本常用）
```
import asyncio

async def hello():
    print("hello")

loop = asyncio.get_event_loop()
loop.run_until_complete(hello())
loop.close()
```

🚨 从 Python 3.10 起建议改用 ```asyncio.run()```。

### 🧱 5.2 协程与任务（Coroutines & Tasks）
#### 4️⃣ asyncio.create_task(coro)

将协程包装成任务（Task），提交给事件循环执行。
```
import asyncio

async def worker(name):
    await asyncio.sleep(1)
    print(f"{name} done")

async def main():
    t1 = asyncio.create_task(worker("A"))
    t2 = asyncio.create_task(worker("B"))
    await t1
    await t2

asyncio.run(main())
```

📘 说明：
任务一经创建就会立即加入事件循环调度，不会阻塞。

#### 5️⃣ asyncio.gather(*coros)

并发执行多个协程，并在全部完成后返回结果。
```
import asyncio

async def foo():
    await asyncio.sleep(1)
    return "foo"

async def bar():
    await asyncio.sleep(2)
    return "bar"

async def main():
    results = await asyncio.gather(foo(), bar())
    print(results)  # ['foo', 'bar']

asyncio.run(main())
```
#### 6️⃣ asyncio.wait(tasks, *, timeout=None)

更底层的控制接口，可设置超时或检查部分任务完成。
```
import asyncio

async def work(i):
    await asyncio.sleep(i)
    return i

async def main():
    tasks = [asyncio.create_task(work(i)) for i in [1, 2, 3]]
    done, pending = await asyncio.wait(tasks, timeout=2)
    print("Done:", len(done), "Pending:", len(pending))

asyncio.run(main())
```
### ⏳ 5.3 延迟与超时控制
#### 7️⃣ asyncio.sleep(delay)

非阻塞地暂停指定时间。
```
import asyncio

async def main():
    print("Sleep start")
    await asyncio.sleep(1)
    print("Sleep end")

asyncio.run(main())
```
#### 8️⃣ asyncio.wait_for(coro, timeout)

等待协程执行完成或超时抛出异常。
```
import asyncio

async def slow():
    await asyncio.sleep(5)
    return "done"

async def main():
    try:
        result = await asyncio.wait_for(slow(), timeout=2)
    except asyncio.TimeoutError:
        print("Timeout!")

asyncio.run(main())
```
### 🧠 5.4 线程与阻塞操作整合
#### 9️⃣ asyncio.to_thread(func, *args)

在后台线程中执行阻塞函数，不阻塞主循环。
```
import asyncio
import time

def blocking():
    time.sleep(2)
    return "done"

async def main():
    print("Before blocking")
    result = await asyncio.to_thread(blocking)
    print("After blocking:", result)

asyncio.run(main())
```
### ⚙️ 5.5 同步原语（并发控制）
#### 🔟 asyncio.Semaphore(n)

限制同时运行的任务数量。
```
import asyncio

async def worker(i, sem):
    async with sem:
        print(f"Task {i} start")
        await asyncio.sleep(1)
        print(f"Task {i} end")

async def main():
    sem = asyncio.Semaphore(2)
    await asyncio.gather(*(worker(i, sem) for i in range(5)))

asyncio.run(main())
```
#### 11 asyncio.Event()

实现协程间的信号通信。
```
import asyncio

async def waiter(event):
    print("Waiting for event...")
    await event.wait()
    print("Event received!")

async def setter(event):
    await asyncio.sleep(2)
    print("Setting event")
    event.set()

async def main():
    event = asyncio.Event()
    await asyncio.gather(waiter(event), setter(event))

asyncio.run(main())
```
### 🧩 5.6 任务组（结构化并发，Python 3.11+）
#### 12 asyncio.TaskGroup()

自动管理多个任务的生命周期，统一异常处理。
```
import asyncio

async def worker(name, delay):
    await asyncio.sleep(delay)
    print(f"{name} done")

async def main():
    async with asyncio.TaskGroup() as tg:
        tg.create_task(worker("A", 1))
        tg.create_task(worker("B", 2))
    print("All done")

asyncio.run(main())
```
### 🪄 5.7 任务与循环状态查询
#### 13 asyncio.all_tasks(loop=None)

返回指定事件循环中的所有活动任务。
```
import asyncio

async def foo():
    await asyncio.sleep(1)

async def main():
    t = asyncio.create_task(foo())
    print(asyncio.all_tasks())  # 查看当前活跃任务
    await t

asyncio.run(main())
```
#### 14 asyncio.current_task(loop=None)

返回当前正在执行的任务。
```
import asyncio

async def show_task():
    task = asyncio.current_task()
    print("Current:", task)

asyncio.run(show_task())
```
### 📚 5.8 底层接口（了解即可）

| API                                | 说明              |
| ---------------------------------- | --------------- |
| `loop.call_soon(callback, *args)`  | 把回调函数立即加入事件循环   |
| `loop.call_later(delay, callback)` | 延迟执行回调          |
| `loop.call_at(when, callback)`     | 在特定时间点执行回调      |
| `loop.create_future()`             | 手动创建 Future 对象  |
| `loop.time()`                      | 返回事件循环的时间（单调递增） |

### 5.9 总结

| 功能类别   | 常用 API                                | 说明      |
| ------ | ------------------------------------- | ------- |
| 事件循环管理 | `asyncio.run()`, `get_running_loop()` | 启动和控制循环 |
| 协程调度   | `create_task()`, `gather()`           | 并发执行协程  |
| 延迟与超时  | `sleep()`, `wait_for()`               | 控制执行时间  |
| 同步与信号  | `Event`, `Semaphore`                  | 协程间同步   |
| 阻塞整合   | `to_thread()`                         | 调用同步函数  |
| 结构化并发  | `TaskGroup`                           | 自动管理子任务 |
| 状态监控   | `all_tasks()`, `current_task()`       | 查询任务状态  |

