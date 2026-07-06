# Mineradio Code Graph

> 项目代码图谱，生成于 2026-07-06。用于了解整体架构、模块关系和后续迭代参考。

---

## 1. 项目概览

| 指标 | 数值 |
|------|------|
| 总代码行数 | ~36,000（不含 `package-lock.json`） |
| 核心语言 | JavaScript + HTML + CSS（嵌入 HTML） |
| 运行时 | Node.js + Electron |
| 外部依赖 | `NeteaseCloudMusicApi`、`Three.js`、`GSAP`、`music-tempo` |
| 包管理 | npm / electron-builder (NSIS 打包) |

## 2. 文件结构与大小

```
Mineradio/
├── server.js              (4203 行)  ← 本地 HTTP API 服务
├── dj-analyzer.js         (864 行)  ← 音频节奏分析引擎
├── package.json           (96 行)   ← 项目配置 & electron-builder
│
├── public/                          ← 前端资源（Electron 加载）
│   ├── index.html          (26,879 行)  ← 主界面：UI / 3D / 歌词 / 全套逻辑
│   ├── desktop-lyrics.html (1,211 行)  ← 桌面独立歌词窗口
│   ├── wallpaper.html      (142 行)    ← 壁纸模式页面
│   ├── default-user-fx-archive.json   ← 首次启动默认视觉用户存档
│   ├── vendor/                        ← 第三方库 (Three.js, GSAP, music-tempo)
│   └── assets/                        ← 二进制资源 (骷髅点云 .bin)
│
├── desktop/                         ← Electron 主进程
│   ├── main.js             (1,466 行)  ← 主进程：窗口管理 / IPC / 热键
│   ├── preload.js          (52 行)     ← preload 桥接
│   └── overlay-preload.js  (19 行)     ← 歌词浮层 preload
│
├── build/                           ← 打包 & 安装器
│   ├── installer.nsh       (1,023 行)  ← NSIS 安装器脚本
│   ├── after-pack.js       (66 行)     ← electron-builder afterPack hook
│   └── icon.ico / .bmp 等              ← 应用图标 & 安装器视觉
│
└── docs/                             ← 项目文档
    ├── PROJECT_MEMORY.md             ← 项目记忆（偏好、边界）
    ├── 3D_PLAYLIST_SHELF_MEMORY.md   ← 3D 歌单架交互边界
    ├── GLASS_SVG_TEXTURE.md          ← SVG 玻璃质感基线
    ├── INSTALLER_STYLE.md            ← 安装器样式规范
    ├── DESKTOP_LYRICS_VISUAL.md      ← 桌面歌词视觉基线
    ├── QQ_MUSIC_INTERFACE_NOTES.md   ← QQ 音乐接口排障记录
    ├── HANDOFF_NEXT_CHAT.md          ← AI 交接指引
    ├── SECURITY_REBUILD_2026-06-24.md
    └── RELEASE_NOTES_v1.1.0.md
```

---

## 3. 后端服务 (`server.js`) — API 路由图

