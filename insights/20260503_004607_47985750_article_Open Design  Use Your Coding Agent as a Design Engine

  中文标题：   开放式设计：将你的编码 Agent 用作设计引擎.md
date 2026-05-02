# 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **仓库链接** | https://github.com/nexu-io/open-design |
| **项目名称** | open-design |
| **描述** | Local-first, open-source alternative to Anthropic's Claude Design. 本地优先、可部署到Web的BYOK设计工具，支持多种AI编码代理。 |
| **Stars** | 14.9k |
| **Forks** | 1.7k |
| **License** | Apache-2.0 |
| **主分支** | main |
| **提交数** | 149 |
| **创建时间** | 2026年 |

## 项目概述

Open Design (OD) 是 Anthropic Claude Design 的开源替代方案 Same loop, same artifact-first mental model, none of the lock-in。项目的核心理念是不自带代理——用户已有的最强编码代理已安装在电脑上，通过技能驱动设计工作流运行，支持本地运行和Vercel部署。

## 核心特性

### 支持的编码代理 (12个)

Claude Code、Codex CLI、Devin for Terminal、Cursor Agent、Gemini CLI、OpenCode、Qwen Code、GitHub Copilot CLI、Hermes (ACP)、Kimi CLI (ACP)、Pi (RPC)、Kiro CLI (ACP)

### BYOK回退方案

提供 OpenAI 兼容代理端点 `/api/proxy/stream`，可配置任意 OpenAI 兼容供应商（Anthropic-via-OpenAI、DeepSeek、Groq、MiMo、OpenRouter、自托管vLLM等）。

### 设计系统

内置 **129个** 设计系统：2个手写入门模板 + 70个产品系统（Linear、Stripe、Vercel、Airbnb、Tesla、Notion、Anthropic、Apple、Cursor、Supabase、Figma、Xiaohongshu等）+ 57个设计技能。

### 内置技能 (31个)

- **原型模式 (27个)**：web-prototype、saas-landing、dashboard、mobile-app、gamified-app、social-carousel、magazine-poster、motion-frames、sprite-animation、dating-web、digital-eguide、wireframe-sketch、critique、tweaks等
- **幻灯片模式 (4个)**：guizang-ppt、simple-deck、replit-deck、weekly-update

### 媒体生成

- **gpt-image-2**：海报、头像、信息图表、插图地图
- **Seedance 2.0**：电影级15秒文字转视频和图像转视频
- **HyperFrames**：HTML→MP4动态图形（产品发布、动态排版、数据图表、社交叠加）

### 视觉方向

5个策展方向（Editorial Monocle、Modern Minimal、Warm Soft、Tech Utility、Brutalist Experimental），each ships a deterministic OKLch palette + font stack。

### 设备框架

iPhone 15 Pro、Pixel、iPad Pro、MacBook、Browser Chrome。

### 代理运行时

本地守护进程在项目文件夹中生成CLI，代理获得真实的 Read、Write、Bash、WebFetch 工具。

### 导入功能

支持导入 Claude Design 导出的ZIP文件。

### 持久化

SQLite存储在 `.od/app.sqlite`——projects、conversations、messages、tabs、saved templates。

### 生命周期

单一入口点：`pnpm tools-dev`（start/stop/run/status/logs/inspect/check）。

### 桌面支持

可选的 Electron shell。

### 部署方式

本地运行、Vercel Web层、Electron桌面端。

## 项目架构

```
┌────────────────────── Browser (Next.js 16) ──────────────────────┐
│  chat · file workspace · iframe preview · settings · imports     │
└──────────────┬───────────────────────────────────┬───────────────┘
               │ /api/*                            │
               ▼                                    ▼
   ┌──────────────────────────────────┐   /api/proxy/stream (SSE)
   │  Local daemon (Express + SQLite) │   ─→ any OpenAI-compat
   │                                  │       endpoint (BYOK)
   │  /api/agents, /api/skills        │
   │  /api/design-systems, /api/projects...
   │  /api/chat (SSE), /api/upload    │
   └─────────┬────────────────────────┘
             │ spawn(cli, [...], { cwd: .od/projects/<id> })
             ▼
   ┌──────────────────────────────────────────────────────────────────┐
   │  claude · codex · devin · gemini · opencode · cursor-agent      │
   │  qwen · copilot · hermes · kimi · pi · kiro                      │
   └──────────────────────────────────────────────────────────────────┘
```

### 技术栈

- **前端**：Next.js 16 App Router + React 18 + TypeScript
- **守护进程**：Node 24 + Express + SSE streaming + better-sqlite3
- **代理传输**：child_process.spawn，多种CLI适配器
- **BYOK代理**：OpenAI兼容 /v1/chat/completions
- **存储**：项目目录中的平面文件 + SQLite
- **预览**：沙盒iframe via srcdoc
- **导出**：HTML、PDF、PPTX、ZIP、Markdown
- **桌面**：Electron shell

## 六大地基理念

1. **不自带代理**：用户已有的代理足够好
2. **技能是文件而非插件**：遵循Claude Code的SKILL.md约定
3. **设计系统是便携式Markdown**：9-section DESIGN.md schema
4. **交互式问题表单防止80%重定向**：Turn-1 discovery form
5. **守护进程让代理感觉就在电脑上**：因为确实在
6. **提示栈即产品**：多层可组合

## 快速开始

```bash
git clone https://github.com/nexu-io/open-design.git
cd open-design
corepack enable
pnpm install
pnpm tools-dev run web
```

环境要求：Node ~24 和 pnpm 10.33.x。

首次运行会自动：
1. 检测PATH上的代理CLI并自动选择
2. 加载31个技能 + 72个设计系统
3. 弹出欢迎对话框
4. 自动创建 `./.od/` 目录

## 目录结构

```
open-design/
├── .github/
├── apps/                  # Web + Daemon应用
├── assets/               # 框架资源
├── craft/
├── design-systems/      # 设计系统库
├── docs/
├── e2e/
├── packages/
├── prompt-templates/    # 媒体生成模板
├── scripts/
├── skills/              # 31个技能
├── specs/
├── story/
├── templates/
├── tools/
├── AGENTS.md
├── CHANGELOG.md
├── CLAUDE.md
├── CONTRIBUTING.md
├── LICENSE
├── QUICKSTART.md
├── README.md
└── package.json
```

---

*洞察日期：2025-06-03*  
*数据来源：GitHub仓库页面*