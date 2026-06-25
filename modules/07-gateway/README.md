# 模块 7：Gateway 多平台接入

> 预计阅读时间：8 分钟 · 前置：[模块 6](../06-memory/)

同一套 Agent 逻辑跑在飞书、Telegram、Discord 等 10+ 平台。

> 💡 推荐：浏览器打开 `index.html` 获得最佳体验（深色主题 + Mermaid 图表）。

---

## 1. Gateway 是什么？

多平台消息适配层——Agent 核心逻辑不变，只换适配器。

## 2. 支持平台

| 平台 | 复杂度 | 说明 |
|------|--------|------|
| 飞书 | 中 | 创建应用 → 凭证 → Webhook |
| Telegram | 低 | BotFather 创建 Bot |
| Discord | 低 | Application → Bot Token |
| Slack | 中 | App → OAuth Token |
| 微信 | 高 | 需企业微信或第三方 |
| API Server | 低 | 开箱即用 HTTP API |

## 3. 命令

```bash
hermes gateway setup     # 配置平台
hermes gateway run       # 前台运行
hermes gateway install   # 安装后台服务
hermes gateway restart   # 重启
```

## 4. 命令审批

- **manual**：危险命令弹确认（默认）
- **smart**：LLM 辅助判断
- **off**：跳过（不推荐）

---

> **下一步：** [模块 8：委托与多智能体](../08-delegation/)