```
server.js 入口                      ← 依赖: NeteaseCloudMusicApi, dj-analyzer.js
│
├── 系统 API
│   ├── /api/app/version           → 返回版本、更新配置
│   ├── /favicon.ico               → 静态图标
│   └── /* (静态文件)               → 代理到 public/
│
├── 更新系统
│   ├── /api/update/latest         → 拉取 GitHub Release 最新版本
│   ├── /api/update/download       → 启动完整安装包下载任务
│   ├── /api/update/download/status → 下载进度查询
│   ├── /api/update/patch          → 启动快速补丁任务
│   └── /api/update/patch/status   → 补丁进度查询
│
├── 网易云音乐 API
│   ├── /api/search                → 关键词搜索歌曲
│   ├── /api/song/url              → 获取歌曲播放 URL（含试听检测）
│   ├── /api/lyric                 → 歌词（支持 yrc）
│   ├── /api/song/comments         → 歌曲评论
│   ├── /api/artist/detail         → 歌手主页+热门歌曲
│   ├── /api/playlist/tracks       → 歌单曲目列表
│   ├── /api/discover/home         → 首页推荐（每日/私人/精选）
│   ├── /api/weather/radio         → 天气电台生成
│   ├── /api/weather/ip-location   → 天气 IP 定位
│   │
│   ├── 登录系统
│   │   ├── /api/login/qr/key      → 获取 QR key
│   │   ├── /api/login/qr/create   → 生成二维码
│   │   ├── /api/login/qr/check    → 轮询扫码状态 (800/801/802/803)
│   │   ├── /api/login/cookie      → Cookie 登录
│   │   ├── /api/login/status      → 登录态查询
│   │   └── /api/logout            → 登出
│   │
│   ├── 用户相关
│   │   ├── /api/user/playlists    → 用户歌单列表
│   │   ├── /api/playlist/create   → 创建歌单
│   │   └── /api/playlist/add-song → 收藏歌曲到歌单
│   │
│   └── 红心
│       ├── /api/song/like/check   → 批量检查红心状态
│       └── /api/song/like         → 红心/取消红心
│
├── QQ 音乐 API
│   ├── /api/qq/search             → QQ 音乐搜索
│   ├── /api/qq/song/url           → QQ 歌曲播放 URL
│   ├── /api/qq/lyric              → QQ 歌词
│   ├── /api/qq/song/comments      → QQ 歌曲评论
│   ├── /api/qq/artist/detail      → QQ 歌手详情
│   ├── /api/qq/user/playlists     → QQ 用户歌单
│   ├── /api/qq/playlist/tracks    → QQ 歌单曲目
│   │
│   ├── 登录
│   │   ├── /api/qq/login/status   → QQ 登录态查询
│   │   ├── /api/qq/login/cookie   → QQ Cookie 登录
│   │   └── /api/qq/logout         → QQ 登出
│
├── 播客 API
│   ├── /api/podcast/search        → 播客搜索
│   ├── /api/podcast/hot           → 热门播客
│   ├── /api/podcast/detail        → 播客详情
│   ├── /api/podcast/programs      → 播客节目列表
│   ├── /api/podcast/my            → 我的播客收藏
│   ├── /api/podcast/my/items      → 播客收藏分项
│   └── /api/podcast/dj-beatmap    → DJ 长音频离线锁拍分析
│
├── 节拍缓存
│   ├── /api/beatmap/cache/status  → 磁盘缓存状态
│   └── /api/beatmap/cache         → 读写节拍缓存 (GET/POST)
│
├── 代理
│   ├── /api/cover?url=            → 封面图片代理 (CORS)
│   └── /api/audio?url=            → 音频流代理 (支持 Range)
│
└── 工具层 (server.js 内部函数)
    ├── Cookie 持久化               → saveCookie / saveQQCookie
    ├── 更新检查链                  → fetchLatestUpdateInfo / 镜像加速器
    ├── 下载任务管理                → updateDownloadJobs (Map)
    ├── 补丁应用                    → PATCH_ALLOWED_ROOTS / PATCH_ALLOWED_FILES
    ├── 版本比较                    → compareVersions / normalizeVersion
    └── 响应工具                    → sendJSON / serveStatic
```

---

## 4. 前端 (`public/index.html`) — 模块结构

