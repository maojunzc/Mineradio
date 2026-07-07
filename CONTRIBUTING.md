# Contributing

感谢你有兴趣为 Mineradio 做出贡献。

## 提交 Issue

- Bug 报告请注明系统版本、Mineradio 版本和复现步骤
- 功能建议请描述使用场景和期望效果
- 提问前请先搜索是否已有相同 Issue

## 提交 Pull Request

1. Fork 本仓库
2. 创建特性分支：`git checkout -b feature/your-feature`
3. 提交改动：`git commit -m 'feat: add some feature'`
4. 推送到分支：`git push origin feature/your-feature`
5. 提交 Pull Request

## 开发指引

- 主要逻辑在 `server.js`（后端 API）和 `public/index.html`（前端界面）
- 后端 API 路由通过 `pn === '/api/xxx'` 方式注册
- 前端使用原生 JavaScript，无框架依赖
- 安装器逻辑在 `build/installer.nsh`

### 本地开发

```bash
npm install
npm start          # 启动开发服务器
npm run build:win  # 构建安装包
```

### 代码规范

- 保持现有代码风格和注释习惯
- 新增功能请同步更新 CHANGELOG.md
- 提交信息遵循 `type: description` 格式（feat/fix/docs/chore 等）

## 行为准则

- 尊重所有参与者
- 接受建设性批评
- 专注于让项目变得更好
