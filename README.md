# Hermes Agent 完全学习手册

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status: Active](https://img.shields.io/badge/Status-Active-brightgreen.svg)]()

> 一份面向**所有技术背景**的 Hermes Agent 开源学习教程——从零到精通，图文并茂，循序渐进。

## 📖 这是什么？

Hermes Agent 是 [Nous Research](https://github.com/NousResearch/hermes-agent) 开发的开源 AI Agent 框架。它可以：
- 像人一样**持续运行**，不是一问一答的 chatbot
- **记住你是谁**，跨会话持久化记忆
- **自我学习**，从经验中自动创建可复用的"技能"
- 跑在**任何平台**（飞书、Telegram、Discord、终端……）
- **无人值守**，通过定时任务自动完成工作

这份教程的目标是：**让任何有基本技术背景的人，都能理解 Hermes Agent 的工作原理，并最终能自己搭建、定制、扩展它。**

## 🎯 适用人群

| 你如果是…… | 你能学到…… |
|------------|-----------|
| **后端 / 全栈工程师** | 源码级理解 Agent Loop、扩展工具、编写 Skills |
| **DevOps / SRE** | 部署 Gateway、配置 Cron 自动化、多 Profile 隔离 |
| **AI 爱好者** | 理解 Agent 架构、对比不同框架、快速上手使用 |
| **技术管理者** | 评估 Agent 框架能力边界、理解安全模型 |

**前置要求：** 会用终端、了解基本的 HTTP 请求概念、对"AI 能调用工具"这件事有感性认知。

## 🗺️ 学习路径

```
模块 1 ──→ 模块 2 ──→ 模块 3 ──→ 模块 4 ──→ 模块 5 ──→ 模块 6
  │                                                      │
  └────────── 交叉学习（随时可切入）─────────────────────┘
             模块 7 / 8 / 9 / 10
```

| # | 模块 | 内容 | 状态 |
|---|------|------|------|
| 1 | **[核心概念与架构](modules/01-core-concepts/)** | Agent 是什么？Agent Loop 怎么转？目录结构、数据流 | ✅ 已完成 |
| 2 | **[安装、配置与 Provider](modules/02-install-config/)** | 安装方式、config.yaml、.env、Provider 体系 | ✅ 已完成 |
| 3 | **[系统提示词与人格](modules/03-persona/)** | SOUL.md、Persona 结构、Project Context、Profile 多身份 | ✅ 已完成 |
| 4 | **[工具系统 (Tools)](modules/04-tools/)** | 内置工具全景、Toolsets、自定义工具开发 | ✅ 已完成 |
| 5 | **[技能系统 (Skills)](modules/05-skills/)** ⭐ | Hermes 最独特的能力：自我学习与进化 | ✅ 已完成 |
| 6 | **[记忆系统 (Memory)](modules/06-memory/)** | 跨会话持久记忆、User Profile vs Agent Memory | ✅ 已完成 |
| 7 | **[Gateway 多平台接入](modules/07-gateway/)** | 飞书/Telegram/Discord/Slack 等平台适配 | ✅ 已完成 |
| 8 | **[委托与多智能体](modules/08-delegation/)** | delegate_task、子代理、多实例协作 | ✅ 已完成 |
| 9 | **[定时任务 (Cron)](modules/09-cron/)** | 自动化定时运行、脚本模式、任务链 | ✅ 已完成 |
| 10 | **[进阶特性](modules/10-advanced/)** | Compression、Plugins、MCP、Webhooks、Kanban | ✅ 已完成 |
| 📎 | **[本地安全运行指南](modules/appendix-mac-setup/)** | Mac + Apple Container 隔离环境搭建、API Server 配置 | ✅ 已完成 |

## 📂 项目结构

```
hermes-agent-tutorial/
├── README.md
├── LICENSE
├── modules/
│   ├── 01-core-concepts/     # 模块 1：核心概念与架构
│   ├── 02-install-config/    # 模块 2：安装、配置与 Provider
│   ├── 03-persona/           # 模块 3：系统提示词与人格
│   ├── 04-tools/             # 模块 4：工具系统
│   ├── 05-skills/            # 模块 5：技能系统 ⭐
│   ├── 06-memory/            # 模块 6：记忆系统
│   ├── 07-gateway/           # 模块 7：Gateway 多平台接入
│   ├── 08-delegation/        # 模块 8：委托与多智能体
│   ├── 09-cron/              # 模块 9：定时任务
│   └── 10-advanced/          # 模块 10：进阶特性
│   └── appendix-mac-setup/    # 附录：本地安全运行指南
└── assets/                   # 共享资源
```

每个模块包含 `index.html`（交互式 HTML 版，含深色主题 + Mermaid 图表）。

## 🚀 快速开始

在线浏览（GitHub Pages）：**[hermes-agent-tutorial](https://lucky7jia.github.io/hermes-agent-tutorial/)**

```bash
git clone https://github.com/lucky7jia/hermes-agent-tutorial.git
cd hermes-agent-tutorial

# 浏览器打开任意模块
open modules/01-core-concepts/index.html    # macOS
xdg-open modules/01-core-concepts/index.html # Linux
start modules/01-core-concepts/index.html    # Windows
```

## 🤝 贡献

欢迎提交 Issue 和 PR！如果你发现错误、有更好的解释方式、或者想贡献新模块，请随时参与。

## 📄 许可

MIT License — 自由使用、修改、分发。

---

*本教程由 Hermes Agent 社区维护，与 Nous Research 官方无直接关联。*
