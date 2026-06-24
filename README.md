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
| 1 | **核心概念与架构** | Agent 是什么？Agent Loop 怎么转？目录结构、数据流 | ✅ 已完成 |
| 2 | 安装、配置与 Provider | 安装方式、config.yaml、.env、Provider 体系 | 🚧 待完成 |
| 3 | 系统提示词与人格 (Persona) | SOUL.md、Persona 定义、Profile 系统 | 🚧 待完成 |
| 4 | 工具系统 (Tools) | 内置工具全景、Toolsets、自定义工具开发 | 🚧 待完成 |
| 5 | 技能系统 (Skills) ⭐ | Hermes 最独特的能力：自我学习与进化 | 🚧 待完成 |
| 6 | 记忆系统 (Memory) | 跨会话持久记忆、User Profile vs Agent Memory | 🚧 待完成 |
| 7 | Gateway 多平台接入 | 飞书/Telegram/Discord/Slack 等平台适配 | 🚧 待完成 |
| 8 | 委托与多智能体 (Delegation) | delegate_task、子代理、多实例协作 | 🚧 待完成 |
| 9 | 定时任务 (Cron Jobs) | 自动化定时运行、脚本模式、任务链 | 🚧 待完成 |
| 10 | 进阶特性 | Compression、Plugins、MCP、Webhooks、Kanban | 🚧 待完成 |

## 📂 项目结构

```
hermes-agent-tutorial/
├── README.md                       # 你正在看的这个文件
├── modules/
│   ├── 01-core-concepts/           # 模块 1：核心概念与架构
│   │   ├── README.md               #   Markdown 版（GitHub 友好）
│   │   └── index.html              #   交互式 HTML 版（图表 + 动画）
│   ├── 02-install-config/          # 模块 2（待完成）
│   └── ...
└── assets/                         # 共享资源
```

每个模块提供**两种阅读方式**：
- **Markdown**：适合在 GitHub 上直接浏览，纯文本
- **HTML**：适合本地浏览器打开，含交互式 SVG 图表、深色主题

## 🚀 快速开始

```bash
# 1. 克隆项目
git clone https://github.com/YOUR_USER/hermes-agent-tutorial.git
cd hermes-agent-tutorial

# 2. 阅读模块 1
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
