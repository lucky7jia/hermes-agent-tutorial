# 模块 1：核心概念与架构

> 预计阅读时间：25 分钟 · 适合：所有技术背景

从"什么是 AI Agent"开始，一步步拆解 Hermes 的骨架——它怎么想、怎么做、各部分怎么配合。

> 💡 **推荐阅读方式：** 下载本项目后，用浏览器打开 `index.html` 获得最佳体验（含交互式 SVG 图表、深色主题）。本文档为 Markdown 精简版。

---

## 0. 先理解：「Agent」到底是什么？

### 类比：员工 vs 搜索引擎

| 传统 AI（ChatGPT 等） | AI Agent（Hermes 等） |
|----------------------|----------------------|
| 像**知识渊博的顾问**——你问，它答，结束 | 像**能干的员工**——你交代任务，它自己规划、执行、验证 |
| 不会主动做事 | 能主动调用工具（终端、文件、浏览器…） |
| 不记你上次说了什么 | 跨会话记住你的偏好和历史 |
| 无法操作你的系统 | 可以无人值守定时运行 |

### Agent 的四个核心能力

所有 AI Agent 都具备这四个维度：

1. **感知 (Perceive)** — 读取输入：消息、文件内容、API 返回、终端输出
2. **推理 (Reason)** — 理解意图、拆解任务、决定下一步（LLM 的核心价值）
3. **行动 (Act)** — 实际执行：写文件、跑命令、调 API、发消息
4. **记忆 (Remember)** — 把重要信息存下来，下次还能用

ChatGPT 只有前两个（感知+推理），**没有行动能力和持久记忆**。这就是 Agent 和 Chatbot 的本质区别。

### Agent 光谱

| 类型 | 例子 | 感知 | 推理 | 行动 | 记忆 |
|------|------|:--:|:--:|:--:|:--:|
| Chatbot | ChatGPT 网页版 | ✅ | ✅ | ❌ | ❌ |
| Copilot | GitHub Copilot | ✅ | ✅ | ✅(仅编辑器) | ❌ |
| Coding Agent | Claude Code / Codex CLI | ✅ | ✅ | ✅(终端+文件) | ❌(单会话) |
| **Autonomous Agent** | **Hermes Agent** | ✅ | ✅ | ✅(完整系统) | ✅(跨会话) |

---

## 0b. Hermes 在同类产品中的位置

| 维度 | ChatGPT | Claude Code | Codex CLI | **Hermes** |
|------|---------|-------------|-----------|-----------|
| 定位 | 通用 Chatbot | 终端 Coding Agent | 终端 Coding Agent | **通用自主 Agent** |
| 持久记忆 | ✗ | ✗ | ✗ | **✓ 跨会话** |
| 自我学习 | ✗ | ✗ | ✗ | **✓ Skills 系统** |
| 多平台 | 网页+App | 终端 | 终端 | **✓ 飞书/Telegram/Discord/…** |
| 定时任务 | ✗ | ✗ | ✗ | **✓ Cron** |
| Provider | 仅 OpenAI | 仅 Anthropic | 仅 OpenAI | **✓ 20+** |
| 开源 | ✗ | ✗ | ✗ | **✓ MIT** |

---

## 1. Hermes Agent 的核心定位

> **Hermes 不是 ChatGPT + 几个插件。** 它是一个有记忆、会学习、能独立干活的自主 Agent 进程。

### 四大设计理念

- **🔓 Provider 无关** — 20+ Provider，任务中途可切换模型
- **🧠 自我进化** — 完成任务后自动保存为 Skill，越用越强
- **🌐 无处不在** — 同一套逻辑跑在终端/飞书/Telegram/Discord/…
- **⏰ 自主运行** — Cron 定时任务，无人值守自动工作

---

## 2. 核心引擎：Agent Loop

### 一个具体例子

用户说：「帮我在 `~/projects/myapp` 创建 FastAPI 项目，带 `/health` 端点。」

| 步 | LLM 返回 | Agent 执行 | 结果 |
|:--:|---------|-----------|------|
| 1 | Tool Call: `mkdir` | 创建目录 | 成功 |
| 2 | Tool Call: `write_file(main.py)` | 写 FastAPI 代码 | 已写入 |
| 3 | Tool Call: `write_file(requirements.txt)` | 写依赖 | 已写入 |
| 4 | Tool Call: `pip install` | 安装依赖 | 成功 |
| 5 | Tool Call: `curl /health` | 验证端点 | `{"status":"ok"}` |
| 6 | **文本回复** | → 通知用户 | "项目已创建" |

