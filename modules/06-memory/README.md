# 模块 6：记忆系统 (Memory)

> 预计阅读时间：10 分钟 · 前置：[模块 5](../05-skills/)

跨会话持久化——Agent 不会"失忆"，你也不用重复说同样的话。

> 💡 推荐：浏览器打开 `index.html` 获得最佳体验（深色主题 + Mermaid 图表）。

---

## 1. 两类 Memory

| 维度 | User Profile | Agent Memory |
|------|-------------|-------------|
| 存什么 | 用户是谁 | Agent 的笔记 |
| 示例 | "用户偏好直接执行" | "JDK 21 路径" |
| 目标 | 减少重复自我介绍 | 积累跨会话知识 |

## 2. 运作方式

Memory 在每个 turn 自动注入 System Prompt（Layer 3+4）。Agent 无需主动查询。

字符预算默认 2,200 字符，快满时需合并优化。

## 3. 配置

```yaml
memory:
  memory_enabled: true
  user_profile_enabled: true
  provider: builtin  # builtin / honcho / mem0
```

## 4. 最佳实践

**✅ 该存的：** 用户偏好、环境路径、项目约定、工具版本问题
**❌ 不该存的：** 会话进度、TODO、PR 号、commit SHA

---

> **下一步：** [模块 7：Gateway 多平台接入](../07-gateway/)
