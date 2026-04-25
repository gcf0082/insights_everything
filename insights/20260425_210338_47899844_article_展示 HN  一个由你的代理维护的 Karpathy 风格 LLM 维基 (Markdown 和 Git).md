# 洞察报告

- **链接**: https://github.com/nex-crm/wuphf
- **项目名称**: WUPHF
- **描述**: Slack for AI employees with a shared brain
- **星标数**: 252
- **Fork数**: 11
- **编程语言**: Go 75.2%, TypeScript 14.6%, CSS 3.7%
- **许可证**: MIT
- **最新版本**: v0.74.2
- **提交数**: 696

## 项目概述

WUPHF 是一个为AI员工打造的协作办公平台，带有共享知识库。通过一个命令启动，AI员工（CEO、PM、工程师、设计师等）可以协同工作、分配任务并交付成果。

## 核心特性

### 1. 多AI协作者
- 支持 Claude Code、Codex CLI 和 OpenClaw 多种AI代理
- 每个AI有独立角色（CEO、PM、工程师、设计师等）
- DM模式加载4个工具，全办公室模式加载27个工具

### 2. 共享记忆系统
- **Notebook**: 每个代理的私人笔记
- **Wiki**: 团队共享知识库（支持 markdown、nex、gbrain 后端）
- LLM/wiki 循环：代理记录事实→合成阈值触发→提交至git仓库

### 3. 推送驱动的代理唤醒
- 无心跳轮询，零空闲消耗
- 代理仅在收到通知时唤醒
- 每次代理轮次全新启动，无累积上下文

### 4. 实时可见性
- Web UI 实时显示代理工作状态
- 支持 stdout 流式输出
- Mid-task steering：可在任务中途DM代理调整

### 5. 集成支持
- **Telegram桥接**: 双向消息同步
- **OpenClaw桥接**: 将现有OpenClaw代理引入办公室
- **动作提供者**: One CLI（本地）或 Composio（云端）

## 技术亮点

### 提示缓存
- 97% 缓存命中率（Claude API）
- 每轮固定约87k输入 tokens，缓存后约40k
- 10轮总计约286k tokens

### 命令行使用

```bash
npx wuphf                    # 快速启动
wuphf --memory-backend markdown  # 使用markdown知识库（默认）
wuphf --pack starter         # 使用初始代理包
wuphf --tui                 # 使用tmux TUI
wuphf --1o1                 # 与CEO一对一
```

### 目录结构

| 目录 | 用途 |
|------|------|
| `.cursor/rules` | Cursor规则 |
| `cmd` | 命令行入口 |
| `internal` | 核心逻辑 |
| `mcp` | MCP工具 |
| `prompts` | 提示模板 |
| `web` | Web前端 |
| `website` | 文档网站 |
| `tests` | 测试 |

## 使用场景

1. **AI团队协作**: 多AI角色协同完成项目
2. **知识管理**: 团队知识积累与传承
3. **自动化工作流**: 代理自动执行任务
4. **外部集成**: 连接Telegram、OpenClaw等

## 总结

WUPHF 是一个创新性的多AI协作平台，通过共享记忆、推送驱动和实时可见性等特性，实现了真正可观测的AI团队工作。相比传统对话式AI，它提供了一种更接近人类团队协作的工作模式。