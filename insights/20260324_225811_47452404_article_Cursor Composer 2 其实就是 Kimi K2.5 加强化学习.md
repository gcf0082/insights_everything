# 洞察报告：Cursor Composer 2 底层模型揭秘

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://twitter.com/fynnso/status/2034706304875602030 |
| **发现者** | @fynnso |
| **发现时间** | 2026年3月19日 |
| **相关公司** | Cursor (Anysphere), Moonshot AI (月之暗面) |
| **主题** | AI编程工具底层模型技术揭示 |

## 洞察摘要

开发者 @fynnso 在调试 Cursor 的 OpenAI base URL 时，意外发现 Composer 2 的真实底层模型信息。当设置自定义 OpenAI base URL 并使用 Composer 2 时，系统发送的请求中包含模型 ID：`accounts/anysphere/models/kimi-k2p5-rl-0317-s515-fast`。

这一发现揭示了 Cursor 官方宣称的"自研"Composer 2 模型，实际上是基于中国 AI 公司 Moonshot AI（月之暗面）开发的 Kimi K2.5 模型进行强化学习（RL）后训练的产物。

## 关键发现

### 1. 模型身份确认
- **模型ID**: `kimi-k2p5-rl-0317-s515-fast`
- **基础模型**: Kimi K2.5
- **开发公司**: Moonshot AI (月之暗面)
- **技术特点**: 模型ID中包含 "kimi-k2p5" 和 "RL（强化学习）" 明确标识

### 2. 复现方法
用户可通过以下步骤验证：
1. 托管一个简单的服务器来记录所有请求
2. 将其设置为 OpenAI base URL
3. 使用 Composer 2 功能
4. 观察请求中的模型标识

注：Composer 1.5 会阻止此操作，但 Composer 2 尚未完全阻止（目前已修复）

### 3. 官方回应
- Cursor 官方随后确认 Composer 2 确实以 Kimi K2.5 为基座模型
- Moonshot AI (Kimi) 官方账号在 Twitter 上确认了这一合作关系

## 舆论反应

### 正面观点
- Kimi K2.5 是开源模型（MIT 许可证），Cursor 使用该模型并不违法
- 模型性能表现出色，说明 Kimi K2.5 + RL 的组合效果良好

### 质疑观点
- **Elon Musk 参与调侃**：在社交媒体上公开质疑 Cursor 的"自研"声称
- **许可证问题**：修改后的 MIT 许可证要求商业产品必须明确标注基于 Kimi 2.5
- **透明度问题**：Cursor 在发布 Composer 2 时未提及使用了第三方模型

## 许可证分析

Kimi K2.5 采用修改后的 MIT 许可证，主要要求包括：
- 商业使用如果月收入超过 2000 万美元或月用户超过 1 亿，必须明确标注基于 Kimi 2.5
- Cursor 目前估值约 293 亿美元，据报道月收入超过 1.6 亿美元
- 目前尚不清楚 Cursor 是否完全遵守许可证要求

## 行业影响

这一发现揭示了 AI 编程工具行业的一个重要趋势：
- **模型商品化**：模型本身正在变成可替换的商品，真正的价值捕获在于用户体验和界面设计
- **开源模型的商业应用**：开源 AI 模型正在被广泛用于商业产品
- **透明度重要性**：关于底层模型的透明度对用户信任至关重要

## 参考来源

- 原始推文: https://twitter.com/fynnso/status/2034706304875602030
- Elon Musk 回应: https://twitter.com/elonmusk/status/2034941631871455262
- Kimi 官方确认: https://twitter.com/Kimi_Moonshot/status/2035074972943831491
- Reddit 讨论: https://www.reddit.com/r/LocalLLaMA/comments/1rytksg/

---

*报告生成时间: 2026年3月24日*
