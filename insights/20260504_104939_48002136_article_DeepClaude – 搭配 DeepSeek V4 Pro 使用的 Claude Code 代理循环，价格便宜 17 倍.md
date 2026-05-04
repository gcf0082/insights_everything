# 洞察报告：deepclaude

**洞察链接**：https://github.com/aattaran/deepclaude

**项目名称**：deepclaude
**作者**：aattaran
** License**：MIT
**星标数**：170
**分支数**：9

## 项目概述

deepclaude 是一个开源项目，允许用户使用 Claude Code 的自主代理循环，但将 Anthropic 的 API 后端替换为 DeepSeek V4 Pro、OpenRouter 或任何 Anthropic 兼容的后端。用户可以获得与 Claude Code 相同的用户体验，但成本降低约 17 倍。

## 核心功能

1. **成本大幅降低**：使用 DeepSeek V4 Pro 的成本为每百万输出 tokens 0.87 美元，而 Anthropic 的 Claude Opus 为每百万输出 tokens 15 美元
2. **保持原有的 Claude Code 体验**：所有工具（文件读取/编辑、bash 执行、子代理生成、多步骤自主编码循环）均可正常工作
3. **支持多个后端**：DeepSeek（默认）、OpenRouter、Fireworks AI、Anthropic 原生
4. **实时切换**：支持在 Claude Code 会话中随时使用斜杠命令切换模型，无需重启
5. **远程控制支持**：可通过浏览器远程使用 Claude Code + DeepSeek

## 支持的后端对比

| 后端 | 标志 | 输入tokens费用 | 输出tokens费用 | 服务器位置 |
|---|---|---|---|---|
| DeepSeek（默认） | --backend ds | $0.44/M | $0.87/M | 中国 |
| OpenRouter | --backend or | $0.44/M | $0.87/M | 美国 |
| Fireworks AI | --backend fw | $1.74/M | $3.48/M | 美国 |
| Anthropic | --backend anthropic | $3.00/M | $15.00/M | 美国 |

## 费用对比

| 使用级别 | Anthropic Max | deepclaude (DeepSeek) | 节省 |
|---|---|---|---|
| 轻量级（10天/月） | $200/月（封顶） | ~$20/月 | 90% |
| 重负载（25天/月） | $200/月（封顶） | ~$50/月 | 75% |
| 自动循环 | $200/月（封顶） | ~$80/月 | 60% |

## 功能支持情况

### 正常工作的功能
- 文件读取、写入、编辑（Read/Write/Edit 工具）
- Bash/PowerShell 执行
- Glob 和 Grep 搜索
- 多步骤自主工具循环
- 子代理生成
- Git 操作
- 项目初始化（/init）
- 思考模式

### 不支持或退化的功能
- 图像/视觉输入：DeepSeek 的 Anthropic 端点不支持图像
- 并行工具使用：工具一次执行一个
- MCP 服务器工具：不通过兼容性层支持
- 提示缓存：DeepSeek 有自己的缓存（自动），但会忽略 Anthropic 的 cache_control

## 技术实现

deepclaude 通过设置以下环境变量来工作：
- `ANTHROPIC_BASE_URL`：API 端点
- `ANTHROPIC_AUTH_TOKEN`：API 密钥
- `ANTHROPIC_DEFAULT_OPUS_MODEL`：Opus 级任务的模型名称
- `ANTHROPIC_DEFAULT_SONNET_MODEL`：Sonnet 级任务的模型名称
- `ANTHROPIC_DEFAULT_HAIKU_MODEL`：Haiku 级（子代理）的模型名称
- `CLAUDE_CODE_SUBAGENT_MODEL`：子代理的模型

## 项目文件结构

- `deepclaude.sh`：Linux/macOS 启动脚本
- `deepclaude.ps1`：Windows PowerShell 启动脚本
- `proxy/`：代理服务器代码
- `screenshots/`：截图
- `LICENSE`：MIT 许可证
- `README.md`：项目文档

## 语言构成

- JavaScript：46.8%
- PowerShell：27.3%
- Shell：25.9%

## 总结

deepclaude 是一个实用的工具，允许开发者以极低的成本使用 Claude Code 的强大代理功能。对于日常编码任务，DeepSeek V4 Pro 的表现与 Claude Opus 相当，可节省高达 90% 的费用。对于复杂推理任务，用户可以随时切换回 Anthropic 后端。这是一个适合预算有限但需要高效 AI 编码助手的开发者的解决方案。