# DeepSeek API 文档洞察报告

## 基本信息

- **洞察链接**: https://api-docs.deepseek.com/
- **报告日期**: 2026-04-24
- **数据来源**: DeepSeek 官方API文档

## 概述

DeepSeek API 是一个与 OpenAI/Anthropic 兼容的 API 接口，通过修改配置即可使用 OpenAI/Anthropic SDK 或兼容软件访问 DeepSeek API。

## 核心参数

| 参数 | 值 |
|------|-----|
| base_url (OpenAI) | https://api.deepseek.com |
| base_url (Anthropic) | https://api.deepseek.com/anthropic |
| api_key | 需要在 [DeepSeek Platform](https://platform.deepseek.com/api_keys) 申请 |

## 可用模型

- **deepseek-v4-flash** - 快速响应模型
- **deepseek-pro** - 专业版模型
- **deepseek-chat** (将于2026/07/24弃用)
- **deepseek-reasoner** (将于2026/07/24弃用)

**注意**: deepseek-chat 和 deepseek-reasoner 将于2026/07/24弃用，为保持兼容性，它们分别对应 deepseek-v4-flash 的非思考模式和思考模式。

## Chat API 调用示例

### cURL

```bash
curl https://api.deepseek.com/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${DEEPSEEK_API_KEY}" \
  -d '{
        "model": "deepseek-v4-pro",
        "messages": [
          {"role": "system", "content": "You are a helpful assistant."},
          {"role": "user", "content": "Hello!"}
        ],
        "thinking": {"type": "enabled"},
        "reasoning_effort": "high",
        "stream": false
      }'
```

### Python

```python
from openai import OpenAI

client = OpenAI(
    api_key=os.environ.get("DEEPSEEK_API_KEY"),
    base_url="https://api.deepseek.com"
)

response = client.chat.completions.create(
    model="deepseek-v4-pro",
    messages=[
        {"role": "system", "content": "You are a helpful assistant"},
        {"role": "user", "content": "Hello"},
    ],
    stream=False,
    reasoning_effort="high",
    extra_body={"thinking": {"type": "enabled"}})
```

### Node.js

```javascript
const openai = new OpenAI({
    baseURL: 'https://api.deepseek.com',
    apiKey: process.env.DEEPSEEK_API_KEY,
});

const completion = await openai.chat.completions.create({
    messages: [{ role: "system", content: "You are a helpful assistant." }],
    model: "deepseek-v4-pro",
    thinking: {"type": "enabled"},
    reasoning_effort": "high",
    stream: false,
});
```

## 主要功能特性

- **思考模式 (Thinking Mode)**: 支持启用深度思考功能
- **多轮对话**: 支持连续对话上下文
- **Chat Prefix Completion (Beta)**: 聊天前缀补全
- **FIM Completion (Beta)**: 完整代码补全
- **JSON输出**: 支持结构化JSON输出
- **工具调用**: 支持工具调用功能
- **上下文缓存**: 支持KV缓存优化
- **集成编码代理**: 支持与编码代理集成

## 相关资源

- Discord: https://discord.gg/Tc7c45Zzu5
- Twitter: @deepseek_ai
- GitHub: https://github.com/deepseek-ai
- Email: api-service@deepseek.com