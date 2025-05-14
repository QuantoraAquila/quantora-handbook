# 🖥️ CMD 与 PowerShell 常用命令对照表（含解释与英文含义）

本文件收录了 Windows CMD 和 PowerShell 的常用命令，每条命令附带功能说明与英文含义，方便学习与对照查阅。

---

## 📁 CMD 常用命令

| 命令             | 英文含义                     | 功能解释                               |
| -------------- | ------------------------ | ---------------------------------- |
| `dir`          | directory                | 显示当前目录下的文件和文件夹列表                   |
| `cd`           | change directory         | 切换目录，如：`cd D:\Projects`            |
| `cls`          | clear screen             | 清屏，清除当前命令窗口的内容                     |
| `copy`         | copy file                | 复制文件，如：`copy file1.txt backup\`    |
| `del`          | delete                   | 删除文件，如：`del file.txt`              |
| `move`         | move file                | 移动文件或重命名，如：`move file.txt folder\` |
| `mkdir` / `md` | make directory           | 创建新文件夹，如：`mkdir new_folder`        |
| `rmdir` / `rd` | remove directory         | 删除文件夹，如：`rmdir folder_name`（空文件夹）  |
| `echo`         | echo (repeat output)     | 打印信息到屏幕，也用于设置变量，如：`echo Hello`     |
| `set`          | set environment variable | 设置环境变量，如：`set PATH=C:\NewPath`     |
| `path`         | path variable            | 查看或设置 PATH 环境变量                    |
| `where`        | where executable is      | 查找命令或可执行文件所在路径，如：`where node`      |
| `exit`         | exit                     | 退出命令行窗口                            |

---

## ⚡ PowerShell 常用命令

| 命令                      | 英文含义     | 功能解释                                           |
| ----------------------- | -------- | ---------------------------------------------- |
| `Get-Location`          | 获取位置     | 显示当前所在路径（等同于 CMD 中的 `cd`）                      |
| `Set-Location`          | 设置位置     | 切换目录（等同于 `cd`），如：`Set-Location D:\Projects`    |
| `Get-ChildItem`         | 获取子项     | 列出目录中的内容（等同于 `dir`）                            |
| `Clear-Host` (`cls`)    | 清除主机     | 清除屏幕内容                                         |
| `Copy-Item`             | 复制项目     | 复制文件或文件夹，如：`Copy-Item a.txt b.txt`             |
| `Remove-Item`           | 删除项目     | 删除文件或文件夹，如：`Remove-Item a.txt`                 |
| `Move-Item`             | 移动项目     | 移动或重命名文件，如：`Move-Item a.txt b.txt`             |
| `New-Item`              | 新建项目     | 创建新文件或文件夹，如：`New-Item -ItemType File test.txt` |
| `Test-Path`             | 测试路径     | 检查路径是否存在，如：`Test-Path .\data`                  |
| `Write-Output` / `echo` | 写入输出     | 打印信息，如：`Write-Output "Hello"`                  |
| `$env:PATH`             | 环境变量     | 查看当前 PowerShell 会话的 PATH 变量                    |
| `$PROFILE`              | 用户配置文件路径 | 查看当前用户配置文件路径，可用于初始化脚本                          |
| `notepad $PROFILE`      | 打开配置文件   | 用记事本打开 PowerShell 用户配置文件                       |
| `Start-Process`         | 启动进程     | 启动程序，如：`Start-Process notepad.exe`             |
| `Get-Command`           | 获取命令     | 查看命令是否存在或查看其路径，如：`Get-Command node`            |
| `Get-Help`              | 获取帮助     | 查看命令帮助信息，如：`Get-Help Copy-Item`                |
| `exit`                  | 退出       | 退出 PowerShell 会话                               |

---

## ✅ 补充说明

* **CMD** 是传统的 Windows 命令行环境，语法接近 MS-DOS。
* **PowerShell** 是现代的命令行环境，支持对象管道和脚本语言。
* 大多数 CMD 命令在 PowerShell 中仍然可用，但建议使用 PowerShell 原生命令（即 cmdlet）。

如需补充 `Git`、`Node.js`、`pnpm` 等开发环境命令，也可以继续扩展在此文档中。
