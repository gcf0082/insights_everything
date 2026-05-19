# GitHub 仓库洞察报告：Forge

**洞察链接**：https://github.com/antoinezambelli/forge

**报告生成时间**：2025年5月20日

---

## 一、项目基本信息

| 属性 | 值 |
|------|------|
| **项目名称** | Forge |
| **仓库地址** | antoinezambelli/forge |
| **描述** | A Python framework for self-hosted LLM tool-calling and multi-step agentic workflows |
| **星标数** | 203 |
| **分支数** | 11 |
| **主要语言** | Python 96.5%, TypeScript 3.4% |
| **许可证** | MIT |
| **Python版本要求** | 3.12+ |
| **作者** | Antoine Zambelli |

---

## 二、项目概述

Forge 是一个专为自托管大型语言模型（LLM）设计的可靠性层（reliability layer），专注于工具调用（tool-calling）和多步骤代理工作流。该框架通过 guardrails（护栏）机制和上下文管理策略，能够将 8B 参数的本地模型提升到同类模型的顶级水平。

根据项目文档，当前最佳配置（Ministral-3 8B Instruct Q8 在 llama-server 上）在 Forge 的 26 场景评估套件中得分 86.5%，在最困难的层级上得分 76%。

---

## 三、核心功能

### 3.1 WorkflowRunner（工作流运行器）

WorkflowRunner 是 Forge 的核心组件，允许用户定义工具、选择后端、运行结构化代理循环。Forge 会管理完整生命周期，包括：

- 系统提示词（system prompts）
- 工具执行（tool execution）
- 上下文压缩（context compaction）
- 护栏机制（guardrails）

**SlotWorker** 是 WorkflowRunner 的扩展，提供优先级队列访问共享推理槽位，支持自动抢占，适用于多代理架构，其中多个专业工作流共享同一个 GPU 槽位。

### 3.2 Guardrails 中间件

Forge 提供了可组合的中间件形式的可靠性堆栈，用户可以在自己的编排循环中使用。Forge 负责：

- 验证响应（validate responses）
- 抢救格式错误的工具调用（rescue malformed tool calls）
- 强制执行必需的步骤（enforce required steps）

### 3.3 代理服务器（Proxy Server）

Forge 提供了一个即插即用的 OpenAI 兼容代理服务器，可以放置在任何客户端和本地模型服务器之间，透明地应用护栏机制。用户无需修改客户端配置，即可获得 Forge 的可靠性增强。

启动命令：

```bash
# 外部模式 - 您管理 llama-server，Forge 作为代理
python -m forge.proxy --backend-url http://localhost:8080 --port 8081

# 托管模式 - Forge 同时启动 llama-server 和代理
python -m forge.proxy --backend llamaserver --gguf path/to/model.gguf --port 8081
```

---

## 四、支持的后端

| 后端 | 最佳场景 | 原生函数调用 |
|------|----------|--------------|
| **llama-server**（推荐） | 最佳性能，完全控制 | 支持（需 --jinja 参数） |
| **Ollama** | 最简单的设置，内置模型管理 | 支持 |
| **Llamafile** | 单二进制文件，零依赖 | 不支持（需要 prompt 注入） |
| **Anthropic** | 前沿基线，混合工作流 | 支持 |

---

## 五、技术架构

```
src/forge/
├── __init__.py          # 公共 API 导出
├── errors.py            # ForgeError 层次结构
├── server.py            # setup_backend(), ServerManager, BudgetMode
├── core/
│   ├── messages.py      # Message, MessageRole, MessageType, MessageMeta
│   ├── workflow.py      # ToolSpec, ToolDef, ToolCall, TextResponse, Workflow
│   ├── inference.py     # run_inference() — 共享前半部分
│   ├── runner.py        # WorkflowRunner — 代理循环
│   ├── slot_worker.py   # SlotWorker — 优先级队列槽位访问
│   └── steps.py         # StepTracker
├── guardrails/
│   ├── nudge.py         # Nudge 数据类
│   ├── response_validator.py  # ResponseValidator, ValidationResult
│   ├── step_enforcer.py       # StepEnforcer, StepCheck
│   └── error_tracker.py       # ErrorTracker
├── clients/
│   ├── base.py          # ChunkType, StreamChunk, LLMClient 协议
│   ├── ollama.py        # OllamaClient
│   ├── llamafile.py     # LlamafileClient
│   └── anthropic.py     # AnthropicClient
├── context/
│   ├── manager.py       # ContextManager, CompactEvent
│   ├── strategies.py    # CompactStrategy, NoCompact, TieredCompact, SlidingWindowCompact
│   └── hardware.py      # HardwareProfile, detect_hardware()
├── prompts/
│   ├── templates.py     # 工具提示构建器
│   └── nudges.py        # 重试和步骤强制提示模板
├── tools/
│   └── respond.py       # 合成 respond 工具
└── proxy/
    ├── proxy.py         # ProxyServer
    ├── server.py        # 原始 asyncio HTTP 服务器，SSE 流式传输
    ├── handler.py       # 请求处理器
    └── convert.py      # OpenAI 消息 ↔ Forge 消息转换
```

