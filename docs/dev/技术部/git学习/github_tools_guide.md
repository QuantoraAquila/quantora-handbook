# GitHub 协作工具使用手册

## 一、GitHub Issues（问题追踪）

### 📌 作用

* 用于记录 Bug、功能需求、改进建议、讨论事项等。
* 可指定负责人、设置标签（labels）、里程碑（milestone）和截止日期。

### ✅ 使用建议

* 每个 Issue 只聚焦一项任务或问题。
* 使用模板（如 bug report、feature request）提升规范性。
* 使用标签（如 `bug`、`feature`、`urgent`）帮助筛选和管理。

### 🛠️ 常见用法

* 创建：点击仓库 > Issues > New Issue
* 指派：Assignees 选择相关开发者
* 讨论：评论区进行实时协作交流
* 关闭：任务完成后点击 Close issue

---

## 二、GitHub Projects（项目看板）

### 📌 作用

* 提供「看板式」任务管理（类似 Trello、Notion）。
* 支持规划迭代（Sprint）、版本发布、团队协作。

### ✅ 使用建议

* 每个项目使用一个 Project（如 `v1.0 开发计划`）。
* 看板常见结构：`To Do` / `In Progress` / `Done`
* 将 Issue 拖动进不同列管理工作状态。

### 🛠️ 常见用法

* 创建：仓库 > Projects > New Project
* 添加内容：可以链接 Issue、PR
* 分配：拖拽任务卡片即可管理状态

---

## 三、GitHub Actions（自动化工作流）

### 📌 作用

* 自动化测试、构建、部署、CI/CD。
* 触发条件：如 `push`、`pull request`、`release` 等。

### ✅ 使用建议

* 初期可配置基础 Lint、测试自动运行。
* 后期用于自动部署前端网站、发布版本、通知等。

### 🛠️ 常见用法

* 配置文件路径：`.github/workflows/` 目录下的 `.yml` 文件
* 常用工作流：

  * 自动运行 Python/JS 测试
  * 自动部署前端到 GitHub Pages / Vercel
  * 自动打标签发布

---

## 四、GitHub Releases 与 Tags（版本管理）

### 📌 作用

* Releases 用于正式发布版本（如 `v1.0.0`）。
* Tags 是具体某次提交的版本标识。

### ✅ 使用建议

* 遵循语义化版本管理（SemVer）：

  * `v1.0.0`：主版本号.次版本号.修订号
  * 大版本更新破坏兼容性，中版本增加功能，小版本修复 bug。

### 🛠️ 常见用法

* 创建 Tag：

```bash
git tag v1.0.0
git push origin v1.0.0
```

* 创建 Release：

  * GitHub 网页仓库 > Releases > Draft a new release
  * 填写 Tag、标题、说明，附带构建产物（如 zip 包）

---

## 五、综合协作流程建议

1. 项目负责人规划版本：创建 Project + Milestones
2. 将功能/任务拆分为 Issues，分配给技术人员
3. 技术人员处理任务，提交 PR 关联对应 Issue
4. 审核通过后合并 PR，自动执行 GitHub Actions
5. 发布阶段使用 Tag + Release 管理正式版本

---

## 📚 补充资源

* GitHub Issues 官方文档：[https://docs.github.com/en/issues](https://docs.github.com/en/issues)
* GitHub Projects 官方文档：[https://docs.github.com/en/issues/planning-and-tracking-with-projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects)
* GitHub Actions 官方文档：[https://docs.github.com/en/actions](https://docs.github.com/en/actions)
* 语义化版本：[https://semver.org/](https://semver.org/)

如需图解操作、团队实践模板，请联系技术负责人。
