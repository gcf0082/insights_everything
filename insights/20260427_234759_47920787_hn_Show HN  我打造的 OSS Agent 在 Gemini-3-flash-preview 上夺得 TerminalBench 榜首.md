# 洞察报告：Dirac OSS Agent

**链接：** https://news.ycombinator.com/item?id=47920787

**标题：** Show HN: OSS Agent I built topped the TerminalBench on Gemini-3-flash-preview

**发布时间：** 3小时前

**得分：** 148 points

**评论数：** 56 comments

---

## 项目概述

**Dirac** 是一个开源的 CLI Agent，基于 Cline 分支开发，在 Gemini-3-flash-preview 模型上以 65.2% 的得分登顶 TerminalBench，超越了 Google 官方成绩 (47.8%) 和现有顶级闭源模型 Junie CLI (64.3%)。

---

## 核心技术特性

1. **Hash-Anchored 编辑**：使用优化的哈希锚点编辑进行文件修改
2. **AST 上下文获取**：利用语言 AST 决定需要加载的内容，完全避免大代码文件读取
3. **批量操作**：支持一次性执行大量读取/编辑操作
4. **动态代码执行**：允许模型直接执行 bash/python/perl 脚本分析问题
5. **上下文管理**：主动更新模型可能需要的上下文内容

---

## 支持的语言

项目使用 tree-sitter WASMs，当前支持 **14 种语言**。

---

## 社区讨论要点

### 1. Harness 的重要性

多位开发者指出，**harness（测试框架）对性能的影响远超模型选择**：
- 更换 harness 带来的性能差异大于更换底层模型
- 这解释了为什么 TerminalBench 2.0 最近出现大量作弊行为

### 2. 上下文管理策略

用户 sally_glance 分享了有效的优化经验：
- 修剪旧的工具调用响应
- 截断工具输出
- 自动压缩总结
- 减少上下文的收益似乎大于"记住一切"

### 3. 子代理设计

sally_glance 提出一种新思路：主代理只暴露 run_agent 工具，子代理拥有完整的搜索/执行/获取能力。子代理返回简洁总结可保持父代理上下文清洁。

### 4. 与其他工具的对比

- **vs pi.dev**：Pi-mono 成本第二低，但偶尔会产生不完整的修改，尤其在需要 AST 符号理解的任务中表现不佳
- **vs OpenCode**：在大型代码库重构任务中表现更优

---

## 争议与澄清

作者针对近期 TerminalBench 作弊指控，明确说明：
1. 绝对没有插入任何 agents/skills.md 文件
2. 严格遵守 leaderboard 规则运行
3. 使用完全开源版本进行测试

---

## 安装方式

```bash
npm install -g dirac-cli
```

---

## 总结

Dirac 展示了在 AI 编程助手领域，**harness 设计和工程优化**可以显著提升模型表现。这一发现呼吁业界建立更多针对 harness 本身的基准测试。