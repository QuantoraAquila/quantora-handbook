# 1. FastAPI 是什么、功能和意义

---

## 定义
FastAPI 是一个基于 Python 3.6+ 类型注解（type hints） 的现代 Web 框架，用于构建高性能 API 服务。
它的核心技术基础是：
* Starlette（处理 Web 路由、请求/响应、中间件等）
* Pydantic（进行数据验证和序列化）
## 主要功能
* 高性能：性能接近 Node.js 和 Go（得益于 async / await + uvicorn + Starlette）。

* 类型驱动：利用 Python 的类型注解进行数据验证、自动生成文档。

* 自动文档：内置 Swagger UI 和 ReDoc，无需手写 API 文档。

* 异步支持：天然支持 async / await，非常适合高并发场景（如 WebSocket、实时接口）。

* 依赖注入：提供优雅的依赖管理机制（如数据库会话、认证逻辑）。

* 易用性：新手可快速上手，老手可以充分利用 Python typing 构建复杂系统。

## 意义
FastAPI 让 Python 后端开发有了类似 TypeScript + Express 的“类型安全 + 高性能”体验，尤其适合：

* 微服务 API 网关

* 机器学习/数据科学模型的 API 封装

* 高并发实时服务（WebSocket / Streaming）

* 中小型后端项目快速开发

## 2. Python 版本演进 & FastAPI 改动

---
FastAPI 本身是一个框架，不是 Python 官方库，所以它的演变更多取决于：

* Python 语言版本特性

* Starlette 和 Pydantic 的升级

我帮你整理了一个时间线，方便看清变化：

| 时间 / 版本       | Python 新特性                                         | FastAPI 相关影响             | 框架本身的重要改动                           |                           |
| ------------- | -------------------------------------------------- | ------------------------ |-------------------------------------| ------------------------- |
| **2018-2019** | Python 3.6 类型注解 (PEP 484)，`dataclasses`            | FastAPI 初版要求 Python 3.6+ | 初版发布，依赖 Pydantic v1.x，集成自动文档生成      |                           |
| **2020**      | Python 3.7 引入 `from __future__ import annotations` | 提升类型解析速度                 | 支持复杂依赖注入，增加 WebSocket 支持            |                           |
| **2021**      | Python 3.8 支持 `TypedDict`、`Literal`                | 更精确的数据模型类型支持             | 引入 `BackgroundTasks`，完善异步依赖         |                           |
| **2022**      | Python 3.9 标准库 `zoneinfo`，字典合并 \`                  | \` 运算符                   | 时区处理更方便  |
| **2023**      | Python 3.10 模式匹配（`match`）、`TypeAlias`              | 更强类型系统                   | **重大变更**：适配 Pydantic v2，提高数据验证性能    |                           |
| **2024**      | Python 3.11 性能提升（+10%-60%）、`Self` 类型               | 框架运行更快                   | 升级 Starlette，优化启动速度，增加依赖缓存          |                           |
| **2025**      | Python 3.12 类型注解更强、解析器优化                           | 编写复杂依赖更轻松                | FastAPI 与 Pydantic v2 深度整合，文档系统更可定制 |                           |

