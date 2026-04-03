# Hypura 洞察报告

## 基本信息

- **洞察链接**: https://github.com/t8/hypura
- **项目名称**: Hypura
- **项目描述**: 运行超过Mac内存大小的模型
- **星标数**: 181
- **分支数**: 5
- **主要语言**: Rust (91.8%)
- **许可证**: MIT
- **最新版本**: v0.1.0 (2026年3月17日)

---

## 项目概述

Hypura 是一个针对 Apple Silicon 设计的存储层级感知的 LLM 推理调度器。它根据访问模式、带宽成本和硬件能力，将模型张量分布在 GPU、RAM 和 NVMe 层级上，使得超过物理内存的模型也能运行而不会崩溃系统。

### 核心能力

- **在 32GB Mac Mini 上运行 31GB Mixtral 8x7B**: 2.2 tok/s
- **运行 40GB Llama 70B**: 0.3 tok/s
- **Vanilla llama.cpp**: 在以上两种情况下都会崩溃

---

## 技术原理

### 1. 分层存储策略

Hypura 读取 GGUF 文件，分析硬件配置（GPU 工作集、RAM、NVMe 带宽），然后解决优化问题将每个张量分配到合适的层级：

- **GPU (Metal)**: 注意力层、归一化层、嵌入层
- **RAM**: 溢出的层，通过 mmap 访问
- **NVMe**: 剩余层通过按需加载和预取

### 2. 自动推理模式选择

Hypura 根据模型大小、架构和可用内存自动选择最佳推理模式：

- **Full-resident**: 模型适合 GPU+RAM，无 NVMe I/O
- **Expert-streaming**: 针对 MoE 模型，只在 GPU 保留非专家张量
- **Dense FFN-streaming**: 针对大型密集模型

### 3. MoE 优化

- MoE 稀疏性利用：每 token 只激活 8 个专家中的 2 个
- 路由器拦截识别选中的专家
- 神经缓存追踪跨 token 加载的专家切片
- 协同激活追踪预测下一个专家
- 实现 99.5% 缓存命中率

---

## 性能表现

| 模型 | 大小 | GPU | NVMe | 模式 | Hypura | llama.cpp |
|------|------|-----|------|------|--------|-----------|
| Qwen 2.5 14B Q4_K_M | 8.4 GB | 8.4 GB | - | full-resident | 21 tok/s | ~21 tok/s |
| Mixtral 8x7B Q5_K_M | 30.9 GB | 1.1 GB | 29.8 GB | expert-streaming | 2.2 tok/s | OOM |
| Llama 3.3 70B Q4_K_M | 39.6 GB | 7.8 GB | 31.8 GB | dense-FFN-streaming | 0.3 tok/s | OOM |

**关键发现**：
- 对于适合内存的模型，Hypura 零开销
- 对于不适合内存的模型，Hypura 是"运行"与"崩溃"的区别
- Expert-streaming 在 Mixtral 上实现可交互速度

---

## 安装与使用

```bash
git clone --recurse-submodules https://github.com/hypura/hypura.git
cd hypura
cargo build --release
```

### 快速开始

```bash
# 硬件分析（运行一次）
hypura profile

# 运行推理
hypura run ./model.gguf --prompt "Hello, world"

# 交互式聊天
hypura run ./model.gguf --interactive

# 基准测试
hypura bench ./model.gguf

# 检查模型放置计划
hypura inspect ./model.gguf
```

---

## Ollama 兼容服务器

Hypura 提供 Ollama 兼容的 HTTP API：

```bash
hypura serve ./model.gguf
# Endpoint: http://127.0.0.1:8080
# Ollama-compatible API: /api/generate, /api/chat, /api/tags
```

---

## 架构

Hypura 是一个 Cargo 工作空间，包含两个 crate：

- **hypura**: 主二进制和库
- **hypura-sys**: llama.cpp 的 FFI 绑定

### 关键模块

- `scheduler/placement.rs`: 张量层级放置
- `compute/inference.rs`: 推理引擎
- `compute/nvme_backend.rs`: 自定义 GGML 缓冲区，神经缓存
- `server/routes.rs`: Ollama 兼容 API
- `profiler/`: 硬件检测

---

## 安全说明

- 模型超过 RAM 减去 4GB 时阻止基准测试
- 始终从 `--max-tokens 10` 开始测试
- 不会损坏 SSD（仅读取，不写入）

---

## 总结

Hypura 是一个创新的 LLM 推理优化工具，通过分层存储策略使得超过 Mac 物理内存的大型模型能够运行。它利用 MoE 稀疏性和按需加载技术，在 Apple Silicon 设备上实现了突破性的模型运行能力。