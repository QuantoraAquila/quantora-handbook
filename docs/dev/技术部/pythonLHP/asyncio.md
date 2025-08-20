# Python `asyncio` 系统性总结

> 适用于：Python 3.4 ~ 3.12  
> 更新时间：2025-07  
> 作者：ChatGPT

---

## 📌 目录

1. [简介](#简介)
2. [核心概念](#核心概念)
3. [关键语法与组件](#关键语法与组件)
4. [常用 API 示例](#常用-api-示例)
5. [各版本更新历史](#各版本更新历史)
6. [参考资料](#参考资料)

---

## 简介

`asyncio` 是 Python 从 3.4 起引入的 **异步 I/O 框架**，用于编写并发代码，适合 I/O 密集型任务，如网络请求、数据库交互等。

**特点：**

- 基于 **事件循环（Event Loop）**
- 使用 `async` / `await` 语法（Python 3.5+）
- 支持高并发的任务调度
- 适合替代传统的 `threading` / `multiprocessing`（适用于 I/O 场景）

---

## 核心概念

| 概念              | 说明 |
|-------------------|------|
| 协程（coroutine） | 可暂停与恢复的函数，使用 `async def` 定义 |
| 事件循环（event loop） | 协调任务执行的中心，管理任务调度 |
| 任务（Task）       | 协程的封装体，由事件循环调度执行 |
| Future            | 表示未来结果的占位符对象 |
| awaitable 对象    | 可用 `await` 等待的对象，如协程、Future、Task |

---

## 关键语法与组件

### 1. 定义协程

```
#python

async def fetch_data():
    await asyncio.sleep(1)
    return "数据返回"
```
### 2. 启动事件循环
```
#python

import asyncio

async def main():
    result = await fetch_data()
    print(result)

asyncio.run(main())  # Python 3.7+
```