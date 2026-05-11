# 洞察报告：Cloudflare是否敲诈了Canonical？

## 基本信息

- **洞察链接**: https://www.flyingpenguin.com/can-someone-please-explain-whether-cloudflare-blackmailed-canonical/
- **发布日期**: 2026年5月11日
- **作者**: Davi Ottenheimer
- **网站**: flyingpenguin
- **分析日期**: 2026年5月12日

---

## 事件概述

2026年4月30日，Canonical（Ubuntu的母公司）遭受了大规模DDoS攻击，导致其多个公共网站服务中断约20小时。这起事件引发了关于Cloudflare是否涉嫌敲诈的广泛讨论。

## 攻击时间线

| 时间（UTC） | 事件 |
|------------|------|
| 4月30日 16:33:37 | blog.ubuntu.com 首次标记为服务中断 |
| 4月30日 16:34起 | canonical.com、academy.canonical.com、developer.ubuntu.com等陆续下线 |
| 4月30日 16:41 | assets.ubuntu.com 下线 |
| 4月30日 16:43 | ubuntu.com 下线 |
| 4月30日 19:34 | security.ubuntu.com 下线（关键的安全更新端点） |
| 4月30日 19:40 | archive.ubuntu.com 下线（关键的软件仓库端点） |
| 4月30日 20:50 | archive.ubuntu.com 恢复运营 |
| 4月30日 20:51 | security.ubuntu.com 恢复运营 |
| 5月1日 07:13 | Canonical首次公开承认事件 |
| 5月1日 12:44 | 所有服务完全恢复 |

## 攻击者与工具

自称为"伊斯兰网络抵抗组织"（伊拉克）的攻击团体声称使用了付费DDoS服务Beamed进行攻击。Beamed是一种商业拒绝服务产品，通过多个TLD销售，beamed.su作为营销博客，beamed.st作为客户登录入口。

Beamed在其博客中明确宣传可以绕过Cloudflare保护，广告语包括：
> "Cloudflare acts as a reverse proxy, hiding the origin server's IP address. Many low-quality booters fail against 'Under Attack Mode' or Bot Fight Mode. Beamed.su employs several advanced techniques to effectively stress test websites protected by Cloudflare and similar CDNs."

讽刺的是，Beamed的网站本身也托管在Cloudflare上。

## 关键发现：Cloudflare的双重角色

### 攻击者和受害者的基础设施都依赖Cloudflare

- Beamed的域名（beamed.su和beamed.st）解析到Cloudflare AS13335地址
- Canonical的仓库端点（security.ubuntu.com和archive.ubuntu.com）在攻击后也迁移到了Cloudflare AS13335
- Immaterialism Limited（Beamed的注册商）也通过Cloudflare的nameservers代理

### 公司背景

**Immaterialism Limited**：
- 注册于英国公司注册处（公司编号15738452）
- 成立于2024年5月24日
- 最初董事：Nicole Priscila Fernandez Chaves（哥斯达黎加，1993年生）
- 2025年4月11日辞职，现任董事：Naomi Susan Colvin（英国，Courage Foundation前总监）

**AS39287的演变**：
- 2017-2020年：Privactually Ltd（塞浦路斯），运营Flattr
- 2020-2026年：ab stract Ltd（芬兰），负责人Peter Kolmisoppi（Pirate Bay创始人之一）
- 2026年2月27日：Materialism s.r.l.（罗马尼亚）接管

### 2026年2月27日的关键事件

同一天发生三个关键变化：
1. Materialism s.r.l.接管AS39287及IP段
2. Immaterialism Limited向英国公司注册处提交变更文件
3. Canonical为其仓库端点续签了Let's Encrypt证书

这一同步性引发了对预先协调的质疑。

## 敲诈指控的核心论点

### 作者认为这构成敲诈/勒索的模式

1. **无可见赎金支付**：公开渠道未见任何赎金转移
2. **无加密货币痕迹**：未发现相关规模的加密货币流动
3. **无公开威胁信**：未见任何勒索信曝光
4. **私下谈判可能性**：任何谈判都在私下进行

**然而，真正发生的是**：
- Canonical购买了Cloudflare的付费订阅服务
- 其两个最高价值的端点（repository endpoints）——拒绝服务会造成全球Ubuntu安全更新失败——转向了一个vendor，而该vendor的其他当前客户正是攻击他们的booter服务

### 历史类比

作者将此事与1930年代Moses Annenberg的"General News Bureau"进行类比：
- 当时向赌徒提供赛马结果
- 订阅者生存，不订阅者业务被摧毁
- 政府通过税务起诉打破垄断

结论：DDoS保护市场与booter市场的关系与当年如出一辙。Cloudflare既是威胁的来源，也是保护的服务商。

## Selective Cloudflare Onboarding

Canonical仅将两个A记录移交Cloudflare：
- security.ubuntu.com
- archive.ubuntu.com

这两个恰恰是攻击者重点针对的仓库端点。其他受影响的站点（ubuntu.com、canonical.com等）留在Canonical自有机房，最终通过上游过滤和攻击消退恢复。

## 结论

作者认为，虽然Cloudflare没有直接发出威胁，但：
- Beamed持续可租用即为威胁
- 服务中断时钟即为最后期限
- Cloudflare在双方同时获利：向攻击者提供免费基础设施，向受害者出售保护服务

整个事件中，每一步都在公开记录中：英国公司注册处、RIPE数据库、证书透明度日志、Canonical自身状态页面。作者认为这是"洗衣"公共记录的典型案例。

---

*本报告基于公开信息整理，仅代表原文观点，不代表本平台立场。*