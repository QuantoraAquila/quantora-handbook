# Git Flow 常用术语缩写与对照表

| 缩写/名称             | 全称（英文）                                         | 中文释义        | 用途说明                   |
| ----------------- | ---------------------------------------------- | ----------- | ---------------------- |
| `dev` / `develop` | development                                    | 开发分支        | 整合所有功能分支，用于开发测试        |
| `main` / `master` | main (or master)                               | 主分支 / 主线版本  | 发布稳定版本的主干              |
| `feat`            | feature                                        | 新功能         | Git 提交信息中常见前缀，用于新增功能说明 |
| `fix`             | fix                                            | 修复 bug      | Git 提交信息前缀，表示修复缺陷      |
| `hotfix/*`        | hotfix                                         | 热修复分支       | 针对生产环境紧急修复的问题          |
| `release/*`       | release                                        | 发布准备分支      | 从 develop 分出，用于发布前修整版本 |
| `feature/*`       | feature                                        | 功能开发分支      | 开发新功能，从 develop 分出     |
| `PR`              | Pull Request                                   | 合并请求        | 提交代码变更供审查并准备合并入主分支     |
| `CI/CD`           | Continuous Integration / Continuous Deployment | 持续集成 / 持续部署 | 自动化测试、构建、部署流程          |
| `WIP`             | Work In Progress                               | 开发中         | PR 或提交中表示尚未完成的工作       |
| `LGTM`            | Looks Good To Me                               | 看起来没问题      | 审核代码时常用语，表示认可代码变更      |
| `TBD`             | To Be Determined                               | 待定          | 表示还未确认的项               |
| `README`          | Read Me                                        | 自述文件        | 项目说明文档，一般用于介绍项目用途和结构   |

> ✅ 以上缩写和结构建议统一用于提交说明、分支命名、文档中，保持整个团队的协作规范。

如需为您的项目创建标准化的提交信息模板（如 Conventional Commits），可继续提出需求。
