# 模块 9：定时任务 (Cron Jobs)

> 预计阅读时间：8 分钟 · 前置：[模块 8](../08-delegation/)

Agent 在无人操作的情况下自动完成工作——定时汇总、监控、报告。

> 💡 推荐：浏览器打开 `index.html` 获得最佳体验（深色主题 + Mermaid 图表）。

---

## 1. Cron 调度器

```bash
hermes cron list              # 列出
hermes cron create "30m"      # 每 30 分钟
hermes cron create "0 9 * * *" # 每天 9 点
```

## 2. 调度表达式

| 格式 | 示例 | 含义 |
|------|------|------|
| 时长 | `30m`, `2h` | 每 30 分 / 每 2 小时 |
| every | `every monday 9am` | 每周一 9 点 |
| Cron | `0 9 * * *` | 每天 9 点 |
| ISO | `2026-06-01T09:00` | 一次性 |

## 3. 两种模式

- **LLM 模式**（默认）：Agent 运行 prompt，可推理、调工具
- **Script 模式**（`no_agent=true`）：只跑脚本，零 token

## 4. 任务链

通过 `context_from` 串联 Job：A 收集数据 → B 分析 → C 生成报告。

---

> **下一步：** [模块 10：进阶特性](../10-advanced/)
