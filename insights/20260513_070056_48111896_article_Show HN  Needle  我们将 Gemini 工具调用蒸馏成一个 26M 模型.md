# 仓库洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **仓库链接** | https://github.com/cactus-compute/needle |
| **仓库名称** | cactus-compute/needle |
| **项目描述** | 26m function call model that runs on incredibly small devices |
| **星标数** | 271 |
| **Fork数** | 8 |
| **观察者数** | 3 |
| **提交数** | 228 commits |
| **许可证** | MIT |
| **主要语言** | Python (92.7%), CSS (2.5%), JavaScript (2.3%), Shell (1.3%), HTML (1.2%) |
| **组织/所有者** | cactus-compute |
| **创建时间** | 2026年 |
| **主题标签** | gemini, cactus, gemma, on-device-ai, llm |

---

## 项目概述

Needle 是一个实验性的 Simple Attention Networks 项目，旨在重新定义面向消费设备（手机、手表、眼镜等）的轻量级 AI 模型。该项目将 Google 的 Gemini 3.1 蒸馏成一个仅有 26M 参数的"Simple Attention Network"模型，可以直接在 Mac/PC 上进行微调。

在生产环境中，Needle 可在 Cactus 平台上运行，达到 6000 tokens/秒的预填充速度和解码速度 1200 tokens/秒。模型权重完全开放，可在 Hugging Face 上获取。

---

## 技术架构

### 模型规模
- **参数数量**: 26M
- **配置**: d=512, 8H/4KV, BPE=8192

### 训练数据
- **预训练**: 16个 TPU v6e，200B tokens（27小时）
- **后训练**: 2B tokens 单次函数调用数据集（45分钟）

### 核心组件
1. **编码器**: 12层，包含 ZCRMSNorm、自注意力（GQA+RoPE）、门控残差连接（无FFN）
2. **解码器**: 8层，包含 ZCRMSNorm、Masked Self Attention + RoPE、门控残差连接
3. **工具调用**: 专用工具调用输出层
4. **归一化**: ZCRMSNorm（简化版RMSNorm）

---

## 性能表现

Needle 在单次函数调用任务中表现优于以下模型：
- FunctionGemma-270m
- Qwen-0.6B
- Graninte-350m
- LFM2.5-350m

**注意**: 这些对比模型具有更大的参数量和更广泛的应用范围，在对话场景中表现更出色。轻量级模型可能较为敏感，建议通过 Web UI 进行测试和微调。

---

## 快速开始

### 环境准备
```bash
git clone https://github.com/cactus-compute/needle.git
cd needle && source ./setup
```

### 运行 Playground
```bash
needle playground
```
这将打开一个 Web UI（http://127.0.0.1:7860），可在其中测试和微调模型。

### Python 使用示例
```python
from needle import SimpleAttentionNetwork, load_checkpoint, generate, get_tokenizer

params, config = load_checkpoint("checkpoints/needle.pkl")
model = SimpleAttentionNetwork(config)
tokenizer = get_tokenizer()

result = generate(
    model, params, tokenizer,
    query="What's the weather in San Francisco?",
    tools='[{"name":"get_weather","parameters":{"location":"string"}}]',
    stream=False,
)
print(result)
# 输出: [{"name":"get_weather","arguments":{"location":"San Francisco"}}]
```

---

## 命令行工具

| 命令 | 功能 |
|------|------|
| `needle playground` | 通过 Web UI 测试和微调 |
| `needle finetune <data.jsonl>` | 使用自定义数据微调 |
| `needle run --query "..." --tools` | 单次推理 |
| `needle train` | 完整训练流程 |
| `needle pretrain` | 预训练 |
| `needle eval --checkpoint <path>` | 评估检查点 |
| `needle tokenize` | 数据集分词 |
| `needle generate-data` | 通过 Gemini 合成训练数据 |

---

## 资源链接

- **模型权重**: https://huggingface.co/Cactus-Compute/needle
- **官方网站**: https://cactuscompute.com
- **文档**: 包含在 docs 目录中

---

## 总结

Needle 是一个创新性的轻量级 AI 项目，专注于在极小的设备上运行高效的语言模型。通过将大型 Gemini 模型蒸馏成 26M 参数的模型，它为消费设备上的 AI 应用开辟了新的可能性。该项目完全开源，为开发者和研究人员提供了一个可访问的实验平台。

**项目状态**: 活跃开发中，已发布 228 个提交