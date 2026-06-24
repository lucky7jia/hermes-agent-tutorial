# 模块 2：安装、配置与 Provider

> 预计阅读时间：20 分钟 · 前置：[模块 1：核心概念与架构](../01-core-concepts/)

把 Hermes 跑起来——从安装到接入你喜欢的 LLM 模型。

> 💡 **推荐：** 本地浏览器打开 `index.html` 获得最佳体验（深色主题 + 交互图表）。本文档为 Markdown 精简版。

---

## 1. 安装 Hermes

### Linux / macOS / WSL2

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

### Windows (PowerShell)

```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

安装脚本做的事：下载 → 解压到 `~/.hermes/hermes-agent/` → 创建 `~/.local/bin/hermes` 软链接 → 加入 PATH。

**更新**：`hermes update`  
**卸载**：`hermes uninstall`

---

## 2. 首次运行：hermes setup

```bash
hermes setup              # 交互式配置向导
hermes setup model        # 只配模型
hermes setup terminal     # 只配终端后端
hermes setup gateway      # 只配平台接入
hermes setup --portal     # 最快路径：OAuth 一条龙
```

Setup 向导会问你：Provider → API Key → 模型 → 终端后端 → 平台接入（可选）。

---

## 3. config.yaml 深度解析

```bash
hermes config              # 查看当前配置
hermes config edit         # 用 $EDITOR 编辑
hermes config set key val  # 命令行改单项
hermes config path         # 打印路径
```

### 主要配置段

| 段 | 作用 | 关键字段 |
|----|------|---------|
| `model` | 模型配置（必填） | `default`, `provider`, `base_url`, `context_length` |
| `agent` | Agent 行为 | `max_turns`(90), `tool_use_enforcement` |
| `terminal` | 终端后端 | `backend`(local/docker/ssh), `timeout`(180) |
| `compression` | 上下文压缩 | `threshold`(0.50), `target_ratio`(0.20) |
| `memory` | 持久记忆 | `memory_enabled`, `user_profile_enabled`, `provider` |
| `approvals` | 命令审批 | `mode`(manual/smart/off) |
| `delegation` | 子代理 | `model`, `max_iterations`(50) |
| `display` | 显示偏好 | `skin`, `show_reasoning`, `show_cost` |

> ⚠ 改配置后需要重启（Gateway: `/restart`，终端: 退出重进）。

---

## 4. .env 密钥管理

```
OPENROUTER_API_KEY=YOUR_API_KEY
ANTHROPIC_API_KEY=YOUR_API_KEY
DEEPSEEK_API_KEY=YOUR_DEEPSEEK_KEY
GEMINI_API_KEY=AIzaxxxxxx
```

- 密钥永远放 `.env`，不要写 `config.yaml`
- `.env` **不入 git**——加到 `.gitignore`
- `.env` 优先级 > config.yaml 中的 api_key

---

## 5. Provider 体系（20+）

| Provider | 认证 | 环境变量 |
|----------|------|---------|
| OpenRouter | API Key | `OPENROUTER_API_KEY` |
| Anthropic | API Key | `ANTHROPIC_API_KEY` |
| OpenAI | API Key | `OPENAI_API_KEY` |
| DeepSeek | API Key | `DEEPSEEK_API_KEY` |
| Google Gemini | API Key | `GEMINI_API_KEY` |
| xAI / Grok | API Key | `XAI_API_KEY` |
| GitHub Copilot | OAuth | `COPILOT_GITHUB_TOKEN` |
| Nous Portal | OAuth | `hermes auth` |
| HuggingFace | Token | `HF_TOKEN` |
| Kimi / Moonshot | API Key | `KIMI_API_KEY` |
| Alibaba DashScope | API Key | `DASHSCOPE_API_KEY` |
| Custom | 自定义 | 设 `base_url` + `api_key` |

**推荐初学者用 OpenRouter**——一个 Key 通吃 200+ 模型。

### Custom Provider（本地模型等）

```yaml
model:
  default: "llama-3.1-70b"
  provider: "custom"
  base_url: "http://localhost:11434/v1"
  api_key: "ollama"
```

---

## 6. 模型选择与切换

```bash
hermes model                        # 交互式选择
hermes chat -m "deepseek/deepseek-chat"  # 临时切换
hermes config set model.default "..."    # 永久切换
/model anthropic/claude-sonnet-4         # 会话内切换
```

### 推荐

| 定位 | 模型 |
|------|------|
| 最强综合 | `anthropic/claude-sonnet-4` |
| 高性价比 | `deepseek/deepseek-chat` |
| 国内友好 | DeepSeek / Kimi / DashScope |
| 本地运行 | Ollama + Custom Provider |
| 快速便宜 | `google/gemini-2.5-flash` |

---

## 7. 凭证池（Credential Pool）

多 Key 自动轮转，避免 Rate Limit：

```bash
hermes auth add openrouter      # 添加 Key
hermes auth list                # 查看
hermes auth reset openrouter    # 恢复耗尽状态
```

---

## 8. 验证与排障

```bash
hermes doctor        # 健康检查
hermes doctor --fix  # 自动修复
hermes config check  # 配置完整性检查
```

| 现象 | 解决 |
|------|------|
| `command not found` | 加 `~/.local/bin` 到 PATH |
| 401/403 | 检查 API Key |
| 429 | 等一会或加凭证池 |
| 工具不出现 | `hermes tools` 启用 → `/reset` |
| 改配置不生效 | 重启会话 |

---

> **下一步：** [模块 3：系统提示词与人格 (Persona)](../03-persona/)（待完成）
