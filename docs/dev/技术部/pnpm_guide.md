# pnpm 常用命令

## 1. `pnpm init`
- **功能**: 初始化一个新的 `package.json` 文件，类似于 `npm init`，但速度更快。

## 2. `pnpm install` 或 `pnpm i`
- **功能**: 安装项目中的所有依赖项，依赖管理比 `npm` 更高效，使用硬链接来减少磁盘空间的使用。

## 3. `pnpm add [package]`
- **功能**: 将指定的包添加到项目中并安装。例如：`pnpm add lodash`。

## 4. `pnpm add [package] --dev`
- **功能**: 将指定的包添加到开发依赖中。例如：`pnpm add typescript --dev`。

## 5. `pnpm remove [package]`
- **功能**: 从项目中移除指定的包。例如：`pnpm remove lodash`。

## 6. `pnpm update`
- **功能**: 更新项目中的所有依赖项到最新的符合 `package.json` 版本要求的版本。

## 7. `pnpm list` 或 `pnpm ls`
- **功能**: 显示当前项目中安装的所有依赖包及其版本。

## 8. `pnpm outdated`
- **功能**: 检查项目中安装的依赖项是否有新版本。

## 9. `pnpm install [package]@[version]`
- **功能**: 安装指定版本的依赖项。例如：`pnpm install lodash@4.17.21`。

## 10. `pnpm audit`
- **功能**: 执行安全性检查，查看项目中依赖包的已知安全漏洞。

## 11. `pnpm run [script]`
- **功能**: 执行在 `package.json` 中定义的脚本命令。例如：`pnpm run build`。

## 12. `pnpm publish`
- **功能**: 将本地包发布到 npm 仓库。

## 13. `pnpm bin`
- **功能**: 显示安装的二进制文件的位置。

## 14. `pnpm root`
- **功能**: 显示当前项目的根目录路径。

## 15. `pnpm link [package]`
- **功能**: 将一个全局包链接到本地项目。

## 16. `pnpm store`
- **功能**: 管理 pnpm 缓存的存储位置。

## 17. `pnpm config`
- **功能**: 管理 pnpm 配置项，查看或修改设置。

## 18. `pnpm prune`
- **功能**: 删除项目中未使用的依赖包，确保依赖树保持干净。

## 19. `pnpm set`
- **功能**: 设置全局的配置项。例如：`pnpm set store-dir /path/to/dir`。

## 20. `pnpm help`
- **功能**: 查看 `pnpm` 的帮助信息，列出所有可用命令及其用法。
