# Mineradio Next Chat Handoff

更新时间：2026-07-06

## 新对话先执行

```bash
git status --short --branch
git log --oneline -5 --decorate
cat AGENTS.md
cat docs/PROJECT_MEMORY.md
```

如涉及 3D 歌单架、发布、安装包，再读：

```bash
cat docs/3D_PLAYLIST_SHELF_MEMORY.md
cat CHANGELOG.md
cat RELEASE.md
```

## 当前状态

- 当前版本基线：`v1.1.1`
- 安装包样式沿用 `docs/INSTALLER_STYLE.md` 的中文极简黑白蓝格式。

## 不要做

- 不要使用 `git reset --hard` 或 `git checkout --` 回滚用户改动。
