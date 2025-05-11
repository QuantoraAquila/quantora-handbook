# Git 分支管理与部署流程指南

## 一、Git 分支管理规范（推荐使用 Git Flow）

### 📌 Git Flow 分支结构

```
main           -> 主分支（仅包含稳定版本）
develop        -> 开发主分支（功能合并汇总）
feature/*      -> 功能开发分支（从 develop 分出）
release/*      -> 发布准备分支（从 develop 分出）
hotfix/*       -> 热修复分支（从 main 分出）
```

### ✅ 使用建议

* 所有开发应在 `feature/*` 分支中进行，如 `feature/auth-login`
* 合并进 `develop` 前需进行 Pull Request（PR）审核
* 发布新版本时从 `develop` 创建 `release/v1.0.0` 分支，完成后合并至 `main` 和 `develop`
* 发现线上 bug，快速从 `main` 创建 `hotfix/*` 分支修复，并合并回 `main` 和 `develop`

---

## 🧩 通俗理解 Git Flow 分支策略：像“做披萨”的工厂

> 想象你的代码就像一个披萨工厂的流水线：

* `main`：出货区，只放能交付客户的“最终披萨”
* `develop`：中央厨房，在这里汇总各种配料并进行试吃测试
* `feature/*`：配料工作台，开发新功能（如加培根、加芝士）
* `release/*`：质检台，出货前仔细检查味道、外形
* `hotfix/*`：紧急修复区，客户投诉后马上调整问题披萨

### 🛠️ 工作流程：

1. 开发新功能 → 从 `develop` 分出 `feature/login`，做完后合并回 `develop`
2. 准备发布版本 → 从 `develop` 分出 `release/1.0.0`，修修补补，准备上线
3. 发布上线 → `release` 合并到 `main`，也同步合并到 `develop`
4. 上线后发现问题 → 从 `main` 分出 `hotfix/fix-bug`，修完后合并回 `main` 和 `develop`

### 🎯 好处：

* 每个人有明确分工（你做芝士我做番茄）
* 每个版本上线前都有检修
* 有 bug 可以随时抢修，不耽误主线开发

---

## 二、分支操作流程示例

### 创建功能分支

```bash
git checkout develop
git pull

# 创建新功能分支
git checkout -b feature/awesome-feature
```

### 完成功能后发起 PR

```bash
git add .
git commit -m "feat: add awesome feature"
git push origin feature/awesome-feature
```

然后在 GitHub 上创建 Pull Request，选择合并至 `develop`，并由同事进行代码审核。

---

## 三、部署流程（以前后端项目为例）

### 📦 前端项目部署（如 Vite/React/Next.js）

#### 本地打包

```bash
npm run build
```

#### 自动部署推荐

* 使用 GitHub Actions + Vercel / Netlify / GitHub Pages 自动部署
* 主分支或 `release/*` 分支变动即触发构建发布

#### GitHub Actions 示例

```yaml
# .github/workflows/deploy.yml
name: Deploy Frontend

on:
  push:
    branches: [main, release/**]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm ci
      - run: npm run build
      - run: echo "可接入 Vercel CLI 或上传至服务器"
```

### 🖥 后端部署（如 FastAPI / Django）

#### 推荐部署平台

* 云服务（如 Railway, Render, Fly.io）
* 自建服务器（使用 Docker + nginx + systemd）

#### 自动化部署建议

* 使用 GitHub Actions 构建 Docker 镜像并部署
* 可配置 Secrets 存储 SSH 密钥、API Token

---

## 四、代码评审（Code Review）

### PR 规范建议：

* 明确标题，例如：`feat(api): 新增订单查询接口`
* 添加简要描述说明变更目的
* 关联 Issue，例如：`Closes #23`
* 至少一位成员代码审核通过后方可合并

---

## 五、总结建议

| 项目阶段 | 分支类型       | 说明         |
| ---- | ---------- | ---------- |
| 开发中  | feature/\* | 每个功能一个分支   |
| 集成阶段 | develop    | 汇总所有功能并测试  |
| 发布前  | release/\* | 准备版本，修复小问题 |
| 上线后  | main       | 保存正式发布代码   |
| 紧急修复 | hotfix/\*  | 线上出问题立即修复  |

如需结合您项目结构制定具体的部署脚本和 CI/CD 流程，请提交系统架构设计请求给技术负责人。
