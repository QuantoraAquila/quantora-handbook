# Docker 使用指南（2026 年版）
Docker 是现代开发者尤其是量化交易、自动化脚本部署的必备工具。它可以将你的应用、数据库、依赖环境打包成容器，实现“一次构建，到处运行”。本文从零基础到进阶实战，适合个人短线交易员、量化开发者使用。
# 一、Docker 是什么？
Docker 是一个开源容器化平台，将应用程序及其所有依赖打包成一个轻量级、可移植的容器。
核心优势：

- 环境一致：本地开发、服务器部署完全相同，避免“在我电脑上能跑”。
- 启动快：秒级启动复杂系统（Redis + TimescaleDB + 交易机器人）。
- 资源省：比虚拟机轻量，适合个人电脑/VPS。
- 易管理：用一条命令启动/停止/备份整个系统。
# 二、安装 Docker
Windows & Mac（推荐 Docker Desktop）

- 官网下载：https://www.docker.com/products/docker-desktop/
- - Apple M 系列 Mac → 自动 ARM64 版
- - Intel Mac / Windows → AMD64 版

- 安装后启动 Docker Desktop（小鲸鱼图标常驻托盘）。

常见问题修复（Windows）：

    PowerShell# 以管理员运行 PowerShell
    wsl --update
    # 重启电脑后再开 Docker Desktop
Linux（Ubuntu 示例）

    Bashsudo apt update
    sudo apt install docker.io
    sudo usermod -aG docker $USER
    newgrp docker
验证安装：

    Bashdocker --version
    docker run hello-world
# 三、常用命令
## 1、Docker 基础命令

| 功能 | 命令 |
|----|----|
| 查看 Docker 版本 | `docker --version` |
| 查看 Docker 运行状态 | `docker info` |
| 查看本地镜像 | `docker images` |
| 拉取镜像 | `docker pull redis:7` |
| 删除镜像 | `docker rmi <image_id>` |
| 清理无用镜像 | `docker image prune` |

---

## 2、容器管理（Container）

| 功能 | 命令 |
|----|----|
| 查看运行中容器 | `docker ps` |
| 查看全部容器 | `docker ps -a` |
| 启动容器 | `docker start <container>` |
| 停止容器 | `docker stop <container>` |
| 重启容器 | `docker restart <container>` |
| 删除容器 | `docker rm <container>` |
| 强制删除容器 | `docker rm -f <container>` |

---

## 3、进入容器 & 日志（高频）

| 功能 | 命令 |
|----|----|
| 进入容器（bash） | `docker exec -it <container> bash` |
| 进入容器（sh） | `docker exec -it <container> sh` |
| 查看容器日志 | `docker logs <container>` |
| 实时日志 | `docker logs -f <container>` |

---

## 4、Docker Compose（你最常用）

| 功能 | 命令 |
|----|----|
| 启动服务 | `docker compose up -d` |
| 重新构建并启动 | `docker compose up -d --build` |
| 停止并删除容器 | `docker compose down` |
| 停止并删除容器+数据 | `docker compose down -v` |
| 查看服务状态 | `docker compose ps` |
| 查看服务日志 | `docker compose logs api` |
| 实时查看日志 | `docker compose logs -f` |

---

## 5、数据卷（Redis / TimescaleDB）

| 功能 | 命令 |
|----|----|
| 查看数据卷 | `docker volume ls` |
| 查看数据卷详情 | `docker volume inspect <volume>` |
| 删除数据卷 | `docker volume rm <volume>` |
| 清理未使用数据卷 | `docker volume prune` |

---

## 6、网络（Docker Network）

| 功能 | 命令 |
|----|----|
| 查看网络 | `docker network ls` |
| 查看网络详情 | `docker network inspect <network>` |

---

## 7、资源监控（生产必看）

| 功能 | 命令 |
|----|----|
| 实时资源使用 | `docker stats` |
| 查看磁盘占用 | `docker system df` |

---

## 8、镜像构建 & 运行

| 功能 | 命令 |
|----|----|
| 构建镜像 | `docker build -t quantora-api ./server` |
| 运行镜像 | `docker run -it -p 8000:8000 quantora-api` |

---

## 9、清理命令（磁盘管理）

| 功能 | 命令 |
|----|----|
| 清理未使用资源 | `docker system prune` |
| 清理所有未使用资源 | `docker system prune -a` |

---

## 10、Redis / PostgreSQL 专用

| 服务 | 功能 | 命令 |
|----|----|----|
| Redis | 进入 CLI | `docker exec -it quantora-redis redis-cli` |
| TimescaleDB | 进入 psql | `docker exec -it quantora-timescaledb psql -U quantora -d quantora` |

---

## 11、 交易系统日常必会（TOP 8）

| 场景 | 命令 |
|----|----|
| 查看容器状态 | `docker ps` |
| 重新部署 | `docker compose up -d --build` |
| 看 API 日志 | `docker compose logs -f api` |
| 进入后端容器 | `docker exec -it quantora-api bash` |
| 查看 Redis | `docker exec -it quantora-redis redis-cli` |
| 查看数据库 | `docker exec -it quantora-timescaledb psql -U quantora` |
| 查看资源占用 | `docker stats` |
| 清理垃圾 | `docker system prune` |

# 四、进阶用法
## 1、 数据持久化（Volume）
容器删除数据会丢失，使用卷持久化。

    Bash# 创建命名卷
    docker volume create tsdb_data
    
    # 使用卷
    docker run -d --name tsdb \
      -v tsdb_data:/var/lib/postgresql/data \
      -p 5432:5432 \
      -e POSTGRES_PASSWORD=123456 \
      timescale/timescaledb:latest-pg16
## 2、自定义镜像（Dockerfile）
项目根目录创建 Dockerfile：

    dockerfileFROM python:3.11-slim
    
    WORKDIR /app
    
    COPY requirements.txt .
    RUN pip install --no-cache-dir -r requirements.txt
    
    COPY . .
    
    CMD ["python", "main.py"]
构建与运行：

    Bashdocker build -t my-trading-bot:latest .
    docker run -d --name bot \
      --restart unless-stopped \
      my-trading-bot:latest
## 3、 docker-compose：一键启动全家桶（强烈推荐！）
项目根目录创建 docker-compose.yml：

    YAMLversion: '3.8'
    
    services:
      redis:
        image: redis:latest
        ports:
          - "6379:6379"
        restart: unless-stopped
    
      timescaledb:
        image: timescale/timescaledb:latest-pg16
        ports:
          - "5432:5432"
        environment:
          POSTGRES_PASSWORD: yourpass123
        volumes:
          - tsdb_data:/var/lib/postgresql/data
        restart: unless-stopped
    
      trading-bot:
        build: .                        # 使用当前目录 Dockerfile
        depends_on:
          - redis
          - timescaledb
        environment:
          - REDIS_HOST=redis
          - DB_HOST=timescaledb
          - OKX_API_KEY=your_key
        restart: unless-stopped
    
    volumes:
      tsdb_data:
常用命令：

    Bashdocker-compose up -d        # 后台启动所有服务
    docker-compose down         # 停止并删除容器
    docker-compose logs -f      # 查看实时日志
    docker-compose restart      # 重启所有服务
# 五、最佳实践（量化交易专用）

- 实盘脚本加 ```--restart unless-stopped```，崩溃自动重启。
- 敏感信息（如 API Key）用环境变量传入，不要写死在代码里。
- 定期备份卷数据：
```
Bashdocker volume ls
# 备份示例：复制卷到主机目录
```
- 清理无用资源：
```
Bashdocker system prune    # 删除停止容器、无用网络、悬空镜像
```
