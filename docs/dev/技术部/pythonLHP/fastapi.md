# FastAPI 是什么、功能和意义

---
## fastapi 发布版本
| 版本       | 发布日期       | 重要变更 / 特性                             |
| -------- | ---------- | ------------------------------------- |
| 0.116.1  | 2025-07-11 | 升级 Starlette 支持范围至 `>=0.40.0,<0.48.0` |
| 0.116.0  | 2025-07-07 | —（版本号发布记录）                            |
| 0.115.14 | 2025-06-26 | —（版本号发布记录）                            |
| 0.115.13 | 2025-06-17 | —（版本号发布记录）                            |
| 0.115.12 | 2025-03-23 | —（版本号发布记录）                            |
| 0.115.6  | 2024-12-03 | —（版本号发布记录）                            |
| 0.115.0  | 2024-09-17 | —（版本号发布记录）                            |
| 0.114.0  | 2024-09-06 | —（版本号发布记录）                            |
| 0.112.0  | 2024-08-02 | —（版本号发布记录）                            |
| 0.95.0   | —          | 增加 `Annotated` 支持，提升依赖注入与静态类型工具的兼容性   |

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

## Python 版本演进 & FastAPI 改动

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

## FastAPI 应用开发核心关注点
**FastAPI 开发核心**
- 生命周期管理
- - 启动和关闭事件
- - lifespan API
- - 资源清理
- 配置管理
- - 环境变量
- - Pydantic Settings
- - 多环境配置
- 依赖注入
- - Depends()
- - 解耦逻辑
- - 共享资源
- 异步与并发
- - asyncio
- - 协程
- - BackgroundTasks
- - WebSocket 实时通信
- 数据库管理
- - 连接池
- - 事务
- - ORM 会话生命周期
- 异常与错误处理
- - @app.exception_handler
- - 自定义 HTTPException
- - 日志记录
- 中间件
- - 请求日志
- - 权限验证
- - 缓存处理
- 状态管理
- - app.state
- - request.state
- 任务调度
- - FastAPI BackgroundTasks
- - Celery / RQ
- - APScheduler 定时任务
- 安全与认证
- - JWT
- - OAuth2
- - API Key
- 健康检查与监控
- - /health 路由
- - Prometheus
- - OpenTelemetry

## FastAPI 语法知识全景指南（基础 → 进阶 → 高级）

---
### 1. 基础语法
#### 1.1 创建应用
```
#python

from fastapi import FastAPI

app = FastAPI(title="My API", version="1.0.0")


title、version、description 会显示在自动生成的 API 文档中。
```
#### 1.2 定义路由
**GET 路由**
```
@app.get("/")
def read_root():
    return {"message": "Hello FastAPI"}
```
**带路径参数**
```
@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id}
```
类型注解会自动进行校验和类型转换。

#### 1.3 查询参数
```
@app.get("/search/")
def search(q: str = None, limit: int = 10):
    return {"q": q, "limit": limit}
```
* q: str = None → 可选参数

* limit: int = 10 → 默认值

#### 1.4 请求体（Pydantic 模型）
```
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float
    in_stock: bool = True

@app.post("/items/")
def create_item(item: Item):
    return {"item": item}
```

* 自动进行 JSON 解析与类型验证。

#### 1.5 自动文档

* Swagger UI: /docs

* ReDoc: /redoc

### 2. 进阶语法
#### 2.1 路径与查询混合参数
```
@app.get("/users/{user_id}")
def get_user(user_id: int, active: bool = True):
    return {"user_id": user_id, "active": active}
```
#### 2.2 响应模型
```
class ItemOut(BaseModel):
    name: str
    price: float

@app.get("/items/{item_id}", response_model=ItemOut)
def read_item(item_id: int):
    return {"name": "Apple", "price": 3.5, "in_stock": True}
```

- response_model 会自动过滤掉多余字段（如 in_stock）。

#### 2.3 状态码
```
from fastapi import status

@app.post("/login/", status_code=status.HTTP_201_CREATED)
def login():
    return {"message": "Created"}
```
#### 2.4 异步处理
```
@app.get("/async-data/")
async def get_data():
    return {"data": "This is async"}
```

