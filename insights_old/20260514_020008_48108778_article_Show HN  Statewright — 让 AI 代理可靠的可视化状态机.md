# 洞察报告：Statewright 项目分析

## 基本信息

| 项目 | 详情 |
|------|------|
| **洞察链接** | https://github.com/statewright/statewright |
| **项目名称** | statewright |
| **全称** | statewright/statewright |
| **描述** | State machine guardrails for AI agents (AI代理的状态机护栏) |
| **创建时间** | 2026-05-03 |
| **主编程语言** | Rust |
| **Star数** | 229 |
| **Fork数** | 4 |
| **开源许可** | Apache 2.0 / FSL-1.1-ALv2 |
| **官网** | https://statewright.ai |

---

## 项目概述

Statewright 是一个AI代理的状态机护栏框架，其核心理念是 **"Agents are suggestions, states are laws"**（代理是建议，状态即法律）。该项目通过状态机来约束AI代理在各阶段可以使用哪些工具，从而提高代理的工作效率和可靠性。

---

## 核心价值主张

### 问题背景
- AI代理功能强大但脆弱
- 给模型40+工具和开放式问题，模型几乎无法有效工作
- 传统解决方案是使用更大的模型和更长的提示词，但效果有限
- 可观测性只能在事后发现问题，无法预防

### 核心方案
**不要让模型变得更大，而是让问题变得更小。**

状态机通过约束工具和解决方案空间，使模型在每个步骤都能在专注的上下文中进行推理：
- 规划阶段：仅提供只读工具
- 实现阶段：解锁编辑工具，限制shell访问（阻止重定向和破坏性操作）
- 测试阶段：仅允许指定的测试命令
- 调用非当前阶段工具时，会被拒绝并告知当前可用工具和状态转换方式

---

## 技术架构

### 核心技术栈
- **核心引擎**：Rust编写，无运行时依赖，可嵌入使用
- **插件层**：通过MCP与各编码代理集成
- **许可证**：Apache 2.0（核心引擎FSL许可，自托管免费）

### 支持的代理

| 代理 | 集成方式 | 强制级别 |
|------|---------|---------|
| Claude Code | Hooks + MCP | 强制（协议层） |
| Codex | Hooks | 强制（alpha） |
| opencode | TypeScript插件 | 强制（alpha） |
| Pi | Skills扩展 | 强制（alpha） |
| Cursor | MCP + rules | 建议性（alpha） |

---

## 护栏功能

| 护栏类型 | 功能说明 |
|---------|---------|
| 状态级工具强制 | 不在`allowed_tools`中的工具对代理不可见 |
| Bash辨别 | 非写入状态下阻止重定向(`>>`)、破坏性操作(`rm`)和脚本解释器 |
| 编辑保护 | 拒绝超过`max_edit_lines`的diff，限制每状态编辑文件数 |
| 命令白名单 | 按状态前缀匹配的`allowed_commands` |
| 条件转换 | 基于上下文数据的编程谓词（eq, gt, exists等） |
| 审批门控 | 高风险转换前暂停等待人工审查 |
| 环境作用域 | 每状态的`blocked_env`和`env_overrides` |
| 会话隔离 | 通过`CLAUDE_SESSION_ID`的会话级状态 |

---

## 研究结果

| 模型 | 大小 | Bug Fix (26行) | SWE-bench (5任务) |
|------|------|---------------|-------------------|
| gemma3 | 3.3GB | FAIL | FAIL |
| gemma4:e2b | 7.2GB | PASS* | FAIL |
| gpt-oss:20b | 13.8GB | PASS | PASS (5/5) |
| gemma4:31b | 19.9GB | PASS | PASS (5/5) |
| llama3.3 | 42.5GB | PASS | PASS (2/2) |

**关键发现**：
- 在5任务SWE-bench子集中，两个模型（13.8GB和19.9GB）**从2/10跃升到10/10**
- 相同任务，相同硬件
- 低于13GB的模型可以产生工具调用，但无法保留足够文件内容来产生准确编辑

---

## 定价策略

| 方案 | 工作流数 | 转换次数/月 | 运行历史 | 价格 |
|------|---------|-------------|---------|------|
| Free | 3 | 200 | 72小时 | $0 |
| Pro | 10 | 2500 | 7天 | $29/月 |
| Team | 30 | 10000 | 90天 | $99/月 |
| Enterprise | 无限 | 无限 | 按规格 | 联系销售 |

---

## 工作流定义示例

```json
{
  "id": "bugfix",
  "initial": "planning",
  "states": {
    "planning": {
      "allowed_tools": ["Read", "Grep", "Glob"],
      "max_iterations": 8,
      "on": { "READY": "implementing" }
    },
    "implementing": {
      "allowed_tools": ["Read", "Edit", "Write"],
      "max_edit_lines": 20,
      "max_files_per_state": 3,
      "on": { "DONE": "testing" }
    },
    "testing": {
      "allowed_tools": ["Read", "Bash"],
      "allowed_commands": ["pytest", "cargo test", "npm test"],
      "on": {
        "PASS": { "target": "completed", "guard": "tests_passed" },
        "FAIL_TEST": "implementing"
      }
    },
    "completed": { "type": "final" }
  }
}
```

---

## 优势与局限

### 优势
- 显著提升本地模型在代码修复任务上的表现
- 打破模型反复读取同一文件5+次而不编辑的读循环死锁
- 减少前沿模型的token消耗
- 核心引擎开源，可自托管

### 局限
- 需要代理支持MCP
- 工作流需要手动编写（虽然可通过`statewright_create_workflow`由代理生成）
- Cursor强制是建议性的，不是强制的
- 研究结果基于5任务SWE-bench子集，非完整2294实例基准
- 工作流过于严格时，代理会卡住（可用`statewright_deactivate`退出）

---

## 技术亮点

1. **确定性执行**：核心引擎评估状态机定义，无LLM参与
2. **协议层拦截**：工具调用在模型看到前就被阻止
3. **Rust高性能**：单进程，无运行时依赖
4. **多代理支持**：覆盖主流编码代理
5. **自托管友好**：个人和团队自托管免费

---

## 总结

Statewright 通过状态机护栏的方式，为AI代理提供了结构化的工作流程约束，有效解决了代理"工具过多导致效率低下"的问题。核心Rust引擎的确定性设计和多代理支持使其具有广泛的应用前景。对于需要提升本地模型编程能力的团队，这是一个值得关注的技术方案。

---

*报告生成时间：2026-05-14*