# 洞察报告

## 基本信息

- **洞察链接**: https://teamchong.github.io/turboquant-wasm/draw.html
- **生成时间**: 2026-04-20
- **工具**: TurboQuant Prompt → Diagram

---

## 内容总结

### 项目概述

TurboQuant Prompt → Diagram 是一个基于 WebGPU 的浏览器端图表生成工具，它利用 Gemma 4 E2B 模型根据用户的文字描述生成 Excalidraw 格式的图表。

### 核心技术特点

1. **模型与压缩算法**
   - 使用 Gemma 4 E2B 模型生成图表
   - 采用 TurboQuant 算法（极坐标 + QJL 方法）对 KV 缓存进行压缩
   - 压缩比达到约 2.4 倍，使更长的对话能够适配 GPU 显存

2. **性能表现**
   - 在 GPU 上通过 WGSL 计算着色器运行
   - 推理速度可达 30+ token/秒
   - 需要约 3 GB 内存（移动端浏览器限制更低）

3. **技术实现**
   - LLM 输出紧凑的代码（约 50 个 token），而非原始 Excalidraw JSON（约 5,000 个 token）
   - 需要支持 WebGPU 子组的浏览器（仅限桌面版 Chrome 134+）
   - Safari/iOS 暂不支持

4. **相关产品**
   - 同期项目 `turboquant-wasm` npm 包实现了相同算法的 WASM+SIMD 版本，适用于 CPU 端的向量搜索

### 功能特性

该工具支持生成多种类型的图表，包括：
- OAuth 2.0 授权码流程序列图
- 全栈应用 OAuth 流程图
- CI/CD 管道图
- TCP 状态机
- 工程组织架构图
- 电子商务模式图
- 支付类图
-  Checkout 泳道图

用户可以通过 Cmd+Enter 快捷键生成图表，也可以编辑和渲染代码。

---

## 结论

TurboQuant Prompt → Diagram 是一个创新的浏览器端 AI 图表生成工具，通过 TurboQuant 算法优化了内存使用，使得在浏览器中运行大语言模型生成图表成为可能。该项目展示了 WebGPU 在客户端 AI 应用中的潜力。
