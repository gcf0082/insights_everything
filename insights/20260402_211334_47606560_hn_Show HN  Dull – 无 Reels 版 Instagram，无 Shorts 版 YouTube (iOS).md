# 洞察报告：Dull – Instagram Without Reels, YouTube Without Shorts

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://news.ycombinator.com/item?id=47606560 |
| **标题** | Show HN: Dull – Instagram Without Reels, YouTube Without Shorts (iOS) |
| **作者** | kasparnoor |
| **发布时间** | 约16小时前 |
| **得分** | 115 points |
| **评论数** | 99条 |
| **网站** | https://getdull.app |

## 项目概述

Dull 是一款 iOS 应用，旨在帮助用户在使用 Instagram、YouTube、Facebook 和 X 时过滤掉短视频内容（如 Reels 和 Shorts）。

### 核心功能

1. **短视频过滤**：通过 CSS 和 JS 注入过滤短格式内容
2. **MutationObserver**：处理懒加载的动态内容
3. **灰度模式**：将应用界面转为黑白以减少使用欲望
4. **时间限制**：帮助用户控制使用时长
5. **使用追踪**：记录应用使用数据

### 技术挑战

- 平台经常更改 DOM 结构
- Instagram 混淆类名
- YouTube 不断调整 Shorts 在信息流中的显示方式
- 这是一场持续的"猫鼠游戏"

## 社区讨论焦点

### 1. 长期可行性担忧

开发者承认无法确定 Instagram 或 YouTube 是否会采取行动阻止该应用，但指出平台目前正因让自家应用上瘾而面临诉讼。

### 2. 法律与 TOS 风险

- **法律层面**：类似广告拦截，服务是否有权控制用户"不使用"其服务的方式仍有争议
- **TOS 违规**：使用此类工具可能违反平台服务条款，导致账户被封禁或 IP 被屏蔽
- **历史先例**：YouTube-dl 诉讼失败，Invidious 等服务仍然存在

### 3. 定价模式争议

- 年费订阅或终身订阅
- 用户认为订阅模式更诚实："直到停止工作才付费"
- 也有人批评"终身订阅"是一种赌博

### 4. 竞品对比

| 方案 | 平台 | 说明 |
|------|------|------|
| News Feed Eradicator | 浏览器扩展 | Firefox/Chrome 扩展 |
| ScrollGuard | Android/iOS | 使用 AccessibilityService |
| YouTube ReVanced | Android | 非官方 YouTube 应用 |
| Untrap | iOS | Safari 扩展 |
| Beeper | 跨平台 | 聚合消息应用 |

### 5. 替代建议

- 直接卸载原生应用
- 使用网页版并添加书签（如 Instagram following 页面）
- iOS 辅助访问模式（Assistive Access）
- Brave 浏览器内置过滤功能

## 总结

Dull 反映了用户对短视频成瘾问题的关注，以及对平台算法推荐的抵制意愿。社区讨论主要集中在产品的长期可行性、法律风险和替代方案上。一方面，有人认为这类工具是用户对抗"黑暗设计模式"的合理选择；另一方面，批评者指出这可能违反平台 TOS，且依赖持续维护来应对平台的技术反制。

---
*报告生成时间：2026-04-02*