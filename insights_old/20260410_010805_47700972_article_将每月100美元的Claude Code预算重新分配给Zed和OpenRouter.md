# 洞察报告：将每月100美元的Claude Code支出重新分配给Zed和OpenRouter

**洞察链接：** https://braw.dev/blog/2026-04-06-reallocating-100-month-claude-spend/

**发布日期：** 2026年4月6日

**作者：** Braw.dev

**标签：** AI, Claude Code

---

## 摘要

作者将每月100美元的Claude Code订阅费用重新分配：每月支付10美元使用Zed编辑器，并将剩余的90美元用于OpenRouter充值。这种方案提供了更大的灵活性，当不使用时积分可以累积而非像Claude那样浪费使用窗口。

---

## 核心问题

- 作者同时使用Claude Code和Claude桌面应用，每月支付100美元
- 使用模式呈"突发性"，并非全天持续使用
- 经常在编码过程中遇到配额限制，感到沮丧
- 这种情况并非个例，AMD AI高级总监也有类似反馈

---

## 解决方案对比

### Zed编辑器

**价格：** $10/月

**优势：**
- 性能显著优于VSCode及其分支
- 内置Agent Harness，可跟随Agent修改文件
- 支持自定义Agent行为配置
- 通过Agent Client Protocol (ACP)集成Claude Code、Mistral Vibe等工具
- 支持完整的上下文窗口（如Gemini 3.1可达1M tokens）

**劣势：**
- 扩展程序数量远少于VSCode

### OpenRouter

**费用：** 5.5%手续费

**优势：**
- 支持最多数量的模型和提供商
- 预付积分，365天未用才过期
- 不使用时积分可累积、滚动
- 可配置零数据保留(ZDR)端点，降低数据暴露风险
- 支持通过Zed集成使用各种模型

### Cursor

**价格：** $20 | $60 | $200/月

**优势：**
- VSCode分支，兼容所有扩展
- 支持Agent编排模式
- 提供debug模式
- 支持规则精确应用到特定文件（如`*.py`或`**/models.py`）

**劣势：**
- 月费不滚动
- 最低$20/月

### Claude Code + OpenRouter

可以通过配置环境变量让Claude Code使用OpenRouter：

```bash
export OPENROUTER_API_KEY="<your-openrouter-api-key>"
export ANTHROPIC_BASE_URL="https://openrouter.ai/api"
export ANTHROPIC_AUTH_TOKEN="$OPENROUTER_API_KEY"
export ANTHROPIC_API_KEY=""
export ANTHROPIC_DEFAULT_OPUS_MODEL="anthropic/claude-opus-4.6"
export ANTHROPIC_DEFAULT_SONNET_MODEL="anthropic/claude-sonnet-4.6"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="anthropic/claude-haiku-4.5"
```

---

## 作者的最终方案

- **Zed订阅：** $10/月
- **Cursor订阅：** $20/月（观察Cursor 3.0发展）
- **OpenRouter充值：** $70/月（自动充值，积分可累积）

---

## 其他CLI工具推荐

- **OpenCode** (TypeScript)：作者最常使用，支持广泛
- **Crush** (Go)：性能好但配置自定义模型较麻烦

---

## 建议

对于经常遇到Claude限制且想尝试其他模型的用户，建议：
1. 免费开始使用Zed
2. 充值$20到OpenRouter无需订阅
3. 需要Opus时可随时通过OpenRouter调用