---

## 六、护栏机制详解

Forge 的护栏机制是其核心创新，包括以下几个关键组件：

1. **ResponseValidator（响应验证器）**：验证模型响应是否正确调用了工具，检测格式错误
2. **Rescue Parsing（抢救解析）**：当工具调用格式错误时，尝试修复并重新解析
3. **Retry Nudges（重试提示）**：当模型未正确响应时，提供引导信息促使模型重试
4. **Step Enforcement（步骤强制）**：确保工作流按预期步骤执行
5. **VRAM-Aware Budgets（VRAM 感知预算）**：根据硬件配置管理上下文大小
6. **Tiered Compaction（分层压缩）**：根据重要性分层压缩上下文

---

## 七、评估套件

Forge 包含一个由 26 个场景组成的评估套件，用于衡量模型和后端组合在多步骤工具调用工作流中的可靠性：

- **OG-18 基线层级**：18 个基础场景
- **高级推理层级**：8 个高级场景，用于顶级区分

运行评估：

```bash
# llama-server 评估
python -m tests.eval.eval_runner --backend llamafile --llamafile-mode prompt --gguf "path/to/Ministral-3-8B-Instruct-2512-Q8_0.gguf" --runs 10 --stream --verbose

# 批量评估
python -m tests.eval.batch_eval --config all --runs 50

# 生成报告
python -m tests.eval.report eval_results.jsonl
```

---

## 八、安装和使用

### 安装

```bash
pip install forge-guardrails                # 核心功能
pip install "forge-guardrails[anthropic]"   # 包含 Anthropic 客户端
```

### 快速开始

```python
import asyncio
from pydantic import BaseModel, Field
from forge import (
    Workflow, ToolDef, ToolSpec,
    WorkflowRunner, OllamaClient,
    ContextManager, TieredCompact,
)

def get_weather(city: str) -> str:
    return f"72°F and sunny in {city}"

class GetWeatherParams(BaseModel):
    city: str = Field(description="City name")

workflow = Workflow(
    name="weather",
    description="Look up weather for a city.",
    tools={
        "get_weather": ToolDef(
            spec=ToolSpec(
                name="get_weather",
                description="Get current weather",
                parameters=GetWeatherParams,
            ),
            callable=get_weather,
        ),
    },
    required_steps=[],
    terminal_tool="get_weather",
    system_prompt_template="You are a helpful assistant. Use the available tools to answer the user.",
)

async def main():
    client = OllamaClient(model="ministral-3:8b-instruct-2512-q4_K_M", recommended_sampling=True)
    ctx = ContextManager(strategy=TieredCompact(keep_recent=2), budget_tokens=8192)
    runner = WorkflowRunner(client=client, context_manager=ctx)
    await runner.run(workflow, "What's the weather in Paris?")

asyncio.run(main())
```

---

## 九、相关资源

- [用户指南](docs/USER_GUIDE.md)：使用模式、多轮对话、上下文管理、护栏机制
- [模型指南](docs/MODEL_GUIDE.md)：根据硬件选择模型和后端
- [后端设置](docs/BACKEND_SETUP.md)：后端安装和服务器设置
- [评估指南](docs/EVAL_GUIDE.md)：评估套件 CLI 参考
- [架构文档](docs/ARCHITECTURE.md)：完整设计文档
- [论文](docs/forge_ieee_preprint.pdf)：发表在 ACM 的研究论文

---

## 十、总结

Forge 是一个设计精良的 Python 框架，专为自托管 LLM 的工具调用和多步骤代理工作流提供可靠性保障。其核心创新在于：

1. **护栏机制**：通过响应验证、格式抢救、重试引导和步骤强制，确保工作流稳定执行
2. **上下文管理**：根据硬件配置动态管理上下文，支持 VRAM 感知预算和分层压缩
3. **多后端支持**：兼容 Ollama、llama-server、Llamafile 和 Anthropic
4. **灵活的部署方式**：支持直接集成、作为中间件、或通过代理服务器透明使用

对于希望在本地部署高效可靠的 AI 代理系统的开发者来说，Forge 是一个值得关注和尝试的选择。