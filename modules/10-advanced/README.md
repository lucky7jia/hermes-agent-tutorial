# 模块 10：进阶特性

> 预计阅读时间：8 分钟 · 前置：[模块 9](../09-cron/)

Context Compression、Plugins、MCP、Webhooks、Kanban——让 Hermes 更强大。

> 💡 推荐：浏览器打开 `index.html` 获得最佳体验（深色主题 + Mermaid 图表）。

---

## 1. Context Compression

历史太长时自动压缩：

```yaml
compression:
  enabled: true
  threshold: 0.50    # 超 50% 触发
  target_ratio: 0.20 # 压到 20%
  protect_last_n: 20 # 保护最近 20 条
```

## 2. Plugins

```bash
hermes plugins list
hermes plugins install
```

扩展核心能力——新 Provider、新工具、新平台适配器。

## 3. MCP Servers

接入外部工具服务器：

```bash
hermes mcp add NAME --url URL
hermes mcp add NAME --command CMD
```

## 4. Webhooks

外部系统通过 Webhook 触发 Agent：

```bash
hermes webhook subscribe NAME
hermes webhook test NAME
```

## 5. Kanban

多 Profile 协作任务板：

```bash
hermes kanban init
hermes kanban create
hermes kanban dispatch
```

## 6. Checkpoints

文件快照与回退：

```yaml
checkpoints:
  enabled: true
  max_snapshots: 50
```

---

> **🎉 恭喜完成全部 10 个模块！** 你已经掌握了 Hermes Agent 的全部核心概念。