Agent 不是一次性搞定的。它一步一步来——每步都是 LLM 看了上一步结果后决定下一步。

### Agent Loop 流程

```
┌──────────────────────────────────┐
│ ① 构建 System Prompt（一次性）     │
│   Persona + SOUL.md + Memory      │
│   + Skills + Tools + Context      │
└──────────────┬───────────────────┘
               ▼
┌──────────────────────────────────┐
│ ② 调用 LLM                        │
│   发送 messages + tool schemas    │
└──────┬───────────────┬───────────┘
       ▼               ▼
  Tool Call        文本回复
       │               │
       ▼               ▼
  ③a 执行工具      ③b 返回用户
       │
       └──→ 结果追加 → 回到 ②
```

### 关键概念

- **Turn**：1 次 LLM 调用。Tool Call 是 Turn 内的子步骤
- **并行 Tool Call**：LLM 可一次返回多个 tool call，并行执行
- **Prompt Caching**：System Prompt 会话内不变，换工具/技能需 `/reset`
- **Context Compression**：历史太长时自动压缩

---

## 3. 关键目录结构

```
~/.hermes/                    ← 数据根目录（$HERMES_HOME）
├── config.yaml               ← 主配置（模型、Agent 参数…）
├── .env                      ← API Key（不入 git）
├── SOUL.md                   ← 全局人格定义（始终加载）
├── skills/                   ← 安装的技能文件
├── state.db                  ← SQLite 会话数据库（FTS5）
├── sessions/                 ← 路由索引 + 日志
├── logs/                     ← Gateway 日志
└── profiles/                 ← 多 Profile 隔离
    └── default/
```

---

## 4. System Prompt 的 8 层结构

```
Layer 1 · Agent Persona          "你叫 Hermes…"
Layer 2 · SOUL.md                 全局身份
Layer 3 · User Profile            用户画像（Memory）
Layer 4 · Agent Memory            Agent 笔记（Memory）
Layer 5 · Active Skills           加载的技能
Layer 6 · Project Context         .hermes.md / AGENTS.md
Layer 7 · Environment Hints       OS / Shell / CWD
Layer 8 · Tools + Enforcement     tool schemas
```

**优先级：** Persona > SOUL.md > Memory > Skills > Project Context

Project Context 文件**先到先得**，不叠加：
1. `.hermes.md` — 向上遍历到 git root
2. `AGENTS.md` — 仅 cwd
3. `CLAUDE.md` — 仅 cwd
4. `.cursorrules` — 仅 cwd

---

## 5. 一次请求的完整数据流

```
用户 → 平台适配器(Gateway) → Agent Loop(构建Prompt+读state.db)
  → LLM API → {Tool Call? → 执行→追加→再调LLM} → 文本回复
  → 平台适配器 → 用户
```

- Tool Call 路径可能循环多次（上限 max_turns=90）
- 文本路径直接格式化投递到平台

---

## 6. 核心术语速查

| 术语 | 含义 |
|------|------|
| Agent Loop | Prompt → LLM → 工具 → 循环。核心引擎 |
| System Prompt | 8 层拼成的"世界观"，决定认知边界 |
| Turn | 1 次 LLM 调用 + 后续工具执行 |
| Compression | 历史太长时自动压缩 |
| Gateway | 多平台接入层 |
| state.db | SQLite 会话存储 + 全文搜索 |
| Memory | 跨会话持久记忆 |
| Toolset | 工具分组，控制能力边界 |
| Skill | 可复用知识文档，自学习核心 |
| Profile | 多套配置隔离 |
| Cron Job | 定时自动任务 |
| Delegation | 子 Agent 并行工作 |

---

## 7. 常见疑问

**Q: Hermes 和 LangChain / AutoGPT 有什么区别？**

LangChain 是开发框架（用来构建 Agent），Hermes 是成品 Agent（直接干活）。AutoGPT 是早期实验项目，基本不维护。

**Q: Agent Loop 会不会死循环？**

不会。max_turns=90 强制终止 + Context Compression + LLM 自身判断力。

**Q: 数据隐私？**

全部本地存储，不上传。可配置自动脱敏和定期清理。

**Q: 能用本地模型吗？**

可以。任何 OpenAI 兼容端点的本地模型都行。建议 70B+ 以保证工具调用准确度。

---

> **下一步：** [模块 2：安装、配置与 Provider](../02-install-config/)
