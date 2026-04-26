# 洞察报告：browser-use/browser-harness

## 基本信息

- **洞察链接**: https://github.com/browser-use/browser-harness
- **项目名称**: Browser Harness
- **项目描述**: Self-healing harness that enables LLMs to complete any task
- ** starred**: 6.6k
- ** forks**: 586
- **主要语言**: Python 100.0%
- **License**: MIT license

## 项目概述

Browser Harness 是一个极简的、自我修复的浏览器控制工具，旨在赋予大型语言模型（LLM）完成任何浏览器任务的完全自由度。它直接建立在 CDP（Chrome DevTools Protocol）之上，不依赖任何框架或预定义的工作流程。

## 核心特性

1. **极简设计**: 约 592 行 Python 代码
2. **自我修复能力**: 代理可以在任务执行过程中编写缺失的辅助函数
3. **无框架约束**: 没有预设的 rails 或配方，代理拥有完全的自由
4. **直接基于 CDP**: 通过 WebSocket 连接到 Chrome，无中间层

## 核心文件结构

| 文件 | 行数 | 用途 |
|------|------|------|
| install.md | - | 首次安装和浏览器配置 |
| SKILL.md | - | 日常使用指南 |
| run.py | ~36 | 运行预加载了辅助函数的 Python 代码 |
| helpers.py | ~195 | 起始工具函数（代理可编辑） |
| admin.py + daemon.py | ~361 | 守护进程启动、CDP WebSocket 和套接字桥接 |

## 技术原理

 Browser Harness 的核心创新在于其"自我修复"机制：

```
  ● agent: wants to upload a file
  │
  ● helpers.py → upload_file() missing
  │
  ● agent edits the harness and writes it
  │                                                       + upload_file()
  ✓ file uploaded
```

当代理在执行任务时发现缺少某个功能，它可以即时编辑 harness 代码来补充实现该功能，然后继续执行任务。

## 远程浏览器

项目提供免费的远程浏览器服务，适用于：
- 隐身浏览
- 子代理部署
- 避免本地浏览器被检测

免费额度：3 个并发浏览器、代理、验证码解决功能。

## 应用场景

- LinkedIn 外展联系
- Amazon 购物下单
- 费用报销提交
- 各类需要浏览器自动化的任务

## 贡献指南

欢迎提交 PR 和改进建议。最佳的贡献方式是：

1. 在 domain-skills/ 目录下贡献一个新的领域技能
2. 技能应由代理生成，而非手动编写
3. 当代理在执行任务时发现非显而易见的解决方案时，它会自动生成技能文件

## 相关资源

- [The Bitter Lesson of Agent Harnesses](https://browser-use.com/posts/bitter-lesson-agent-harnesses)
- [Web Agents That Actually Learn](https://browser-use.com/posts/web-agents-that-actually-learn)
- 官方网站: https://browser-use.com

## 总结

Browser Harness 代表了一种全新的浏览器自动化范式——赋予 AI 代理完全的自由度和自我修复能力，使其能够在执行复杂任务时动态创建和修改所需的工具函数。相比传统的预定义工作流方式，这种"边做边学"的模式更加灵活和强大。