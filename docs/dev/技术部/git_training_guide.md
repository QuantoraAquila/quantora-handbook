# Git 协作开发培训资料

## 一、上传前的标准操作流程（本地已有仓库）

```bash
git status             # 1. 查看哪些文件改动了
git diff               # 2. 可选：查看改动的具体内容
git add .              # 3. 添加所有改动文件到暂存区
git commit -m "说明"   # 4. 提交到本地 Git 版本记录
git push               # 5. 上传改动到远程 GitHub 仓库
git ls-files           # 6. 查看当前已被 Git 跟踪的文件列表（可选）
```

> 💡 **建议：添加 `.gitignore` 文件忽略本地环境配置文件**
>
> 在项目根目录创建 `.gitignore` 文件，添加以下内容忽略 IDE 文件夹（如 `.idea/`）：
>
> ```bash
> .idea/
> *.log
> __pycache__/
> *.pyc
> .DS_Store
> ```
>
> 若某些文件已被 Git 跟踪，可执行 `git rm -r --cached .idea` 将其从版本控制中移除，再提交。

---

## 二、首次参与项目开发（克隆 + 本地开发）

### 1. 克隆远程仓库（只做一次）

```bash
git clone https://github.com/QuantoraOrg/project-name.git
```

### 2. 进入项目目录

```bash
cd project-name
```

### 3. 创建开发分支（推荐）

```bash
git checkout develop           # 确保从 develop 分支出发
git checkout -b feature/xxx    # 基于 develop 创建功能分支
```

> 📌 `git checkout -b` 命令会基于当前所在分支创建新分支，注意确保起点正确。

---

## 三、日常协作开发循环流程

```bash
git pull                # 拉取远程最新代码
git checkout -b xxx     # 开新分支（如未创建）
# 本地修改代码...
git add .
git commit -m "说明"
git push origin xxx     # 推送当前分支
```

---

## 四、多人协作建议

| 操作命令                  | 说明                    |
| --------------------- | --------------------- |
| `git pull`            | 每次开发前先拉远程代码，避免代码冲突    |
| `git checkout -b xxx` | 创建新功能分支，避免污染主分支       |
| `git push origin xxx` | 推送当前分支到 GitHub        |
| GitHub Pull Request   | 功能完成后发起合并请求，由主分支维护者审批 |

---

## 五、推荐分支命名规范

* `feature/xxx`：开发新功能
* `fix/xxx`：修复 bug
* `hotfix/xxx`：紧急修复
* `docs/xxx`：文档更新
* `test/xxx`：测试相关代码

---

## 六、协作流程图

```
   GitHub 仓库 ← (git clone)
        ↓
   本地开发环境 (VSCode / PyCharm)
        ↓
   git pull ← 获取最新代码
        ↓
   git checkout -b xxx ← 开新分支
        ↓
   编码 & 测试
        ↓
   git add .
   git commit -m "说明"
        ↓
   git push origin xxx ← 推送分支
        ↓
   GitHub 发起 Pull Request / 审核合并
```

---

## 七、补充建议

* 所有协作者需具备 GitHub 账号，加入公司组织
* 推荐使用 VS Code、GitHub Desktop、命令行结合使用
* 初期可安排一名技术负责人进行代码审核和合并管理
* Git 不会跟踪空文件夹，若需保留空目录可添加 `.gitkeep` 文件（空文本文件）
* 可使用 `git ls-files` 命令查看当前被 Git 跟踪的所有文件列表
* 合并分支时，若目标分支尚无提交，可触发 Fast-forward 合并（主分支直接推进到开发分支），不会产生冲突；否则需逐个处理冲突文件

---

如需配套视频教程、命令速查表、或可视化图解，请联系技术负责人安排。