```
public/index.html  (~26,800 行)
│
├── CSS (~2000 行)                   ← :root 变量 + 各 UI 组件样式
│   ├── 基础变量 (颜色、玻璃、字体)
│   ├── 桌面壳层 (#desktop-window-shell / titlebar)
│   ├── 搜索区 / 顶部导航
│   ├── Home 页卡片布局
│   ├── 播放器控制台 (#bottom-bar)
│   ├── 视觉控制台 (预设、色轮、滑条)
│   ├── 3D 歌单架 (常驻/侧栏/舞台)
│   ├── 歌词舞台
│   ├── 全屏 DIY 模式
│   ├── 启动页 (splash)
│   └── 视听引导 / 更新弹窗
│
└── JavaScript (~24,000 行) ← 单个 <script> 内联
    │
    ├── 全局状态 (~300 行)           ← audio, analyser, lyricsLines, playlist...
    ├── 存储工具 (~400 行)           ← localStorage 读写 + 偏好管理
    │
    ├── 核心播放 (~1000 行)
    │   ├── playQueueAt()            ← 核心播放入口
    │   ├── 音频控制 (play/pause/next/prev)
    │   ├── 音质选择
    │   ├── 淡入淡出
    │   └── 播放队列管理
    │
    ├── 歌词系统 (~1500 行)
    │   ├── 舞台 3D 歌词 (Three.js)
    │   ├── 歌词解析
    │   ├── 卡拉 OK 时间轴
    │   └── 歌词布局/颜色保存
    │
    ├── 视觉预设系统 (~4000 行)
    │   ├── 七大预设: emily, 安魂, 星河, 唱片, 星球, 滚筒, 虚空
    │   ├── 封面粒子 (Three.js Points)
    │   ├── 电影镜头 (camera animation)
    │   ├── 视觉控制台 (滑条/色轮/用户存档)
    │   └── 背景模式切换
    │
    ├── 3D 歌单架 (~3000 行)
    │   ├── makeShelfManager()       ← 3D 场景歌单管理器
    │   ├── makeContentListManager() ← 详情页列表
    │   ├── 常驻 / 侧栏 / 舞台模式
    │   ├── 动态 / 静态镜头
    │   └── 滚轮选中 + 音效合成
    │
    ├── 音频分析 (~800 行)
    │   ├── FFT 音频数据采集
    │   ├── 鼓点 / BPM 检测
    │   ├── DJ 视觉模式
    │   └── 能量曲线
    │
    ├── Home 页 (~2000 行)
    │   ├── 天气电台 UI
    │   ├── 每日/私人推荐
    │   ├── 听歌画像
    │   └── 我的歌单入口
    │
    ├── 搜索 (~800 行)
    │   ├── 网易云 / QQ 搜索
    │   ├── 播客搜索
    │   └── 搜索结果渲染
    │
    ├── 登录系统 (~1500 行)
    │   ├── 网易云扫码登录
    │   ├── QQ 扫码登录
    │   ├── 双账号模式
    │   └── Cookie 管理
    │
    ├── 启动页 splash (~600 行)
    │   ├── WebGL 光流线场
    │   ├── 2D canvas fallback
    │   └── 点击进入
    │
    ├── 桌面模式 (~500 行)
    │   ├── 窗口装饰 (titlebar)
    │   ├── 全屏/窗口化
    │   └── 桌面歌词通信
    │
    ├── 更新系统 (~400 行)
    │   ├── 版本检测
    │   ├── 下载进度 UI
    │   └── 快速补丁
    │
    ├── 自定义功能 (~600 行)
    │   ├── 自定义封面上传/裁剪
    │   ├── 自定义歌词上传
    │   └── 封面取色
    │
    ├── 性能管理层 (~600 行)
    │   ├── 后台策略 (auto/keep/release)
    │   ├── 画质档位 (低/中/高/超高)
    │   ├── 缓存修剪
    │   └── 渲染功耗模式
    │
    └── SVG 玻璃质感 (~100 行)
        ├── generateControlGlassDisplacementMap()
        └── SVG filter 注入
```

---

## 5. Electron 主进程 (`desktop/`) — 模块结构

```
desktop/
├── main.js (1,466 行)              ← Electron 主进程
│   ├── 应用生命周期
│   │   ├── app.on('ready')         → 启动本地服务器 → 创建窗口
│   │   ├── app.on('second-instance') → 单实例锁
│   │   └── app.on('window-all-closed')
│   │
│   ├── 窗口管理
│   │   ├── 主窗口 (BrowserWindow)
│   │   ├── 桌面歌词窗口 (BrowserWindow, 穿透 overlay)
│   │   ├── 壁纸窗口
│   │   ├── 全屏/窗口化切换
│   │   └── 窗口状态同步 (IPC → renderer)
│   │
│   ├── IPC 处理器 (ipcMain.handle)
│   │   ├── find-open-port
│   │   ├── get-main-server-port
│   │   ├── get-app-path
│   │   ├── show-open-dialog
│   │   ├── set-window-size
│   │   ├── enter-fullscreen / exit-fullscreen
│   │   ├── toggle-fullscreen
│   │   ├── is-window-fullscreen
│   │   ├── get-window-bounds / set-window-bounds
│   │   ├── get-window-state
│   │   ├── show-desktop-lyrics / hide-desktop-lyrics / move-desktop-lyrics
│   │   ├── desktop-lyrics-lock / unlock
│   │   ├── show-wallpaper / hide-wallpaper
│   │   ├── get-display-state
│   │   ├── shell-open-external
│   │   ├── register-global-hotkey / unregister-global-hotkey
│   │   ├── set-app-user-model-id
│   │   └── open-update-download
│   │
│   ├── Cookie 管理
│   │   ├── 网易云 Cookie 捕获 (music.163.com)
│   │   ├── QQ Cookie 捕获 (y.qq.com)
│   │   ├── buildCookieHeader() / parseCookieHeader()
│   │   └── 登录窗口管理
│   │
│   ├── 桌面歌词
│   │   ├── overlay 窗口创建
│   │   ├── 鼠标穿透/锁定 (GetAsyncKeyState 轮询)
│   │   ├── 位置保存/恢复
│   │   └── 白底/黑底自适应
│   │
│   ├── 壁纸模式
│   │   ├── WorkerW 窗口嵌入
│   │   └── 隐藏/显示控制
│   │
│   ├── 全局热键
│   │   ├── 播放/暂停、下一首、上一首
│   │   ├── 音量控制
│   │   └── 动态注册/反注册
│   │
│   ├── 更新系统
│   │   ├── 下载目录管理
│   │   └── 桌面快捷方式确保
│   │
│   └── 性能优化
│       ├── Chromium 启动参数
│       ├── 后台节流
│       └── 窗口状态定时器
│
├── preload.js (52 行)              ← 主窗口 preload
│   └── contextBridge: window.electronAPI = { ...IPC 暴露 }
│
└── overlay-preload.js (19 行)      ← 桌面歌词窗口 preload
    └── 暴露窗口移动/关闭 IPC
```

