# 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://news.ycombinator.com/item?id=47675013 |
| **标题** | Dropping Cloudflare for Bunny.net |
| **来源** | Hacker News |
| **作者** | shintoist |
| **发布时间** | 1 hour ago |
| **得分** | 152 points |
| **评论数** | 69 comments |
| **原文链接** | https://jola.dev/posts/dropping-cloudflare |

## 内容摘要

本文是一篇关于作者从Cloudflare迁移到Bunny.net的博客文章，分享了迁移的原因和体验。主要话题包括CDN服务选择、供应商依赖性问题、价格比较以及技术支持等。

## 热门评论摘要

### 1. 透明度和利益披露
- **GeneticGenesis**: 指出博客中的链接都带有Bunny的affiliate会员链接，呼吁作者透明披露
- **joladev** (作者): 承认过多使用affiliate链接，已标注并移除部分链接
- **gruez**: 指出这可能违反FTC指南，英国也有类似规定

### 2. Cloudflare vs Bunny.net 使用体验
- **Manchitsanan**: 赞赏Cloudflare Workers + Pages的开发者体验，但提到边缘缓存调试困难
- **ossa-ma**: 认为Cloudflare的wrangler CLI配合Claude Code非常好用
- **jtbaker**: 认为D1是主要短板，建议用Neon/Supabase等无服务器PostgreSQL

### 3. 价格和免费层
- **wahnfrieden**: Bunny没有免费层，但有预付费账单，避免意外的高额账单风险
- **kugelblitz**: 更喜欢付费服务而非免费服务，避免服务商突然收费
- **gordonhart**: 更愿意每月支付$2而非担心意外账单

### 4. 支持服务
- **ben8bit**: 认为Bunny的优势之一是支持服务，Cloudflare只有付费企业版才提供支持
- **FryHigh**: 分享因恶意举报被Cloudflare禁用首页的经历，申诉未获回应，已使用Bunny一年

### 5. 技术问题
- **tao_oat**: 尝试Bunny Edge Scripting体验不佳，经常失败且无错误日志
- **akoculu**: 近期迁移出Bunny，部分用户无法访问，客户服务响应太慢

### 6. DNS服务推荐
- **evolve2k**: 寻找替代Cloudflare的DNS服务
- **DNSimple**: 推荐DNSimple，$0.50/域名/月
- **desec.io**: 推荐desec.io，免费DNSSEC

### 7. 多CDN策略
- **moralestapia**: 希望有多CDN自动故障切换设置
- **bakugo**: 除了Cloudflare还有哪些CDN有免费层？

## 关键讨论点

1. **供应商锁定问题**: 从Cloudflare迁移到Bunny并未真正解决单一供应商依赖问题
2. **价格vs可预测性**: Bunny的预付费模式提供更好的成本可预测性
3. **欧盟公司偏好**: 部分用户倾向于支持欧洲公司
4. **IPv6支持**: Bunny默认不启用IPv6，需要额外配置

## 总结

这篇讨论反映了开发者对CDN/DNS服务选择的权衡考量。主要观点包括：Cloudflare的免费层难以匹敌但存在供应商锁定风险；Bunny提供更可预测的价格但缺少免费层；技术支持是选择服务商的重要因素；多供应商策略可能更为理想。
