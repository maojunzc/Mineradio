# Mineradio Project Rules

## Project Identity

Mineradio 是 Windows Electron 桌面音乐播放器，核心体验包括搜索、播放、歌单、歌词、3D 歌单架、粒子视觉预设、DIY 视觉控制台和 GitHub 自动更新。

## Repository Layout

```text
├─ public/
│  ├─ index.html        # 主 UI、CSS、歌词、粒子、3D 歌单架、视觉控制台
│  └─ vendor/           # 本地 vendor 依赖
├─ desktop/             # Electron main/preload
├─ build/               # 打包资源和 installer 脚本
├─ docs/                # 项目记忆、设计偏好、长期约束
├─ server.js            # 本地 API、音乐源、更新检查、补丁应用
├─ dj-analyzer.js       # 节奏/音频分析
├─ package.json         # 版本号、构建命令、electron-builder 配置
└─ CHANGELOG.md         # 更新说明
```

## Commands

```bash
npm start
node --check server.js
npm run build:win:dir
npm run build:win
```

改动后至少做：
```bash
git diff --check
node --check server.js
```

## Guardrails

- 不要随意重写 `public/index.html` 的大块视觉系统；先定位已有函数和状态。
- 不要动电影视觉系统，除非用户明确点名。
- 不要恢复旧的侧边栏闪烁、控制台播放暂停失效、3D 歌单架强制切回星河等问题。
- 不要把搜索结果、左侧歌单、3D 歌单架的性能优化做成一次性渲染全部内容。
- 不要把用户认可的玻璃质感改成普通毛玻璃或廉价透明面板。
