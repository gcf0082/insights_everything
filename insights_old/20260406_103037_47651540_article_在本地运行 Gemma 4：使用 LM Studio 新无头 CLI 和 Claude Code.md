# 洞察报告：使用 LM Studio 本地运行 Google Gemma 4 与 Claude Code

## 基本信息

- **洞察链接**: https://ai.georgeliu.com/p/running-google-gemma-4-locally-with
- **来源**: George Liu (Substack)
- **发布日期**: 2026年4月4日
- **主题**: 本地运行 Google Gemma 4 模型 + LM Studio 0.4.0 无头 CLI + Claude Code 集成
- **目标硬件**: 14英寸 MacBook Pro M4 Pro (48GB 统一内存)

---

## 为什么要在本地运行模型？

云端 AI API 虽然好用，但存在以下问题：
- **速率限制**：使用量受限于 API 提供商的配额
- **费用成本**：持续使用会产生费用
- **隐私风险**：数据需要上传到第三方服务器
- **网络延迟**：依赖网络连接质量

在本地运行模型的优势：
- **零 API 费用**：完全免费使用
- **数据隐私**：所有数据保留在本地
- **稳定可用**：不受网络或 API 配额限制

---

## Gemma 4 模型系列

Google 发布了四个版本的 Gemma 4 模型：

| 模型 | 参数量 | 架构类型 | 特点 |
|------|--------|----------|------|
| E2B | 2B | Per-Layer Embeddings | 支持音频输入，设备端优化 |
| E4B | 4B | Per-Layer Embeddings | 支持音频输入 |
| 26B-A4B | 26B总/4B激活 | MoE (混合专家) | 128专家+1共享专家，每token激活8个专家(3.8B参数) |
| 31B | 31B | Dense (密集) | 最强性能，MMLU Pro 85.2%，AIME 2026 89.2% |

### 为什么选择 26B-A4B？

- **混合专家架构**：总参数26B，但每次前向传播只激活4B参数
- **高效推理**：推理成本相当于4B密集模型，质量却接近10B级别
- **Benchmark表现**：MMLU Pro 82.6%，AIME 2026 88.3%
- **内存友好**：在48GB MacBook Pro上运行速度达51 tokens/秒
- **功能完整**：256K最大上下文、视觉支持、函数调用、推理模式

---

## LM Studio 0.4.0 新特性

### 核心变化：llmster 引擎

0.4.0 版本将桌面应用的核心推理引擎提取为独立的 **llmster** 服务器，支持完全命令行操作。

### 新增功能

1. **llmster 守护进程**：后台服务，管理模型加载和推理
2. **lms CLI**：完整的命令行界面，用于下载、加载、聊天和服务模型
3. **并行请求处理**：连续批处理，支持多请求并发
4. **有状态 REST API**：新的 `/v1/chat` 端点维护对话历史
5. **MCP 集成**：支持 Model Context Protocol

---

## 安装与配置

### 安装 lms CLI

```bash
# Linux/Mac
curl -fsSL https://lmstudio.ai/install.sh | bash

# Windows
irm https://lmstudio.ai/install.ps1 | iex
```

### 启动守护进程

```bash
lms daemon up

# macOS 需要更新推理运行时
lms runtime update llama.cpp
lms runtime update mlx
```

---

## 下载与运行 Gemma 4

### 下载模型

```bash
lms get google/gemma-4-26b-a4b
```

- 默认下载：Q4_K_M 量化版本
- 文件大小：17.99 GB

### 查看已下载模型

```bash
lms ls
```

### 启动交互式聊天

```bash
lms chat google/gemma-4-26b-a4b --stats
```

**性能数据**：
- 生成速度：51.35 tokens/秒
- 首 token 延迟：1.551秒
- 总 token 数：215

---

## 内存使用分析

### 不同上下文长度的内存需求

| 上下文长度 | GPU 内存 | 总内存 |
|-----------|----------|--------|
| 4K | ~17.6 GiB | ~17.6 GiB |
| 48K | ~21 GiB | ~21 GiB |
| 128K | ~27 GiB | ~27 GiB |
| 200K | ~32 GiB | ~32 GiB |
| 256K | ~37.48 GiB | ~37.48 GiB |

### 硬件建议

- **48GB Mac**：可运行48K-256K上下文，推荐48K-128K
- **64GB+ Mac**：可轻松运行256K上下文
- 基础模型占用约17.6 GiB，每翻倍上下文增加3-4 GiB

---

## 模型调优参数

### 1. 上下文长度

```bash
lms load google/gemma-4-26b-a4b --context-length 128000

# 先预估内存
lms load google/gemma-4-26b-a4b --estimate-only --context-length 48000
```

### 2. GPU 卸载

