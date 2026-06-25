# 附录：本地安全运行指南

> 预计阅读时间：15 分钟 · 前置：[模块 2：安装、配置与 Provider](../02-install-config/)

在 Mac 上用 Apple Container 搭建隔离的 Hermes Agent 环境——进程 VM 级隔离、对外暴露 API、数据不出本机。

> 💡 推荐：浏览器打开 `index.html` 获得最佳体验（深色主题 + Mermaid 图表）。本文档为 Markdown 精简版。

---

## 0. 快速开始

```bash
# 1. 安装 Apple container（macOS 15+ / Apple 芯片）
brew install --cask container
container system start

# 2. 建数据目录
mkdir -p ~/.hermes

# 3. 配置模型
container run -it --rm -v ~/.hermes:/opt/data nousresearch/hermes-agent setup

# 4. 配置 API Server（追加到 ~/.hermes/.env）
API_SERVER_ENABLED=true
API_SERVER_HOST=0.0.0.0
API_SERVER_PORT=8642
API_SERVER_KEY=<你的随机密钥>

# 5. 启动
container run -d --name hermes -c 4 -m 8G \
  -v ~/.hermes:/opt/data \
  -p 127.0.0.1:9119:9119 -p 127.0.0.1:8642:8642 \
  -e HERMES_DASHBOARD=1 \
  nousresearch/hermes-agent gateway run

# 6. 使用
#   Web： http://localhost:9119
#   API： http://localhost:8642/v1
```

---

## 1. 方案概述

| 项 | 内容 |
|---|---|
| 目标 | 本地搭建可隔离、可编程调用的 AI Agent 执行环境 |
| 底座 | Apple container（轻量 VM）运行 Hermes Agent，进程 VM 级隔离 |
| 接口 | OpenAI 兼容 API（`http://127.0.0.1:8642/v1`），任意 OpenAI SDK 直连 |
| 模型 | 可接任何 OpenAI 兼容端点（OpenRouter / Anthropic / 自定义网关 / 本地模型） |
| 能力 | Agent 能真正调工具执行任务 |
| 入口 | HTTP API + Web Dashboard + 飞书/Telegram 等 |

---

## 2. 分层架构

```
调用方（Python / curl / 自动化）
  ↓
接入层（API Server :8642 / Dashboard :9119）
  ↓
Agent 运行时 — Hermes Agent
  ↓
隔离层 — Apple Container (VM)，仅挂载 ~/.hermes
  ↓
宿主机 macOS
```

---

## 3. 完整搭建

### 3.1 安装 Apple container

需要 macOS 15+ / Apple 芯片：

```bash
brew install --cask container
container system start
```

### 3.2 建数据目录

```bash
mkdir -p ~/.hermes
```

配置、密钥、会话全落在这里。容器删了也不丢。备份整个 `~/.hermes` 即可迁移。

### 3.3 配置模型

```bash
container run -it --rm -v ~/.hermes:/opt/data nousresearch/hermes-agent setup
```

- **Provider** → 选你用的（OpenRouter / Anthropic / Custom 等）
- 自定义端点 → 选 Custom，填 base_url + key + 模型名
- 其余可先 Skip

### 3.4 开启 Dashboard 认证

在 `~/.hermes/config.yaml` 加入：

```yaml
dashboard:
  basic_auth:
    username: 'admin'
    password_hash: '<scrypt$...>'
```

生成哈希：`container run --rm nousresearch/hermes-agent python -c "from plugins.dashboard_auth.basic import hash_password; print(hash_password('<密码>'))"`

### 3.5 开启 API Server

在 `~/.hermes/.env` 追加：

```ini
API_SERVER_ENABLED=true
API_SERVER_HOST=0.0.0.0
API_SERVER_PORT=8642
API_SERVER_KEY=<随机密钥>
```

生成随机 key：`openssl rand -base64 32`

### 3.6 启动容器

```bash
container run -d --name hermes -c 4 -m 8G \
  -v ~/.hermes:/opt/data \
  -p 127.0.0.1:9119:9119 -p 127.0.0.1:8642:8642 \
  -e HERMES_DASHBOARD=1 \
  nousresearch/hermes-agent gateway run
```

> ⚠ `-c 4 -m 8G` 必须带（默认 1G 不够）；`-p 127.0.0.1` 只绑本机；改 .env 需 rm 重建。

---

## 4. 调用方法

**Base URL:** `http://127.0.0.1:8642/v1` · **API Key:** 你的 `API_SERVER_KEY` · **Model:** `hermes-agent`

### curl

```bash
curl http://127.0.0.1:8642/v1/chat/completions \
  -H "Authorization: Bearer <API_SERVER_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"model":"hermes-agent","messages":[{"role":"user","content":"任务"}],"stream":false}'
```

### Python

```python
from openai import OpenAI
client = OpenAI(base_url="http://127.0.0.1:8642/v1", api_key="<KEY>")
r = client.chat.completions.create(model="hermes-agent", messages=[{"role":"user","content":"任务"}])
print(r.choices[0].message.content)
```

### 会话连续性

每请求带 `X-Hermes-Session-Id` header：

```bash
curl ... -H "X-Hermes-Session-Id: my-chat-001" ...
```

```python
r = client.chat.completions.create(..., extra_headers={"X-Hermes-Session-Id": "my-chat-001"})
```

---

## 5. 安全设计

| 维度 | 措施 |
|------|------|
| 进程隔离 | Agent 跑在 Apple Container（VM）内，宿主机不可达 |
| 文件边界 | 仅挂载 `~/.hermes`，Mac 其他文件碰不到 |
| 网络封闭 | 端口只绑 127.0.0.1，局域网不可访问 |
| 密钥本地 | 全部存本地，不上传第三方 |

> ⚠ `~/.hermes/.env` 含 API key 和密码，不要分享或截图。

---

## 6. 运维命令

```bash
container ls                  # 查看容器
container logs -f hermes      # 实时日志
container stop hermes         # 停止
container rm -f hermes        # 删除（配置保留）
container exec -it hermes bash # 进入容器
curl -s http://127.0.0.1:9119/api/status  # 健康检查
```

---

## 7. FAQ

- **连不上？** 确认端口映射 + 日志无报错。Dashboard 9119，API 8642。
- **Dashboard 拒绝启动？** 配 basic_auth 后重建。
- **卡顿？** 加 `-m 8G`（默认 1G）。
- **改 .env 不生效？** `container start` 不重读，需 rm 后 run。
- **IP 变了？** 用 `-p 127.0.0.1:端口:端口` 映射，始终 localhost 访问。
