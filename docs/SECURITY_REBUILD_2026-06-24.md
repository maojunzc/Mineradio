# Mineradio Security Rebuild Log — 2026-06-24

## Context

- v1.1.0 是纯净安装安全重建版本。构建和发布应仅从当前可信源码进行，不得使用旧的打包产物。

## Current Source Boundary

- 任何生成的 `dist/` 输出必须来自当前源码树，上传前进行扫描。

## Update Fix Boundary

- 软件更新失败应停在明确的错误状态。
- 快速补丁失败后不得自动开始完整安装包下载。
- 完整安装包下载完成后不得自动打开安装器。
- 已验证过的已下载安装包可复用，避免重复下载。

## Before Any Installer Release

- 从当前 Git-tracked 源码构建。
- 不要复用旧 `dist/`、旧安装包、旧 `node_modules` 或旧备份包。
- 运行语法检查和安全扫描。
- 优先代码签名后再发布安装包。