- 使用 async def 可以直接调用异步 I/O 操作。

#### 2.5 依赖注入
```
from fastapi import Depends

def common_params(q: str = None, limit: int = 10):
    return {"q": q, "limit": limit}

@app.get("/search/")
def search(params: dict = Depends(common_params)):
    return params
```
#### 2.6 路由分组（APIRouter）
```
from fastapi import APIRouter

router = APIRouter(prefix="/items", tags=["items"])

@router.get("/")
def list_items():
    return [{"id": 1, "name": "Apple"}]

app.include_router(router)
```
#### 2.7 中间件
```
from fastapi import Request
from starlette.middleware.base import BaseHTTPMiddleware

@app.middleware("http")
async def log_requests(request: Request, call_next):
    print(f"Request: {request.url}")
    response = await call_next(request)
    return response
```
#### 2.8 异常处理
```
from fastapi import HTTPException

@app.get("/error/")
def raise_error():
    raise HTTPException(status_code=404, detail="Item not found")
```
### 3. 高级语法
#### 3.1 OAuth2 + JWT 认证
```
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

@app.get("/users/me")
def read_users_me(token: str = Depends(oauth2_scheme)):
    return {"token": token}
```

- tokenUrl="token" 用于自动文档 UI 的“Authorize”按钮。

#### 3.2 WebSocket
```
from fastapi import WebSocket

@app.websocket("/ws")
async def websocket_endpoint(ws: WebSocket):
    await ws.accept()
    while True:
        data = await ws.receive_text()
        await ws.send_text(f"You said: {data}")
```
#### 3.3 lifespan 生命周期管理（替代 startup/shutdown）
- lifespan 是什么？
- - lifespan 是 FastAPI ≥ 0.95 引入的一种新的生命周期管理方式，用于替代 @app.on_event("startup") 和 @app.on_event("shutdown")。它使用 异步上下文管理器（@asynccontextmanager） 来统一管理应用启动与关闭过程

* 推荐使用 lifespan 管理应用生命周期

* 用于数据库初始化、Redis/WebSocket 启动、后台任务管理等
```
from contextlib import asynccontextmanager
from fastapi import FastAPI

@asynccontextmanager
async def lifespan(app: FastAPI):
    # 启动前逻辑
    db = await init_db()
    app.state.db = db
    yield
    # 关闭时逻辑
    await db.close()

app = FastAPI(lifespan=lifespan)
```
#### 3.4 后台任务
```
from fastapi import BackgroundTasks

def write_log(message: str):
    with open("log.txt", "a") as f:
        f.write(message + "\n")

@app.post("/send/")
def send_email(background_tasks: BackgroundTasks):
    background_tasks.add_task(write_log, "Email sent")
    return {"message": "Task scheduled"}
```
#### 3.5 文件上传
```
from fastapi import File, UploadFile

@app.post("/uploadfile/")
async def upload_file(file: UploadFile = File(...)):
    contents = await file.read()
    return {"filename": file.filename, "size": len(contents)}
```
#### 3.6 异步数据库（示例：SQLAlchemy + asyncpg）
```
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "postgresql+asyncpg://user:pass@localhost/dbname"

engine = create_async_engine(DATABASE_URL, echo=True)
SessionLocal = sessionmaker(bind=engine, class_=AsyncSession, expire_on_commit=False)

async def get_db():
    async with SessionLocal() as session:
        yield session

@app.get("/users/")
async def list_users(db: AsyncSession = Depends(get_db)):
    result = await db.execute("SELECT * FROM users")
    return result.fetchall()
```
#### 3.7 自定义 OpenAPI 文档
```
from fastapi.openapi.utils import get_openapi

def custom_openapi():
    if app.openapi_schema:
        return app.openapi_schema
    openapi_schema = get_openapi(
        title="Custom API",
        version="1.0.0",
        description="This is a custom OpenAPI schema",
        routes=app.routes,
    )
    app.openapi_schema = openapi_schema
    return app.openapi_schema

app.openapi = custom_openapi
```