# 发布流程

## 发布前检查

- 确认 `package.json` 和 `package-lock.json` 版本号正确。
- 确认 `.cookie`、`.qq-cookie`、`updates/`、`node_modules/`、旧 `dist/` 没有进入 git。
- 运行语法检查：`git diff --check`、`node --check server.js`、前端内联脚本解析。
- 运行 Git 跟踪风险残留检查，确认没有跟踪 `.exe/.dll/.scr/.bat/.cmd/.ps1/.vbs/.jse/.wsf/.hta/.xlsm` 等可执行/脚本残留。
- 从当前源码执行 `npm run build:win` 生成 Windows 安装包。
- 对新生成的安装包和当前源码执行安全扫描。
- 生成并记录新安装包 SHA256。

## GitHub Release

Release tag：

```text
v<版本号>
```

Release 标题：

```text
Mineradio v<版本号>
```

建议上传资产：

- `dist/Mineradio-<版本号>-Setup.exe`
- `dist/Mineradio-<版本号>-Setup.exe.blockmap`（可选）
- `dist/Mineradio-<版本号>-SHA256SUMS.txt`

## 更新检测

应用会请求 GitHub Releases latest 检测新版本。

本地验证更新链路时，可以用临时 manifest：

```json
{
  "latestVersion": "1.1.1-test",
  "release": {
    "name": "Mineradio v1.1.1-test",
    "downloadUrl": "http://127.0.0.1:3144/Mineradio-1.1.1-Setup.exe",
    "notes": ["本地在线更新链路测试"]
  }
}
```
