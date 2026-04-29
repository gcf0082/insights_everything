# 洞察报告：evanflow

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://github.com/evanklem/evanflow |
| **项目名称** | evanflow |
| **描述** | A TDD-driven iterative feedback loop for software development. 16 cohesive Claude Code skills walk an idea from brainstorm → plan → execute → iterate, with checkpoints throughout. |
| **所有者** | evanklem |
| **星标数** | 273 |
| **Fork数** | 6 |
| **主要语言** | Shell 100% |
| **许可证** | MIT |
| **默认分支** | main |

---

## 项目概述

Evanflow 是一个面向 Claude Code 的 TDD（测试驱动开发）驱动迭代反馈循环框架。该项目提供 16 个高度内聚的技能（skills）和 2 个自定义子代理（subagents），将一个想法从头脑风暴逐步推进到规划、执行和迭代，并在整个过程中设置检查点，让用户始终保持控制权。

核心工作流程：

```
brainstorm → plan → execute (vertical-slice TDD per task) → iterate → STOP
```

用户只需说"let's evanflow this"，Orchestrator 就会启动整个循环。

---

## 核心特性

### 1. 反馈循环机制

- **Brainstorm（头脑风暴）**：澄清意图，提出 2-3 种方法并进行嵌入式压力测试（grill），需用户批准设计
- **Plan（规划）**：首先规划文件结构，遵循深度模块化原则，需用户批准计划
- **Execute（执行）**：逐任务执行并内联验证，阻塞问题会停止循环并反馈给用户
- **Iterate（迭代）**：重新阅读差异，运行质量检查，针对 UI 变化进行截图验证，并执行"五大失败模式"检查清单，最多 5 次迭代上限
- **STOP**：报告完成情况，等待用户指示，代理从不自动提交代码

### 2. TDD 集成

TDD 不是执行后的独立阶段，而是嵌入每个代码编写任务中的纪律。执行是任务跟踪、阻塞和质量检查的框架，而 `evanflow-tdd` 是内部运行机制。

垂直切片 TDD 原则：
- 每个周期完整的 RED → GREEN → REFACTOR
- 一个失败的测试 → 最小实现 → 重构（测试仍然新鲜时）
- 通过公共接口验证行为

### 3. 并行处理能力

对于包含 3 个或以上真正独立单元的计划，循环会分叉为并行 coder/overseer 编排：
- 每个单元一个 coder（使用垂直切片 TDD）
- 每个 coder 一个 overseer（只读审查子代理）
- 一个集成 overseer 在每个触点运行命名集成测试

### 4. 内置硬规则

- **永不发明值**：文件路径、环境变量、ID、函数名、库 API 等，如不确定则停止询问
- **断言正确性警告**：超过 62% 的 LLM 生成测试断言不正确
- **五大失败模式检查**：幻觉行为、范围蔓延、级联错误、上下文丢失、工具误用
- **上下文漂移监控**：在干净边界和漂移症状时触发
- **永不自动提交**：每次代理自行集成时都集成了错误的内容

---

## 技能体系

### 默认循环（5个技能）

| 技能 | 用途 |
|------|------|
| `evanflow-brainstorming` | 澄清意图，提出 2-3 种方法并进行压力测试 |
| `evanflow-writing-plans` | 首先规划文件结构，小任务分解，内置压力测试 |
| `evanflow-executing-plans` | 逐任务执行并内联验证，然后停止 |
| `evanflow-tdd` | 垂直切片 TDD，通过公共接口验证行为 |
| `evanflow-iterate` | 自我审查循环，运行质量检查，五大失败模式检查 |

### 特殊用途（8个技能）

