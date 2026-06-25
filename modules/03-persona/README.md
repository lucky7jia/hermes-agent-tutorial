# 模块 3：系统提示词与人格 (Persona)

> 预计阅读时间：20 分钟 · 前置：[模块 1](../01-core-concepts/) · [模块 2](../02-install-config/)

定义 Agent 的"灵魂"——名字、性格、说话方式、工作原则。

> 💡 推荐：浏览器打开 `index.html` 获得最佳体验（深色主题 + Mermaid 图表）。本文档为 Markdown 精简版。

---

## 1. Persona 是什么？

Persona 是 System Prompt 的 **Layer 1**（最高优先级），定义了 Agent 的角色、性格和工作原则。

## 2. Persona 六大模块

| 模块 | 内容 |
|------|------|
| 名字 | Agent 叫什么——身份锚点 |
| 身份 | 角色定位——决定行为边界 |
| 性格与说话方式 | 语气、语言、风格 |
| 工作原则 | 做事方法论 |
| 绝对不做的事 | 硬约束——优先级最高 |
| 能力范围 | 能做什么——引导思维方向 |

## 3. SOUL.md

`~/.hermes/SOUL.md` —— 全局人格文件，所有会话共享。优先级：Persona > SOUL.md。

## 4. Project Context 文件

| 文件 | 发现方式 | 适用场景 |
|------|---------|---------|
| `.hermes.md` | 向上遍历到 git root | Hermes 专属 |
| `AGENTS.md` | 仅当前目录 | 跨 Agent 可移植 |
| `CLAUDE.md` | 仅当前目录 | Claude 风格 |

**先到先得，不叠加。** 单个文件上限 20,000 字符。

## 5. System Prompt 加载优先级

```
Layer 1 · Persona          ← 最高优先级
Layer 2 · SOUL.md
Layer 3 · User Profile     ← Memory
Layer 4 · Agent Memory     ← Memory
Layer 5 · Active Skills
Layer 6 · Project Context
Layer 7 · Environment Hints
Layer 8 · Tools
```

上层覆盖下层，下层可补充上层未涉及的领域。

## 6. Profile 多身份系统

```bash
hermes profile create work      # 工作用
hermes profile create personal  # 个人用
hermes --profile work           # 切换身份
```

每个 Profile 独立拥有：config.yaml、.env、SOUL.md、skills/、Memory。

## 7. 实战示例

完整「代码审查助手」Persona 示例见 `index.html`。

## 8. 常见疑问

- **Persona 写多长？** 200-800 词最佳。核心约束放前面。
- **Persona vs SOUL.md？** Persona 优先级更高，放任务级指令；SOUL.md 放通用身份。
- **怎么确认生效？** 问 Agent「你是谁？」。
- **改 Persona 要重启？** 需要 `/reset` 或新会话。

---

> **下一步：** [模块 4：工具系统](../04-tools/)
