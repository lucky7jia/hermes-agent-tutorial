# 模块 4：工具系统 (Tools)

> 预计阅读时间：15 分钟 · 前置：[模块 3](../03-persona/)

Agent 的"双手"——通过工具操作终端、文件、浏览器和网络。

> 💡 推荐：浏览器打开 `index.html` 获得最佳体验（深色主题 + Mermaid 图表）。

---

## 1. 什么是 Hermes 工具？

每个工具 = Python 函数 + JSON Schema。LLM 返回 tool call → Hermes 解析 → 执行函数 → 结果追加到上下文。

## 2. 内置工具

| 类别 | 工具 | 功能 |
|------|------|------|
| 文件 | `read_file`, `write_file`, `patch`, `search_files` | 文件读写搜索 |
| 终端 | `terminal`, `process`, `execute_code` | Shell 命令、Python 沙箱 |
| 浏览器 | `browser_navigate`, `browser_click`, `browser_type`, `browser_snapshot`, `browser_vision` | Web 自动化 |
| 网络 | `web_search`, `web_extract` | 搜索、内容提取 |
| 会话 | `memory`, `session_search`, `todo` | 记忆、历史、任务 |
| 协作 | `delegate_task`, `cronjob` | 子 Agent、定时任务 |

## 3. Toolsets

```bash
hermes tools                 # 交互式管理
hermes tools enable browser  # 启用
hermes tools disable image_gen  # 禁用
```

不用的工具不要启用——减少 System Prompt 长度，让 LLM 更聚焦。改动后需要 `/reset`。

## 4. 自定义工具

3 个文件：`tools/your_tool.py`（函数 + 注册）+ `toolsets.py`（加入列表）+ `/reset`。

```python
registry.register(
    name="your_tool",
    toolset="custom",
    schema={"name": "your_tool", "description": "...", "parameters": {}},
    handler=lambda args, **kw: your_tool(args.get("param")),
    check_fn=check_requirements,
)
```

## 5. 安全机制

- **命令审批**：manual（弹确认）/ smart（LLM 判断）/ off
- **Secret 脱敏**：工具输出中的 Key 自动替换
- **.env 隔离**：密钥不入 config.yaml

---

> **下一步：** [模块 5：技能系统](../05-skills/)
