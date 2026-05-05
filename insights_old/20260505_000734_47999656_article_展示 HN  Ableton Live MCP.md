# 洞察报告：Ableton Live MCP

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://github.com/bschoepke/ableton-live-mcp |
| **项目名称** | ableton-live-mcp |
| **作者** | bschoepke |
| **描述** | 通用用途的Ableton Live MCP桥接工具 |
| **星标数** | 113 |
| **Fork数** | 1 |
| **许可证** | MIT |
| **主要语言** | Python 94.1%, Max 4.5%, JavaScript 1.4% |

## 项目概述

这是一个用于Ableton Live的MCP（Model Context Protocol）服务器，可以让AI代理（如Codex、Claude Code、Cursor、Copilot、 Gemini等）直接控制Ableton Live音乐制作软件。

## 核心功能

1. **通用控制能力**：可以通过Ableton的对象模型执行任意Python代码，实现几乎所有可能的操作
2. **预定义工具**：为常见任务提供了专用工具，执行更快、更可靠
3. **Agent Audio Tap设备**：内置Max for Live设备，允许代理在信号处理链的任意点捕获音频，实现混音和母带任务的完整反馈循环

## 典型应用场景

- 创建MIDI clips
- 插入音频文件
- 查询Live Set信息（代理可以查看整个现场演出设置）
- 添加带有不同设备和效果的轨道
- 分析和声与音频信号
- 生成频谱图
- 剪辑自动化
- 设置母带或人声处理链
- 从网络获取MIDI并插入

## 支持平台

- macOS和Windows
- 支持近期的Ableton版本
- 测试环境：Ableton Live Suite 12.3.8 on macOS Tahoe

## 注意事项

⚠️ 使用前请备份您的Live Set。MCP可以直接编辑您的演出设置，存在损坏风险。

## 演示示例

项目提供了多个视频演示，展示了几分钟内用Codex从头创建完整音乐曲目的过程。

## 许可证

MIT许可证