```bash
# 自动 (默认)
lms load google/gemma-4-26b-a4b --gpu=auto

# 100% GPU
lms load google/gemma-4-26b-a4b --gpu=1.0
```

### 3. 并行请求

- 默认支持2个并行请求
- 每个并行槽位消耗额外内存
- 在48GB机器上，2个并行槽位+48K上下文是较好平衡

### 4. TTL (自动卸载)

```bash
# 30分钟空闲后自动卸载
lms load google/gemma-4-26b-a4b --ttl 1800
```

### 5. 注意事项

**不推荐**：Gemma 4 不支持投机解码
- MoE 模型在验证时需要加载所有专家的并集
- 会导致内存带宽使用暴增，实际可能更慢

**推荐**：启用 Flash Attention
- 减少 KV 缓存内存使用
- 可在相同内存下支持更长上下文

---

## API 服务

### 启动本地服务器

```bash
lms server start
```

- 默认地址：`http://localhost:1234/v1`
- OpenAI 兼容格式
- Anthropic 兼容格式：`POST /v1/messages`

### 监控日志

```bash
lms log stream --source model --stats
```

### 特点

- 支持 JIT 模型加载：自动加载请求的模型
- 网络访问：可从局域网其他设备访问
- 认证支持：可启用 API 令牌认证

---

## 集成 Claude Code

### 配置方法

在 `~/.zshrc` 中添加以下函数：

```bash
claude-lm() {
    export ANTHROPIC_BASE_URL=http://localhost:1234
    export ANTHROPIC_AUTH_TOKEN=lmstudio
    export ANTHROPIC_MODEL="gemma-4-26b-a4b"
    export CLAUDE_CODE_AUTO_COMPACT_WINDOW="48000"
    export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE="90"
    export API_TIMEOUT_MS="30000000"
    export BASH_DEFAULT_TIMEOUT_MS="2400000"
    export CLAUDE_CODE_MAX_OUTPUT_TOKENS="8000"
    export CLAUDE_CODE_DISABLE_1M_CONTEXT="1"
    export CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING="1"
    claude "$@"
}
```

### 使用方法

```bash
source ~/.zshrc
claude-lm
```

### 关键配置说明

- `ANTHROPIC_BASE_URL`：指向本地 LM Studio 服务器
- `ANTHROPIC_MODEL`：强制使用 Gemma 4
- `CLAUDE_CODE_AUTO_COMPACT_WINDOW`：48K上下文，90%触发压缩
- `API_TIMEOUT_MS`：30秒，本地推理比云端慢
- 禁用 1M 上下文和自适应思考：本模型不支持

---

## 系统监控数据

在 M4 Pro (4 E核心 + 10 P核心，20 GPU核心) 上运行 Gemma 4：

- **内存使用**：46.69 GB / 48 GB (38.07 GB 绑定内存)
- **Swap 使用**：27.49 GB
- **GPU 利用率**：90%，频率 1.45 GHz
- **CPU 利用率**：E核心 82.42%，P核心 35.96%
- **温度**：CPU 91°C，GPU 92.46°C
- **功耗**：总计 23.56W (CPU 11.06W + GPU 13.32W)

---

## 关键经验总结

### ✅ 成功经验

1. **MoE 模型是本地推理的甜点**：26B-A4B 以4B推理成本提供接近10B的质量
2. **无头守护进程改变工作流**：`lms daemon up` 可后台运行，适合服务器部署
3. **上下文长度是主要内存变量**：模型本身固定占用17.6 GiB
4. **`--estimate-only` 防止 OOM**：加载前务必检查预估
5. **Anthropic 兼容端点是游戏改变者**：Claude Code 可无缝切换本地/云端

### ❌ 未达预期

1. **模型身份识别**：Gemma 4 不会自报名称，回答"你是什么模型"时只说"AI助手"
2. **上下文保守**：默认48K对于支持256K的模型来说偏低
3. **非完美替代**：复杂多步骤任务受限于本地模型能力
4. **内存压力**：48GB机器在运行模型时接近满载，有27GB swap

---

## 快速开始指南

```bash
# 1. 安装
curl -fsSL https://lmstudio.ai/install.sh | bash

# 2. 启动守护进程
lms daemon up

# 3. 下载 Gemma 4
lms get google/gemma-4-26b-a4b

# 4. 本地聊天
lms chat google/gemma-4-26b-a4b --stats

# 5. Claude Code 集成
# 将 claude-lm 函数添加到 ~/.zshrc，然后运行 claude-lm
```

---

## 后续计划

作者正在测试其他本地模型：
- **Qwen 3.5 35B**：编程任务
- **GLM 4.7 Flash**：快速草稿
- **Nemotron 3 Nano**：结构化提取

计划发布对比文章，分析各模型的最佳使用场景。