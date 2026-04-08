# 洞察报告：从 Cloudflare 迁移到 bunny.net

## 基本信息

- **洞察链接**：https://jola.dev/posts/dropping-cloudflare
- **原文标题**：Dropping Cloudflare for bunny.net
- **作者**：Johanna Larsson
- **发布日期**：2026年4月2日
- **原文语言**：英语
- **标签**：elixir, phoenix, blog, bunnynet

---

## 核心洞察摘要

### 1. 迁移动机

作者是 Cloudflare 的长期用户，对其免费服务印象深刻，但也表达了以下担忧：

- **单点故障风险**：过度依赖单一公司可能被任意理由切断服务
- **中心化问题**：每次 Cloudflare 故障都会成为新闻
- **地域因素**：将互联网中心化到一家美国公司的感觉不太好
- **丑闻问题**：Cloudflare 过去曾有一些争议事件

### 2. 选择 bunny.net 的原因

- **公司位置**：总部位于斯洛文尼亚（欧盟），支持欧洲科技发展
- **性能优势**：CDN 服务可与 Cloudflare 竞争，PoP 网络虽较小但全球性能和速度表现出色
- **欧洲替代方案**：为互联网去中心化提供欧盟选择

### 3. 迁移内容

作者之前使用 Cloudflare 作为：
- 域名注册商（已迁移到 Porkbun）
- "Orange Cloud"：自动缓存、隐藏源站、可选保护功能

### 4. bunny.net 设置步骤

#### 4.1 创建 Pull Zone

1. 填写 pull zone 名称
2. 选择 origin type 为 Origin URL
3. 填写源站 URL（服务器公网 IP）
4. 如运行多个应用，需设置 Host header
5. 选择 Standard 套餐
6. 选择定价区域（部分区域较贵，可禁用）

#### 4.2 配置域名

1. 添加自定义主机名
2. 设置 CNAME 记录指向 bunny.net 提供的 CDN 域名（如 `website.b-cdn.net`）
3. 等待传播后点击"Verify & Activate SSL"

#### 4.3 配置缓存

两种方式：
- **方式一**：配置适当的 cache headers，bunny.net 默认尊重源站的 Cache-Control
- **方式二**：启用 Smart Cache，自动缓存图片、CSS、JS 等资源

作者采用了自定义缓存策略，在 Phoenix 框架中设置了：
```
cache-control: public, s-maxage=86400, max-age=0
```
这意味着 HTML 页面也被缓存，实现极快的访问速度。

#### 4.4 推荐配置

- **Force SSL**：强制所有请求使用 SSL
- **Origin Shield**：激活后减少源站负载最近的边缘位置优先
- **Stale Cache**：源站离线时继续提供缓存内容
- **Edge Rules**：设置默认域名重定向

### 5. 费用

- 新用户获得 $20 免费额度（14天内有效）
- 无需前置信用卡
- 如绑定信用卡额外获得 $30 额度
- 过期后按使用付费，大多数情况每月几分钱
- 最低支付要求：$1/月

---

## 结论

本文仅涵盖 bunny.net 的基础知识，作者认为还有更多功能待探索：
- Edge rules
- 缓存配置
- 安全和防火墙的 Shield 功能
- 视频托管和流媒体
- Edge scripting
- 边缘分布式容器
- S3 兼容存储（即将推出）

**推荐理由**：优秀的统计信息、日志和指标，可查看每个请求的详细信息，清晰反馈缓存状态。

---

*报告生成时间：2026年4月7日*
