# Docker 工作流指南
（适用于 QuantoraAquila 量化交易项目）

**当前项目背景**（基于 2026 年 1 月 10 日对话）：

- 主语言：Python + asyncio
- 核心组件：OKX WebSocket 实时采集 mark-price、Redis 缓存最新价格、PostgreSQL + TimescaleDB 存储历史数据
- 目标：开发调试 → 本地测试 → 实盘部署（稳定、低延迟、数据持久化）
- 部署环境：Windows（Docker Desktop + WSL2），数据目录统一放在 D 盘

本文档提供完整、可复制的 Docker 工作流，从开发到实盘的全流程。
## 1. 项目目录结构推荐

    QuantoraAquila/
    ├── trade-system-backend/
    │   ├── dataFetchAndStorge/          # 数据采集与存储代码
    │   │   ├── rest_api_data.py
    │   │   └── ...                     # 你的其他脚本
    │   ├── Dockerfile                   # 交易脚本镜像构建文件
    │   ├── requirements.txt             # Python 依赖
    │   ├── .env                         # 环境变量（密钥、DB密码等）
    │   └── docker-compose.yml           # 一键启动全套服务
    ├── DockerData/                      # D 盘数据持久化目录（手动创建）
    │   ├── redis-data/                  # Redis 数据
    │   └── pgdata/                      # PostgreSQL/TimescaleDB 数据
    └── README.md
## 2. 准备工作（只需做一次）

### 创建 D 盘数据目录（防止 C 盘爆满）
在命令提示符或 PowerShell 中运行：

    mkdir D:\EarnMoney\QuantoraAquila\DockerData\redis-data
    mkdir D:\EarnMoney\QuantoraAquila\DockerData\pgdata
### 生成 requirements.txt（在项目根目录终端运行）

    pip freeze > requirements.txt
### 创建 .env 文件（放敏感信息，不要提交到 git）

    # .env
    OKX_API_KEY=xxxx
    OKX_SECRET_KEY=xxxx
    OKX_PASSPHRASE=xxxx
    
    postgresql_host=localhost
    postgresql_port=5432
    postgresql_user=postgres
    postgresql_password=你的强密码123
    postgresql_database=quantora

## 3. Dockerfile（交易脚本镜像）
在项目根目录创建 Dockerfile：

    # 使用轻量 Python 镜像
    FROM python:3.11-slim
    
    # 设置工作目录
    WORKDIR /app
    
    # 先复制依赖，加速构建缓存
    COPY requirements.txt .
    RUN pip install --no-cache-dir -r requirements.txt
    
    # 复制所有代码
    COPY . .
    
    # 启动命令（根据你的主脚本路径调整）
    CMD ["python", "dataFetchAndStorge/rest_api_data.py"]
## 4. docker-compose.yml（一键启动全套服务）
在项目根目录创建 docker-compose.yml：

    version: '3.8'
    
    services:
      redis:
        image: redis:latest
        container_name: quantora-redis
        ports:
          - "6379:6379"
        volumes:
          - D:/EarnMoney/QuantoraAquila/DockerData/redis-data:/data
        restart: unless-stopped
        command: redis-server --appendonly yes --save 60 1000  # 每60秒至少1000次写操作快照
    
      timescaledb:
        image: timescale/timescaledb:latest-pg16
        container_name: quantora-tsdb
        ports:
          - "5432:5432"
        environment:
          POSTGRES_PASSWORD: ${POSTGRESQL_PASSWORD}
          POSTGRES_DB: ${POSTGRESQL_DATABASE}
        volumes:
          - D:/EarnMoney/QuantoraAquila/DockerData/pgdata:/var/lib/postgresql/data
        restart: unless-stopped
    
      trading-script:
        build: .                           # 使用当前目录的 Dockerfile
        container_name: quantora-script
        depends_on:
          - redis
          - timescaledb
        env_file:
          - .env                           # 加载所有环境变量
        restart: unless-stopped
    
    volumes:
      # 如果想改用 Docker 命名卷，可在此定义，但推荐绝对路径
## 5. 常用 Docker 工作流命令

- 构建镜像```docker-compose build```
- 启动全部服务（后台）```docker-compose up -d```
- 启动并构建（首次或代码变更时）```docker-compose up -d --build```
- 查看实时日志```docker-compose logs -f trading-script```\
或查看特定服务：```docker-compose logs -f quantora-tsdb```
- 重启某个服务```docker-compose restart trading-script```
- 停止并删除容器```docker-compose down```\
（数据因卷挂载不会丢失）
- 进入数据库容器调试```docker exec -it quantora-tsdb psql -U postgres -d quantora```
- 查看所有容器状态```docker ps```
- 清理无用资源（谨慎使用）\
```docker system prune -f```

## 6. 推荐开发 → 实盘切换流程

### 开发/调试阶段
- PyCharm 直接运行脚本（连接 localhost:6379 和 localhost:5432）
- Docker 只跑 Redis 和 TimescaleDB：


    docker-compose up -d redis timescaledb

### 测试阶段
- 启动全部服务：

    
    docker-compose up -d --build
- 查看日志、测试信号生成、下单

### 实盘部署
- 在 VPS 或云服务器上克隆项目
- 复制 .env、Dockerfile、docker-compose.yml
- 执行：

    
    docker-compose up -d --build
- 监控日志：docker-compose logs -f

### 数据备份
- 定期复制 D 盘的 DockerData 文件夹（redis-data 和 pgdata）
- 数据库备份示例：

    
    docker exec quantora-tsdb pg_dump -U postgres quantora > backup_$(date +%Y%m%d).sql


## 7. 常见问题快速解决

- 端口冲突\
修改 ports 为 "6380:6379" 或 "5433:5432"
- 容器启动失败\
查看日志：docker logs quantora-tsdb
- 数据库连接不上\
确认 .env 中密码与容器启动密码一致
- C 盘占用过高\
确保所有 volumes 都使用 D 盘绝对路径
- 脚本重启后数据丢失\
检查是否正确挂载了卷（-v 参数）