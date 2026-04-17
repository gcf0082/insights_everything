# 洞察报告

**洞察链接：** https://android-developers.googleblog.com/2026/04/build-android-apps-3x-faster-using-any-agent.html

**发布日期：** 2026年4月16日

**作者：** Adarsh Fernando（集团产品经理）、Esteban de la Canal（高级软件工程师）

---

## 概述

Google发布了全新的Android CLI工具和资源，旨在帮助开发者使用AI代理（Agent）将Android应用开发速度提升3倍。这一工具套件包括Android CLI、Android Skills和Android Knowledge Base，适用于在Android Studio之外使用各种代理（如Gemini CLI、Claude Code、Codex等）进行开发的场景。

---

## 核心内容

### 1. Android CLI（命令行工具）

重新设计的Android CLI作为终端开发的主要接口，提供以下核心功能：

- **SDK管理：** 使用`android sdk install`命令下载特定组件，保持开发环境精简
- **快速项目创建：** `android create`命令从官方模板生成项目，确保从第一行代码就遵循推荐架构和最佳实践
- **快速设备创建和部署：** 使用`android emulator`创建虚拟设备，使用`android run`部署应用
- **可更新性：** 运行`android update`获取最新功能

**性能提升：** 根据内部实验，Android CLI将LLM token使用量减少超过**70%**，任务完成速度**提升3倍**。

### 2. Android Skills（技能库）

传统文档偏重概念和原理，而LLM需要精确、可操作的指令。Android Skills通过模块化、基于Markdown的指令集（`SKILL.md`）提供技术规范，当提示词与技能元数据匹配时可自动触发。

**首批技能包括：**
- Navigation 3设置和迁移
- 边到边显示（Edge-to-edge）支持实现
- AGP 9和XML到Compose迁移
- R8配置分析

可通过`android skills`命令浏览和设置技能，也可与第三方技能共存。

### 3. Android Knowledge Base（知识库）

通过`android docs`命令访问，也已集成到最新版本的Android Studio中。这一专业数据源使代理能够搜索和获取最新的权威开发者指南，确保代理的回答基于最新信息。

知识库涵盖：Android开发者文档、Firebase、Google Developers和Kotlin文档的最新内容。

### 4. 与Android Studio的集成

- 可以先用Agent通过Android CLI快速启动原型，然后在Android Studio中精细调整UI
- Android Studio内置强大的Agent和Planning Mode
- AI驱动的New Project流程可快速原型化Android应用构思

---

## 总结

这些新工具使开发者能够：
1. 在任何环境中开始Android开发（终端、CLI、第三方代理）
2. 确保代理遵循最新推荐模式和最佳实践
3. 无缝过渡到Android Studio进行高级开发和调试

**获取方式：** 访问 d.android.com/tools/agents 下载Android CLI（预览版）