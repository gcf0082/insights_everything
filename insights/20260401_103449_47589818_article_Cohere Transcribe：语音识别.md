# 洞察报告：Cohere Transcribe

## 基本信息

- **洞察链接**: https://cohere.com/blog/transcribe
- **发布时间**: 2026年3月26日
- **来源**: Cohere官方博客
- **主题**: Cohere Transcribe - 最先进的开源语音识别模型

---

## 核心摘要

Cohere正式发布Transcribe，这是一款最先进的自动语音识别（ASR）模型，采用开源方式发布。该模型专注于在实际应用条件下最大限度降低词错误率（WER），不仅是一个研究产物，更是一个为日常使用而设计的生产级系统。

---

## 模型概述

| 属性 | 详情 |
|------|------|
| **模型名称** | cohere-transcribe-03-2026 |
| **架构** | 基于Conformer的编码器-解码器 |
| **输入** | 音频波形 → log-Mel频谱图 |
| **输出** | 转录文本 |
| **模型规模** | 2B参数 |
| **许可证** | Apache 2.0 |

**技术实现**：大型Conformer编码器提取声学表征，轻量级Transformer解码器生成token。从零开始训练，使用标准的监督交叉熵损失函数。

**支持语言**：涵盖14种语言，包括欧洲语言（英语、法语、德语、意大利语、西班牙语、葡萄牙语、希腊语、荷兰语、波兰语）、亚太语言（中文普通话、日语、韩语、越南语）以及中东语言（阿拉伯语）。

---

## 性能表现

### 准确率（Accuracy）

Cohere Transcribe在HuggingFace Open ASR Leaderboard上排名第一，平均词错误率仅为**5.42%**，优于所有开源和闭源的专用ASR替代方案，包括Whisper Large v3、ElevenLabs Scribe v2和Qwen3-ASR-1.7B。

| 测试数据集 | Cohere Transcribe | Whisper Large v3 | 优势 |
|-----------|-----------------|-----------------|------|
| 平均WER | **5.42** | 7.44 | -2.02 |
| AMI | **8.13** | 15.95 | -7.82 |
| Earnings 22 | **10.86** | 11.29 | -0.43 |
| LS clean | **1.25** | 2.01 | -0.76 |
| SPGISpeech | **3.08** | 2.94 | +0.14 |

### 人工评估

人工评估结果同样验证了模型的优势。在英语转录的成对比较中，Cohere Transcribe的平均胜率为61%，显著优于所有竞争对手。

### 多语言表现

在非英语语言的人工评估中，Cohere Transcribe同样表现出色：
- 日语：70%胜率
- 意大利语：60%胜率
- 法语：51%胜率
- 德语：44%胜率
- 西班牙语：48%胜率
- 葡萄牙语：48%胜率

---

## 吞吐量性能

Cohere Transcribe在保持最先进准确率的同时，还实现了最佳的吞吐量（RTFx）。在1B+参数模型中，它在准确率和速度方面均处于领先地位，完美平衡了性能与效率。

---

## 获取方式

1. **开源下载**：从HuggingFace下载模型权重自行部署
   - 地址：https://huggingface.co/CohereLabs/cohere-transcribe-03-2026
2. **API访问**：通过Cohere API免费试用（受速率限制）
3. **Model Vault**：企业级私有云推理服务，无速率限制，按小时计费

---

## 未来规划

Cohere计划将Transcribe与North（企业AI代理编排平台）进行更深层次的整合，使Cohere Transcribe从高准确率转录模型演变为更广泛的Enterprise Speech Intelligence基础。

---

## 主要贡献者

- Julian Mack（技术成员）
- Ekagra Ranjan（技术成员）
- Cassie Cao（产品经理）
- Bharat Venkitesh（技术经理）
- Pierre Harvey Richemond（技术经理）