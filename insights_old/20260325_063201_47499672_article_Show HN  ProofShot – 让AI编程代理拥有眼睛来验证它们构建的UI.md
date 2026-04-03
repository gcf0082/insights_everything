# 洞察报告：proofshot

## 基本信息

| 项目 | 内容 |
|------|------|
| **仓库名称** | AmElmo/proofshot |
| **GitHub链接** | https://github.com/AmElmo/proofshot |
| **描述** | Visual verification for AI coding agents（AI编码代理的视觉验证工具） |
| **作者** | AmElmo (Julien Berthomier) |
| **许可证** | MIT |
| **Stars** | 212 |
| **Forks** | 7 |
| **主要语言** | TypeScript 76.8%, MDX 22.9%, JavaScript 0.3% |
| **最新版本** | v1.3.1 (2026年3月10日) |
| **提交数** | 29次 |
| **发布版本数** | 5个 |

## 项目简介

**ProofShot** 是一个开源的、与代理无关的CLI工具，旨在为AI编码代理提供"眼睛"。当代理构建功能时，ProofShot可以录制视频来证明它是否正常工作。

该工具可与多种AI编码代理配合使用：Claude Code、Cursor、Codex、Gemini CLI、Windsurf、GitHub Copilot以及任何运行shell命令的代理。

## 核心功能

### 1. 三步工作流程

- **Start** - 打开浏览器，开始录制，捕获服务器日志
- **Test** - AI代理驱动浏览器进行交互
- **Stop** - 捆绑视频+截图+错误为证明产物

### 2. 输出产物

每个会话在 `./proofshot-artifacts/` 文件夹中生成以下文件：

| 文件 | 描述 |
|------|------|
| `session.webm` | 整个会话的视频录制 |
| `viewer.html` | 独立的交互式查看器，带有进度条、时间线和日志标签页 |
| `SUMMARY.md` | 包含错误、截图和视频的Markdown报告 |
| `step-*.png` | 关键时刻捕获的截图 |
| `session-log.json` | 带时间戳和元素数据的操作时间线 |
| `server.log` | 开发服务器stdout/stderr（使用--run时） |
| `console-output.log` | 浏览器控制台输出 |

### 3. 支持的代理

`proofshot install` 可检测并配置以下工具的技能：

- **Claude Code**: `~/.claude/skills/proofshot/SKILL.md`
- **Cursor**: `~/.cursor/rules/proofshot.mdc`
- **Codex (OpenAI)**: `~/.codex/skills/proofshot/SKILL.md`
- **Gemini CLI**: `~/.gemini/GEMINI.md`
- **Windsurf**: `~/.codeium/windsurf/memories/global_rules.md`

### 4. 主要命令

- `proofshot start` - 启动验证会话
- `proofshot stop` - 停止录制并生成产物
- `proofshot exec` - 传递命令给agent-browser
- `proofshot diff` - 与基线截图进行视觉回归比较
- `proofshot pr` - 上传产物到GitHub PR并发布验证评论
- `proofshot install` - 检测并安装代理技能
- `proofshot clean` - 清理产物目录

### 5. 错误检测

ProofShot可自动从服务器日志中检测错误，支持10+种语言：JavaScript/Node.js、Python、Ruby/Rails、Go、Java/Kotlin、Rust、PHP、C#/.NET、Elixir/Phoenix等。

## 技术架构

- 使用 TypeScript (ESM-only) 开发
- 使用 tsup 进行构建
- 使用 vitest 进行测试
- 基于 Vercel 的 [agent-browser](https://github.com/vercel-labs/agent-browser) 构建

## 安装和使用

```bash
# 安装
npm install -g proofshot
proofshot install

# 使用示例
proofshot start --run "npm run dev" --port 3000 --description "Login form verification"
# ... 代理执行操作 ...
proofshot stop
```

## 适用场景

- 验证AI代理构建的UI功能是否正常工作
- 捕获功能演示视频
- 记录和控制台错误
- 视觉回归测试
- 为Pull Request添加验证评论
