# 洞察报告

**洞察链接**: https://blog.cloudflare.com/agents-stripe-projects/

**基本信息**:
- **标题**: Agents can now create Cloudflare accounts, buy domains, and deploy
- **发布日期**: 2026-04-30
- **作者**: Sid Chatterjee, Brendan Irvine-Broque
- **来源**: Cloudflare Blog

---

## 摘要

Cloudflare 宣布代理（Agents）现在可以代表用户创建 Cloudflare 账户、购买域名并部署代码。通过与 Stripe Projects 合作推出的新协议，代理可以完成从零到生产部署的全流程，无需人工介入。任何人脸识别、复制粘贴 API 令牌或输入信用卡信息。

## 核心内容

### 1. 代理的能力提升

代理现在可以：
- 创建 Cloudflare 账户
- 启动付费订阅
- 注册域名
- 获取 API 令牌并部署代码

### 2. 工作流程

```
用户登录 Stripe → 代理发现可用服务 → 自动创建/关联账户 →
获取 API 令牌 → 部署应用到生产环境
```

关键点：
- 如果用户已有 Cloudflare 账户，通过 OAuth 流程授权
- 如果是新用户，自动创建账户
- 用户仅需在必要时批准（如添加支付方式）

### 3. 协议三大组件

| 组件 | 功能 |
|------|------|
| **发现（Discovery）** | 代理可查询可用服务目录 |
| **授权（Authorization）** | 平台验证用户身份，Cloudflare 自动创建账户或关联现有账户 |
| **支付（Payment）** | Stripe 提供支付令牌，代理可在限额内消费 |

### 4. 安全措施

- **无信用卡信息泄露**：代理不直接接触信用卡号码
- **默认消费限额**：每月 100 美元上限
- **预算提醒**：用户可设置 Cloudflare 预算警报

## 合作伙伴与激励

- **Stripe Projects**：开放 beta 测试
- **Stripe Atlas**：新创企业可获得 10 万美元 Cloudflare 积分
- **任何平台**：均可集成此协议，实现无摩擦用户体验

## 意义

此协议将 OAuth 理念扩展到支付和账户创建领域，使代理成为一等公民。平台只需一次 API 调用即可为用户创建 Cloudflare 账户并获取认证令牌，实现从构建到部署的完整自动化。

---

*报告生成时间: 2026-05-06*