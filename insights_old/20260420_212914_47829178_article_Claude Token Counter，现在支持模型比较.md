# 洞察报告：Claude Token Counter 模型对比

**洞察链接**: https://simonwillison.net/2026/Apr/20/claude-token-counts/
**发布时间**: 2026年4月20日
**作者**: Simon Willison
**语言**: 英文

---

## 摘要

Simon Willison 更新了他的 Claude Token Counter 工具，新增了不同模型之间的 token 计数对比功能。通过对比发现，Claude Opus 4.7 是首个改变分词器的版本，相比 Opus 4.6 会产生更多的 token。

---

## 关键洞察

### 1. Opus 4.7 与 Opus 4.6 的 token 差异

- Opus 4.7 使用了更新的分词器（tokenizer）
- 相同输入会产生更多 token，约为 **1.0-1.35 倍**
- 实测 Opus 4.7 系统提示词使用了 **7,335 tokens**，而 Opus 4.6 仅为 **5,039 tokens**，增长倍率为 **1.46 倍**

### 2. 成本影响

- Opus 4.7 与 Opus 4.6 定价相同：$5/百万输入 tokens，$25/百万输出 tokens
- 但由于 token 数量增加，预计成本将增加约 **40%**

### 3. 图像处理改进

- Opus 4.7 支持更高分辨率图像：最长边可达 **2,576 像素**（约 375 万像素），是之前模型的三倍以上
- 测试 3456×2234 像素的 3.7MB PNG 图片：
  - Opus 4.7: 4,744 tokens
  - Opus 4.6: 1,578 tokens
  - 增长倍率为 **3.01 倍**
- **注意**：此增长完全是因为 Opus 4.7 能处理更高分辨率。使用低分辨率图像（682×318 像素）时，两者 token 数量基本相同（约 310 tokens）

### 4. PDF 处理

- 测试 15MB、30 页的文本密集型 PDF：
  - Opus 4.7: 60,934 tokens
  - Opus 4.6: 56,482 tokens
  - 增长倍率为 **1.08 倍**，低于纯文本场景

---

## 支持的模型

工具现已支持以下四个模型的对比：
- Claude Opus 4.7
- Claude Opus 4.6
- Claude Sonnet 4.6
- Claude Haiku 4.5

注意：Opus 4.6、Sonnet 4.6 和 Haiku 4.5 使用相同的分词器。

---

## 总结

Claude Opus 4.7 引入了新的分词器，虽然定价与 4.6 相同，但由于 token 数量增加，实际使用成本会更高。不过，Opus 4.7 在图像处理能力上有显著提升，能够处理更高分辨率的图像，这可能为需要处理高质量图像的场景带来优势。