---

## 6. 音频分析 (`dj-analyzer.js`)

```
dj-analyzer.js (864 行)
│
├── analyzePodcastDjStream(url, opts)
│   └── 实时流分析: 下载音频 → BPM → 鼓点 → 节拍时间轴
│
├── analyzePodcastDjIntro(url, opts)
│   └── 片头分析: 仅分析前 introSec 秒
│
├── 内部工具
│   ├── 音频下载 / decode
│   ├── BPM 检测 (music-tempo)
│   ├── 节拍能量计算
│   └── 输出: visualBeatCount, beats[], decode meta
```

---

## 7. 安装器 (`build/installer.nsh`)

```
build/installer.nsh (1,023 行) — NSIS 安装器脚本
│
├── 自定义欢迎页
├── 自定义安装目录页
│   ├── D:\Mineradio 优先 (D→Z 遍历)
│   ├── 选择盘符自动补全 Mineradio 子目录
│   └── C 盘拦截 (D-Z 存在时)
│
├── 安装逻辑
│   ├── 路径安全性检查 (.mineradio-install-root)
│   ├── 旧卸载器处理 (跳过不安全卸载器)
│   └── 桌面快捷方式创建
│
└── 卸载逻辑
    └── 安全文件级删除 (非递归目录删除)
```

---

## 8. 外部依赖关系图

```
┌────────────────────────────────────────────────────┐
│                    Mineradio                        │
├────────────────────────────────────────────────────┤
│  public/index.html                                 │
│  ├── three.r128.min.js     (3D 渲染引擎)           │
│  ├── gsap.min.js           (动画引擎)               │
│  ├── music-tempo.min.js    (BPM 检测)               │
│  └── ⬅ fetch → server.js (全部 API 通信)           │
├────────────────────────────────────────────────────┤
│  server.js                                         │
│  ├── NeteaseCloudMusicApi  (网易云 API 封装)        │
│  └── dj-analyzer.js        (音频节奏分析)            │
├────────────────────────────────────────────────────┤
│  desktop/main.js                                   │
│  └── Electron API         (窗口、IPC、系统集成)      │
├────────────────────────────────────────────────────┤
│  build/installer.nsh                               │
│  └── electron-builder    (打包 → NSIS 安装包)       │
└────────────────────────────────────────────────────┘
```

---

## 9. 数据流

```
用户交互 (点击/搜索/播放)
       │
       ▼
public/index.html (前端渲染)
       │
       ├── fetch() → server.js (HTTP API)
       │                  │
       │                  ├── NeteaseCloudMusicApi → music.163.com
       │                  ├── QQ 音乐 API → y.qq.com
       │                  ├── Open-Meteo API → 天气数据
       │                  └── GitHub API → 更新检测
       │
       ├── Audio element → /api/audio (代理) → 音源 CDN
       │
       └── ipcRenderer → desktop/main.js (Electron)
                          ├── 窗口管理 / 全屏
                          ├── 桌面歌词 overlay
                          ├── 壁纸模式 (WorkerW)
                          └── 全局热键
```

---

## 10. 迭代建议

按维护优先级排列：

| 优先级 | 区域 | 原因 |
|--------|------|------|
| 🥇 P0 | `public/index.html` 拆分 | ~26,800 行单文件，定位修改困难 |
| 🥇 P0 | `server.js` 拆分 | ~4,200 行单文件，API 路由 + 业务逻辑混合 |
| 🥈 P1 | `public/index.html` 内联 CSS 独立 | CSS 混在 `<style>` 中，约 2000 行 |
| 🥈 P1 | 前端模块化 | 全局变量(~200 个)易冲突，考虑 ES modules |
| 🥉 P2 | `desktop/main.js` 热键/窗口/桌面歌词拆分 | 1,466 行，职责较重 |
| 🥉 P2 | 安装器 nsh 文档化 | 复杂安全规则需要更清晰的注释 |
