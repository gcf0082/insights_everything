# 隐私审计洞察报告

## 基本信息

- **洞察链接**: https://www.404media.co/google-microsoft-meta-all-tracking-you-even-when-you-opt-out-according-to-an-independent-audit/
- **发布日期**: 2026年4月14日
- **来源**: 404 Media
- **作者**: Matthew Gault
- **审计机构**: webXray (隐私搜索引擎)
- **审计对象**: Google、Microsoft、Meta
- **审计地区**: 加利福尼亚州
- **审计时间**: 2026年3月
- **审计网站数量**: 7,000+ 个热门网站

## 审计结果摘要

独立隐私审计机构 webXray 对加利福尼亚州超过7,000个热门网站进行了网络流量审计，发现 Google、Microsoft 和 Meta 可能违反州法规并面临数十亿美元罚款。审计显示，即使用户选择退出跟踪，仍有55%的网站设置了广告Cookie。

## 主要发现

### 1. 各公司_opt-out_失败率

| 公司 | Opt-out失败率 |
|------|---------------|
| Google | 87% |
| Meta | 69% |
| Microsoft | 50% |

### 2. Google 的违规行为

审计发现，当浏览器使用 GPC（Global Privacy Control）连接到 Google 服务器时，会发送 "sec-gpc: 1" 代码表示用户选择退出。然而，Google 服务器响应时会使用 "set-cookie" 命令创建一个名为 IDE 的广告Cookie，完全忽视用户的 opt-out 信号。

### 3. Meta 的违规行为

Meta 指示出版商在其网站上安装跟踪代码，该代码不包含任何对全球标准 opt-out 信号的检查——它无条件加载、触发跟踪事件并设置Cookie，完全无视消费者的隐私偏好。审计显示 Meta 的跟踪数据中根本没有 GPC 检查。

### 4. Microsoft 的违规行为

Microsoft 同样未能正确处理 GPC opt-out 信号，失败率达到50%。

### 5. 同意管理平台（CMP）问题

审计还发现 Google 认证的同意管理平台（CMP）存在严重问题：

- 测试的三家CMP公司 opt-out 失败率分别为：77%、91%、90%
- 所有 Google 认证的 CMP 都无法100%工作
- Google 运营的 CMP Partner Program 认证这些 CMP，存在利益冲突

## 技术细节

webXray 提供了简单的解决方案：当 Microsoft 的广告服务器收到带有 Sec-GPC: 1 的流量时，只需返回 "451 Unavailable For Legal Reasons" 状态代码，即可表示因消费者法律定义的 opt-out 而无法提供内容，且不会设置任何Cookie。

## 公司回应

- **Google**: "这份报告基于对我们产品工作方式的根本误解。我们按法律要求尊重广告商和出版商提供的 opt-out。"
- **Meta**: "这是营销手段，误解了 GPC 的工作方式和 Meta 的角色。GPC 仅限制某些第三方数据的使用。"
- **Microsoft**: "当我们收到 GPC 信号时，我们选择用户退出与第三方共享个人数据用于个性化广告。某些 Microsoft Cookie 对于运营目的是必要的，因此即使检测到 GPC 信号也可能被设置和读取。"

## 背景信息

webXray 创始人 Timothy Libert 曾是 Google Cookie 政策与合规性的负责人。他表示在 Google 的工作是保护用户，但公司高层不同意这一理念。他于2023年离开 Google 并创立了 webXray。

此前，Google、Meta 和 Microsoft 已因类似隐私违规行为合计支付了数十亿美元。Libert 表示："在很多方面，罚款已经取代了税收。我想展示的是执法如何失败。"

## 结论

审计揭示了科技巨头在用户隐私保护方面的系统性失败。即使用户明确选择退出，这些公司仍然通过各种方式继续跟踪用户。Libert 将此比喻为"数据经济的霍尔木兹海峡"——如果要做出改变，必须从这里切断，任何其他措施都只是政治作秀。

## 相关链接

- [webXray 官网](https://webxray.ai/)
- [webXray 加州隐私审计](https://globalprivacyaudit.org/2026/california)
- [加州消费者隐私法案 (CCPA)](https://oag.ca.gov/privacy/ccpa)