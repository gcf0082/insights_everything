# Anthropic (Claude) 洞察报告

## 基本信息

- **洞察链接**: https://docs.openclaw.ai/providers/anthropic
- **内容来源**: OpenClaw 官方文档
- **主题**: Anthropic (Claude) 模型提供商的集成与配置
- **生成时间**: 2026-04-21

---

## 概述

Anthropic 构建了 **Claude** 模型系列，通过 API 和 Claude CLI 提供访问支持。在 OpenClaw 中，Anthropic API 密钥和 Claude CLI 重用均被支持。原有的遗留 Anthropic token 配置在运行时仍然有效。

---

## 核心功能

### 1. 两种认证方式

| 方式 | 描述 | 适用场景 |
|------|------|----------|
| Anthropic API 密钥 | 标准 API 访问，按量计费 | 长期网关主机、生产环境 |
| Claude CLI 重用 | 直接复用主机上的 Claude CLI 登录 | 快速测试、本地开发 |

**重要说明**: Anthropic 员工确认 OpenClaw 风格的 Claude CLI 使用已被允许，因此 OpenClaw 将 Claude CLI 重用和 `claude -p` 使用视为该集成的合规操作。

### 2. 快速模式 (Fast Mode)

OpenClaw 的共享 `/fast` 开关支持直接面向 `api.anthropic.com` 的流量：

- `/fast on` → `service_tier: "auto"`
- `/fast off` → `service_tier: "standard_only"`

### 3. 提示缓存 (Prompt Caching)

OpenClaw 支持 Anthropic 的提示缓存功能（仅 API）。

| 缓存时长 | 配置值 | 说明 |
|-----------|--------|------|
| 无缓存 | `none` | 禁用提示缓存 |
| 5 分钟 | `short` | API 密钥认证的默认配置 |
| 1 小时 | `long` | 扩展缓存 |

**默认行为**: 使用 Anthropic API 密钥认证时，OpenClaw 自动应用 `cacheRetention: "short"`（5分钟缓存）。

### 4. 100万上下文窗口 (1M Context)

Anthropic 的 100万上下文窗口处于 beta 阶段。启用方式：

```json
{
  "agents": {
    "defaults": {
      "models": {
        "anthropic/claude-opus-4-6": {
          "params": { "context1m": true }
        }
      }
    }
  }
}
```

### 5. 思维模式 (Claude 4.6)

Anthropic Claude 4.6 模型在 OpenClaw 中默认使用 `adaptive`（自适应）思维模式。可通过以下方式覆盖：

- 每条消息覆盖：`/think:<level>`
- 模型参数覆盖：`agents.defaults.models["anthropic/<model>"].params.thinking`

---

## 配置示例

### API 密钥认证

```json
{
  "env": { "ANTHROPIC_API_KEY": "sk-ant-..." },
  "agents": { "defaults": { "model": { "primary": "anthropic/claude-opus-4-6" } } }
}
```

### 快速设置命令

```bash
# 交互式设置
openclaw onboard

# 非交互式设置
openclaw onboard --anthropic-api-key "$ANTHROPIC_API_KEY"
```

---

## 故障排除

| 问题 | 解决方案 |
|------|----------|
| 401 错误 / token 突然失效 | Anthropic token 认证可能已过期或被撤销，建议迁移到 API 密钥 |
| 未找到 API 密钥 | 认证是**按 agent 分配**的，新 agent 不会继承主 agent 的密钥，需重新运行 onboarding |
| 无可用认证配置 | 运行 `openclaw models status --json` 查看 `auth.unusableProfiles` |

---

## 相关链接

- [Claude Code CLI 参考](https://code.claude.com/docs/en/cli-reference)
- [Claude Agent SDK 概览](https://platform.claude.com/docs/en/agent-sdk/overview)
- [自适应思维](https://platform.claude.com/docs/en/build-with-claude/adaptive-thinking)
- [扩展思维](https://platform.claude.com/docs/en/build-with-claude/extended-thinking)

---

*本报告由 OpenClaw 文档自动生成*