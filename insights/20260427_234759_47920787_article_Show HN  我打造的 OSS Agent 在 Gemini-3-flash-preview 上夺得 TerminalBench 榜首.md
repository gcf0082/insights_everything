# Dirac 仓库洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **仓库链接** | https://github.com/dirac-run/dirac |
| **项目名称** | Dirac |
| **项目描述** | Coding Agent singularly focused efficiency and context curation. Reduces API costs by 50-80% vs other agent AND improves the code quality at the same time. |
| **官方网站** | https://dirac.run/ |
| **Stars** | 344 |
| **Forks** | 17 |
| **主要语言** | TypeScript (95.9%) |
| **许可证** | Apache License 2.0 |
| **贡献者** | Dirac Delta Labs (Max Trivedi) |
| **分支** | master |
| **提交数** | 167 |
| **Issues** | 8 |

---

## 项目概述

Dirac 是一个专注于效率优化和上下文管理的开源编程代理（AI Coding Agent）。该项目源于 Cline 项目，通过多项技术创新实现了**成本降低64.8%**的同时提高代码质量。

---

## 核心特性

### 1. Hash-Anchored Edits（哈希锚定编辑）
- 使用稳定的行哈希精确定位编辑位置
- 避免传统行号编辑的"翻译丢失"问题
- 大幅提升代码修改的准确性

### 2. AST-Native Precision（AST 原生精确度）
- 内置对 TypeScript、Python、C++ 等语言的语法理解
- 支持函数提取、类重构等结构化操作
- 实现 100% 精确度的代码结构操作

### 3. Multi-File Batching（多文件批处理）
- 支持在单次 LLM 调用中处理多个文件
- 显著降低延迟和 API 成本
- 提高大规模代码修改效率

### 4. High-Bandwidth Context（高带宽上下文）
- 优化的上下文整理机制
- 确保 LLM 始终获得最相关信息
- 避免无效的 token 消耗

### 5. Autonomous Tool Use（自主工具使用）
- 读写文件、执行终端命令
- 使用无头浏览器
- 审批制工作流程保持用户控制

---

## 性能评估（Evals）

### TerminalBench 2.0 排行榜
- **Dirac 得分：65.2%**（使用 gemini-3-flash-preview）
- 超越 Google 官方基线 47.6%
- 超越顶级闭源代理 Junie CLI 64.3%

### 基准测试对比（8个真实重构任务）

| 指标 | Cline | Kilo | Ohmypi | Opencode | Pimono | Roo | **Dirac** |
|------|-------|------|--------|----------|--------|-----|-----------|
| 成功率 | 5/8 | 5/8 | 6/8 | 8/8 | 6/8 | 6/8 | **8/8** |
| 平均成本 | $0.49 | $0.73 | $0.51 | $0.44 | $0.38 | $0.60 | **$0.18** |

**Dirac 是唯一一个在所有8个任务中都取得成功的代理**，同时成本最低（$0.18）。

### 测试仓库
- Hugging Face Transformers
- Microsoft VS Code
- Django

---

## 安装方式

### VS Code 扩展
从 [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=dirac-run.dirac) 安装。

### CLI 终端工具
```bash
npm install -g dirac-cli
```

---

## 快速使用

```bash
# 1. 认证
dirac auth

# 2. 运行第一个任务
dirac "Analyze the architecture of this project"

# 计划模式（查看策略后再执行）
dirac -p "prompt"

# Yolo 模式（自动批准所有操作）
dirac -y "prompt"

# 管道输入
git diff | dirac "Review these changes"

# 查看历史任务
dirac history
```

### 环境变量配置
支持多种 API Key 配置：
- `ANTHROPIC_API_KEY`
- `OPENAI_API_KEY`
- `OPENROUTER_API_KEY`
- `GEMINI_API_KEY`
- `GROQ_API_KEY`
- `MISTRAL_API_KEY`
- `XAI_API_KEY`
- `HF_TOKEN`

---

## 技术栈

- **主要语言**: TypeScript (95.9%)
- **辅助语言**: JavaScript (2.8%), Shell (0.7%), CSS (0.4%)
- **构建工具**: esbuild
- **代码质量**: Biome, TypeScript
- **协议**: gRPC (protobuf)

---

## 项目结构

```
dirac/
├── agent-registry/     # 代理注册表
├── cli/               # 命令行工具
├── evals/             # 性能评估测试
├── locales/           # 本地化文件
├── proto/             # Protocol Buffers 定义
├── scripts/           # 构建脚本
├── src/               # 源代码
├── standalone/        # 独立运行文件
├── webview-ui/        # Web UI
└── walkthrough/       # 入门教程
```

---

## 核心优势总结

1. **成本效益**: 相比竞争对手降低 64.8% 的 API 成本
2. **准确性**: 100% 完成所有基准测试任务
3. **无 MCP**: 直接集成，无中间层开销
4. **多语言支持**: 支持 TypeScript、Python、C++ 等主流语言
5. **开源透明**: Apache 2.0 许可证，完全开放源代码

---

## 洞察结论

Dirac 是一个在 AI 编程代理领域具有突破性创新的项目。通过哈希锚定编辑、AST 操作、多文件批处理等核心技术，它成功解决了长上下文导致的推理能力退化问题，在保持代码质量的同时大幅降低成本。

该项目适合：
- 需要优化 AI 编程工具成本的开发团队
- 追求高代码质量准确性的企业
- 希望集成开源 AI 编程代理的开发者

**GitHub**: https://github.com/dirac-run/dirac
**官网**: https://dirac.run/