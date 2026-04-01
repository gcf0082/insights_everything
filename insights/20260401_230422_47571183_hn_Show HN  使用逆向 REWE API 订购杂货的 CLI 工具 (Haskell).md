# 洞察报告

**链接**: https://news.ycombinator.com/item?id=47571183  
**标题**: Show HN: CLI to order groceries via reverse-engineered REWE API (Haskell)  
**发布时间**: 9小时前  
**得分**: 150 points  
**评论数**: 59 comments

---

## 项目概述

这是一个用Haskell编写的CLI工具，用于通过逆向工程的REWE（德国超市连锁）API下单购物。作者分享了他在学习REWE API过程中的收获，包括他们如何使用mTLS以及工作流程。

## 关键技术点

- **编程语言**: Haskell
- **API逆向工程**: 使用mitmproxy2swagger工具自动生成OpenAPI规范
- **mTLS认证**: REWE使用客户端证书进行指纹识别，而非传统安全目的
- **AI辅助开发**: 作者表示2026年是写Haskell的最佳时机，遇到构建系统或类型问题时可以求助AI

## 主要讨论话题

### 1. 智能购物代理工作流

评论者分享了他们的自动化购物流程：
- 将食谱URL或文本粘贴进去，AI提取食材、计算单位、替换为素食选项
- 检查常用物品和搜索缓存
- 询问用户家里已有什么
- 搜索REWE API并让用户选择
- 使用Claude填充购物车

### 2. API开放问题

- 有评论者指出REWE已经封锁API一段时间，担忧此类工具会让API更加难以访问
- 建议应用增加比价功能，比较不同门店的价格
- REWE员工表示这可能是促使管理层放宽API限制的契机

### 3. 形式验证

作者还尝试了形式验证，用Lean4证明建议引擎的正确性，并对Haskell实现和Lean实现进行随机测试对比。

### 4. 其他类似项目

- AsdaBot: 英国Asda的类似CLI工具
- heisse-preise.io: 德国超市价格比较网站

---

## 总结

这是一个有趣的个人项目，展示了如何通过逆向工程API来创建自动化购物工具。讨论的核心议题包括：零售API的开放性、智能代理购物的未来、以及在Haskell开发中使用AI辅助的经验。