# 洞察报告

**链接**: https://news.ycombinator.com/item?id=47890841

**标题**: Show HN: Browser Harness – Gives LLM freedom to complete any browser task

**来源**: Hacker News

**发布日期**: 2026-04-24

**GitHub 仓库**: https://github.com/browser-use/browser-harness

**星标数**: 6.6k

---

## 项目概述

Browser Harness 是一个极简的、自我修复的浏览器控制工具，能够赋予 LLM 完全的自由度来完成任何浏览器任务。该项目直接构建在 CDP（Chrome DevTools Protocol）之上，不使用任何框架，让 LLM 能够直接通过 WebSocket 控制 Chrome 浏览器。

## 核心特性

1. **最大化自由度**: 移除了传统浏览器框架的限制，给予 LLM 最大程度的自由来执行浏览器任务
2. **自我修复能力**: 当 LLM 发现缺少必要的工具函数时，可以直接在任务执行过程中编写并添加新工具
3. **极简架构**: 整个项目仅约 592 行 Python 代码，包括：
   - `install.md` - 首次安装和浏览器配置
   - `SKILL.md` - 日常使用指南
   - `run.py` (~36 行) - 运行入口
   - `helpers.py` (~195 行) - 基础工具函数，LLM 可编辑
   - `admin.py` + `daemon.py` (~361 行) - CDP WebSocket 守护进程

4. **直接基于 CDP**: LLM 可以直接使用 Chrome DevTools Protocol，无需中间层

## 与其他方案的对比

该项目与其他浏览器自动化方案（如 Playwright MCP、browser-use CLI、agent-browser、chrome devtools MCP）的核心区别在于：

- 传统方案：将 Chrome 包装成一套预定义的函数供 LLM 调用
- **最严重的失败模式是静默失败**：LLM 的 `click()` 返回成功，但实际上该网站什么都没发生，LLM 继续基于错误的世界模型执行任务
- Browser Harness 提供最大自由度和完美的上下文，让 LLM 理解工具的实际工作原理

## 新范式

**SKILL.md + 几个可以动态更改的 Python helpers**

LLM 可以根据任务需求实时修改 helpers.py 来添加缺失的功能。

## 安装方式

通过 Claude Code 安装：

```
Set up https://github.com/browser-use/browser-harness for me.
```

然后按照 `install.md` 安装并连接浏览器，再阅读 `SKILL.md` 了解日常使用方法。

## 未来展望

作者提到，这个理念最终会被一个能够在操作系统层面提供自由度的 harness 所取代。

---

*报告生成时间: 2026-04-25*