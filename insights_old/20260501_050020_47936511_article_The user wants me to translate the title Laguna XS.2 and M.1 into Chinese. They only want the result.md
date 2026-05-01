# Poolside Laguna 模型深度解析

**洞察链接**: https://poolside.ai/blog/laguna-a-deeper-dive  
**发布日期**: 2026-04-28  
**报告生成日期**: 2025-05-01  
**原文作者**: Poolside Team

---

## 1. 执行摘要

Poolside 发布了 Laguna 系列的首两款模型——Laguna M.1 和 Laguna XS.2，同时开源了用于训练和运行 Agent 的运行时环境。这两款模型是专为长期任务设计的 Agentic 编程模型。Laguna M.1 是目前最强大的模型，在 SWE-bench Pro 上达到 46.9%，在 Terminal-Bench 2.0 上达到 40.7%。Laguna XS.2 是首款开源权重模型，在 SWE-bench Pro 上达到 44.5%。

## 2. 核心模型

### 2.1 Laguna M.1（225B-A23B）

- **架构**: 225B 总参数的 MoE（混合专家）模型，23B 激活参数
- **训练数据**: 30T tokens，纯自研，从零训练
- **硬件**: 6,144 块 NVIDIA Hopper GPU 互联
- **性能表现**:
  - SWE-bench Pro: 46.9%
  - Terminal-Bench 2.0: 40.7%
  - SWE-bench Verified: 72.5%
  - SWE-bench Multilingual: 67.3%

### 2.2 Laguna XS.2（33B-A3B）

- **架构**: 33B 总参数的 MoE 模型，3B 激活参数
- **训练数据**: 30T tokens
- **权重许可**: Apache 2.0（开源）
- **性能表现**:
  - SWE-bench Pro: 44.5%
  - Terminal-Bench 2.0: 30.1%
  - SWE-bench Verified: 68.2%
  - SWE-bench Multilingual: 62.4%

## 3. 技术创新

### 3.1 数据管道与自动化混合

- **大规模网络数据**: 采用质量与多样性的联合优化，将质量建模为连续多维信号，使用复合评分排名数据。保留部分中低质量数据桶以保持多样性，这对泛化至关重要。
- **合成数据**: 约占最终训练混合的 13%，整个 Laguna 系列使用约 4.4T+ 合成 tokens。
- **AutoMixer**: 自动数据混合优化框架，训练约 60 个代理模型测试不同数据混合，测量关键能力组（代码、数学、STEM、常识）的表现。

### 3.2 Muon 优化器

- 相比 AdamW 基线，训练损失相同但步骤减少约 15%，最终模型评估有明显提升。
- 内存需求低于 AdamW（每个参数只需一个状态而非两个）。
- 通过分布计算和通信重叠，在 Laguna M.1 预训练中，优化器开销小于训练步骤时间的 1%。

### 3.3 Agent 强化学习

- **异步在线 RL 系统**: 使用 Agentic Harness 在大规模真实软件工程、终端和工具集成推理任务中进行训练。
- **异步 rollout**: Actor 和训练器独立运行，通过队列缓冲和显式门控管理 off-policy 程度。
- **权重传输**: 自定义 GPUDirect RDMA 方案，5 秒内跨节点传输数百 GB 权重。

## 4. 产品与生态

### 4.1 可用渠道

- **API**: Laguna M.1 和 Laguna XS.2 限时免费
- **OpenRouter**: 支持
- **Ollama**: 支持 Laguna XS.2
- **权重下载**: Apache 2.0 许可

### 4.2 NVIDIA 合作

- 整个 Laguna 系列在 NVIDIA 硬件上训练和部署
- Laguna XS.2 在 TensorRT-LLM 上首发支持
- 提供 NVFP4 版本，支持 NVIDIA Blackwell 架构

### 4.3 Agent Harness（pool）

- 同一 Agent 客户端协议（ACP）服务器
- 用于 Agent RL 训练和评估
- 作为研究预览版发布

## 5. 关键洞察

1. **Agent 交互范式转变**: 当前大多数 Agent 通过工具调用与世界交互（固定接口），而软件是更具表达力的界面。能够编写和执行代码的 Agent 可以组合动作、并行工作、构建自己的 ad-hoc 系统。

2. **开源权重战略**: Poolside 相信西部需要强大的开源权重模型，希望为生态系统做出贡献。开源是改进模型的最快方式。

3. **数据多样性重要性**: 全球去重会不成比例地移除高质量数据，通过匹配质量分布可进一步缩小性能差距。

4. **性能基准**: 所有基准测试���用 Harbor Framework，max 500 步，sandboxed execution（8GB RAM/2 CPUs）。

## 6. 结论

Poolside 通过 Laguna M.1 和 Laguna XS.2 展示了其在 Agentic 编程模型领域的创新。凭借自研的训练栈、大规模数据管道、Muon 优化器和异步 RL 系统，Poolside 建立了强大的技术基础。开源 XS.2 权重标志着其支持开放生态系统的承诺。