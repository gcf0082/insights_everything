# Ollama MLX 洞察报告

## 基本信息

- **链接**: https://ollama.com/blog/mlx
- **发布日期**: 2026年3月30日
- **摘要**: Ollama 借助 Apple MLX 框架在 Apple Silicon 上实现预览版加速

---

## 核心洞察

### 1. MLX 驱动性能提升

Ollama 现已基于 Apple 的机器学习框架 MLX 构建，充分利用其统一内存架构，在所有 Apple Silicon 设备上实现显著加速。在 M5、M5 Pro 和 M5 Max 芯片上，Ollama 利用新的 GPU 神经加速器提升首批令牌响应时间（TTFT）和生成速度。

性能测试数据（使用 Qwen3.5-35B-A3B 模型）：
- **预填充性能**: 1810 tokens/s（Ollama 0.19）vs 1154 tokens/s（Ollama 0.18）
- **解码性能**: 112 tokens/s（Ollama 0.19）vs 58 tokens/s（Ollama 0.18）

### 2. NVFP4 支持：更高质量与生产环境一致性

Ollama 现在采用 NVIDIA 的 NVFP4 格式，在减少内存带宽和存储需求的同时保持模型精度。这使得 Ollama 用户能够获得与生产环境一致的推理结果，也为使用 NVIDIA 模型优化器优化的模型打开了大门。

### 3. 改进的缓存机制提升响应速度

- **更低内存占用**: 跨对话复用缓存，减少内存使用
- **智能检查点**: 在提示词的关键位置存储缓存快照，加快响应
- **更智能的淘汰策略**: 共享前缀在旧分支被清除时仍能保留更久

### 4. 预览版支持

当前预览版支持 Qwen3.5-35B-A3B 模型，专门针对编码任务进行了采样参数调优。建议使用具有 32GB 以上统一内存的 Mac。

启动命令：
```
ollama launch claude --model qwen3.5:35b-a3b-coding-nvfp4
ollama launch openclaw --model qwen3.5:35b-a3b-coding-nvfp4
ollama run qwen3.5:35b-a3b-coding-nvfp4
```

---

## 致谢

感谢 MLX 贡献者团队、NVIDIA 贡献者、GGML 与 llama.cpp 团队、阿里 Qwen 团队。

---

*报告生成时间: 2026年3月31日*