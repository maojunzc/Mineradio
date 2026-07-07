# Mineradio

[![License: GPL-3.0](https://img.shields.io/badge/License-GPL--3.0-blue.svg)](LICENSE)
[![Release](https://img.shields.io/github/v/release/maojunzc/Mineradio)](https://github.com/maojunzc/Mineradio/releases)

![Mineradio 暗场启动页](./docs/assets/readme/cinema-beat-smoke.png)

Mineradio 是一款 Windows 桌面沉浸式音乐播放器，融合天气电台、搜索播放、歌词舞台、粒子视觉和 3D 歌单架，提供更接近现场感的私人音乐空间。

---

## 致谢

Mineradio 最初由 [XxHuberrr](https://github.com/XxHuberrr) 设计并开发，感谢他为项目打下的坚实基础。本仓库在原作者基础上持续进行 Bug 修复和功能迭代。

同时感谢以下贡献者在早期体验、测试反馈和发布准备中的帮助：emily、小天才e宝、应春日、锋将军、軌跡、林中、骊、风痕、花椰菜。

---

## 社区维护版优化清单

本项目在保留上游全部功能的基础上，完成了以下优化：

### 新增功能

| 功能 | 说明 |
|------|------|
| 发现页 | 热门歌曲、新歌推荐、12 种音乐风格探索，点击即播 |
| 播客专区 | 精选播客推荐卡片，一键跳转详情页 |
| 播放队列增强 | 队列/历史双视图，播放历史自动追踪（最近 50 首） |
| 音效控制台 | 3 段均衡器（低音/中音/高音）、8 种音效预设、夜间模式 |
| 听歌报告 | 热门歌曲/歌手排名、播放次数统计、累计播放时长 |

### Bug 修复

| 问题 | 状态 |
|------|------|
| 安装器将网络驱动器误判为本地磁盘 | 已修复 |
| 系统代理不可用时软件无法启动 | 已修复 |
| 导出口按钮缺少玻璃质感样式 | 已修复 |
| 尝试安装到 C 盘时被安装器阻止 | 已修复 |
| 歌单内无法删除歌曲 | 已修复 |
| 舞台模式 + 自动隐藏不生效 | 已修复 |

---

## 开发计划

### 短期（1-2 个月）

- 接入酷我音乐搜索与播放
- 接入酷狗音乐搜索与播放
- 桌面版 3D 视觉性能优化
- 本地音乐扫描与管理

### 中期（3-6 个月）

- **Mineradio App**（Flutter 移动端）开发
- 多平台歌单同步方案
- 更多音乐源接入（咪咕等）

### 长期

- 移动端与桌面端功能对齐
- 社区插件系统
- 根据用户 Issue 反馈持续迭代

> 移动端 App 项目正在开发中，完成度达到公开发布标准后将会开源。

---

## 安装与使用

从 [Releases](https://github.com/maojunzc/Mineradio/releases) 下载最新版安装包，或通过源码构建：

```bash
npm install
npm start
npm run build:win    # 构建 Windows 安装包
```

---

## 参与贡献

欢迎提交 Issue 和 Pull Request 帮助改进项目：

- 提交 Bug 或功能建议 → [New Issue](https://github.com/maojunzc/Mineradio/issues/new)
- 代码贡献请先阅读 [CONTRIBUTING.md](./CONTRIBUTING.md)
- 重大变更请先开 Issue 讨论

项目处于积极维护状态，所有 Issue 都会得到回复。

---

## 第三方音乐平台说明

Mineradio 不是网易云音乐、QQ 音乐或腾讯音乐娱乐集团的官方客户端，也不隶属于任何音乐平台。

项目中的第三方平台接入仅用于个人学习、本地客户端体验和用户自有账号的播放辅助。请遵守对应平台的用户协议、版权规则和会员权益规则。

---

## 版权与授权

本项目基于 [GPL-3.0](LICENSE) 授权，源自 [XxHuberrr/Mineradio](https://github.com/XxHuberrr/Mineradio)。

第三方依赖和第三方服务分别遵循其各自授权与服务条款。
