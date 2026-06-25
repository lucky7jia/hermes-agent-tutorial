# 模块 5：技能系统 (Skills) ⭐

> 预计阅读时间：20 分钟 · 前置：[模块 4](../04-tools/)

Hermes 最独特的能力——从经验中自我学习，越用越聪明。

> 💡 推荐：浏览器打开 `index.html` 获得最佳体验（深色主题 + Mermaid 图表）。

---

## 1. Skill 是什么？

Agent 完成一次复杂任务后，把流程、命令、注意事项保存为 Skill。下次类似任务自动加载，不用从头摸索。

**这是 Hermes 和所有其他 Agent 框架最本质的区别。** Claude Code、Codex CLI 都不会从经验中学习。

## 2. SKILL.md 格式

```yaml
---
name: my-skill
description: 什么场景触发
version: 1.0.0
---

# 技能标题

## 何时使用
触发条件

## 工作流程
1. 步骤一
2. 步骤二

## 常见问题
- 坑：描述 + 解决方案
```

## 3. 生命周期

```
Agent 完成任务 → 保存为 Skill → Curator 追踪使用频率
→ 闲置 30 天标记 stale → 再 30 天自动归档（可恢复）
```

```bash
hermes curator status    # 查看状态
hermes curator pin NAME  # 锁定重要 Skill
```

## 4. Skill vs Project Context

| 维度 | Skill | Project Context |
|------|-------|----------------|
| 范围 | 跨项目通用 | 当前项目独有 |
| 加载 | 手动 /skill 或自动匹配 | 进入项目目录自动 |
| 管理 | Curator 自动 | 手动编辑 |

## 5. 命令

```bash
hermes skills list          # 列出
hermes skills search QUERY  # 搜索 Hub
hermes skills install ID    # 安装
/skill name                 # 会话内加载
```

---

> **下一步：** [模块 6：记忆系统](../06-memory/)