| 技能 | 用途 |
|------|------|
| `evanflow-go` | **单一入口点**，说"let's evanflow this"即可启动整个循环 |
| `evanflow-glossary` | 将规范领域术语提取到 `CONTEXT.md` |
| `evanflow-improve-architecture` | 通过删除测试和深度模块化揭示重构机会 |
| `evanflow-design-interface` | "设计两次"，生成 3+ 个并行子代理进行比较 |
| `evanflow-debug` | 根本原因纪律，明确假设，修复前压力测试，先写失败测试 |
| `evanflow-review` | 代码评审的双面（给出 + 接收） |
| `evanflow-prd` | 从现有上下文综合 PRD |
| `evanflow-qa` | 对话式 bug 发现 → issue 草稿 |

### 跨领域（1个技能）

| 技能 | 用途 |
|------|------|
| `evanflow-compact` | 长会话上下文管理，干净边界的主动摘要策略 |

### 元技能（1个技能）

| 技能 | 用途 |
|------|------|
| `evanflow` | 索引，共享词汇表和何时调用每个 evanflow-* 技能 |

### 自定义子代理（2个）

| 子代理 | 工具限制 | 用途 |
|--------|----------|------|
| `evanflow-coder` | Read, Edit, Write, Glob, Grep, Bash, TodoWrite | 实现子代理，工具和系统提示防止 git 操作、范围外编辑、值幻觉 |
| `evanflow-overseer` | Read, Grep, Glob（无 Edit/Write/Bash） | 只读审查子代理，工具物理强制"报告发现，从不修复" |

### 捆绑钩子

`hooks/block-dangerous-git.sh` — PreToolUse 钩子，阻止破坏性 git 操作（`git push`、`git reset --hard`、`git clean -f`、`git branch -D`、`git checkout .`、`git restore .`）。

---

## 安装方式

### 方式1：Claude Code 插件市场（推荐）

```
/plugin marketplace add evanklem/evanflow
/plugin install evanflow@evanflow
```

### 方式2：npx CLI

```bash
npx skills@latest add evanklem/evanflow -s '*' -y
```

### 方式3：手动复制

```bash
git clone https://github.com/evanklem/evanflow.git
cd evanflow
mkdir -p .claude/skills
cp -r skills/* .claude/skills/
mkdir -p .claude/agents
cp agents/*.md .claude/agents/
mkdir -p .claude/hooks
cp hooks/block-dangerous-git.sh .claude/hooks/
chmod +x .claude/hooks/block-dangerous-git.sh
```

---

## 技术基础

项目整合了以下来源的思想：

- **mattpocock/skills** by Matt Pocock — 垂直切片 TDD、深度模块化、删除测试、设计两次、 ubiquitous language、grill-me、caveman
- **superpowers** by Jesse Vincent — 验证后再完成、代码评审模式、并行代理调度、完成开发分支（4 选项演示）
- **git-guardrails-claude-code** — 捆绑在 hooks/ 中

行业研究支持的设计：

- Anthropic's 2026 Agentic Coding Trends Report
- 9 Critical Failure Patterns of Coding Agents (DAPLab, Columbia)
- Test-Driven Development for Code Generation (arXiv 2402.13521) — 断言正确性发现

---

## 适用场景

Evanflow 适用于需要高质量代码输出的开发团队，特别是：

1. 重视代码质量和测试覆盖的项目
2. 需要在 AI 辅助开发中保持人类控制权的团队
3. 追求结构化、可重复开发流程的开发者
4. 希望避免 AI 常见失败模式（如幻觉、范围蔓延、上下文丢失）的项目

---

## 总结

Evanflow 是一个设计精良的 Claude Code 开发框架，通过 16 个技能和 2 个子代理，将 TDD 原则深度嵌入到 AI 辅助开发的每个环节。其核心价值在于：

1. **结构化的反馈循环**：从头脑风暴到迭代，每个阶段都有检查点
2. **用户始终控制**：不自动提交代码，等待用户指令
3. **内置质量保证**：通过硬规则和检查清单防止常见 AI 失败模式
4. **灵活的扩展性**：支持并行开发，可根据项目需求定制

该项目获得了 273 个星标，表明其在开发者社区中获得了相当程度的认可。对于希望将 AI 辅助开发与高质量代码标准结合的团队来说，Evanflow 提供了有价值的框架和实践。