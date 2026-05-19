# 洞察报告：CISA 管理员在 GitHub 上泄露 AWS GovCloud 密钥

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://news.ycombinator.com/item?id=48190454 |
| **原文链接** | https://krebsonsecurity.com/2026/05/cisa-admin-leaked-aws-govcloud-keys-on-github/ |
| **发布平台** | Hacker News |
| **得分** | 260 points |
| **评论数** | 114 comments |
| **原始发布时间** | 2026年5月18日 |
| **报告生成时间** | 2026年5月20日 |

---

## 事件概述

2026年5月15日，安全研究员Guillaume Valadon发现CISA（美国网络安全和基础设施安全局）的一名承包商在GitHub上维护了一个名为"Private-CISA"的公开代码仓库，其中暴露了多个高权限AWS GovCloud账户的凭证，以及大量内部CISA系统的敏感信息。

该仓库包含了：
- 三个Amazon AWS GovCloud服务器的管理凭证（文件："importantAWStokens"）
- 数十个内部CISA系统的明文用户名和密码（文件："AWS-Workspace-Firefox-Passwords.csv"）
- CISA内部软件构建、测试和部署的详细信息
- 云密钥、令牌、日志和其他敏感资产

---

## 关键发现

### 泄露者信息
- 承包商：Nightwing（位于弗吉尼亚州杜勒斯）
- GitHub账户创建时间：2018年9月
- 仓库创建时间：2025年11月13日
- 该管理员故意禁用了GitHub默认的密钥检测功能

### 安全问题严重性
安全专家Philippe Caturegli（Seralys创始人）验证了以下内容：
- 暴露的凭证可以访问三个高权限AWS GovCloud账户
- 暴露了CISA内部"artifactory"（软件包仓库）的明文凭证
- 这是近年来最严重的政府数据泄露事件之一

### 后续处理
- 仓库在通知CISA后不久被下线
- 但暴露的AWS密钥在48小时后仍然有效
- CISA表示"目前没有迹象表明敏感数据已被泄露"

---

## CISA现状

CISA目前正经历严重的预算和人员削减：
- 自特朗普第二任期开始以来，已失去近三分之一的员工
- 强制提前退休、买断和辞职遍及各个部门

---

## Hacker News 讨论摘要

### 主要观点分类

**1. 责任归属争议**
- 承包商个人失误 vs. 机构监管缺失
- 有人认为这是"严重过失"（gross negligence）
- 有人质疑为何DOGE裁员后缺乏基本的安全监督机制

**2. 失误 vs. 蓄意**
- 部分用户认为这是个人失误，仓库可能用于工作电脑和家庭电脑间的同步
- 另一些用户指出故意禁用GitHub密钥检测功能这一行为令人怀疑
- 有人提出这可能是外国特工渗透或被胁迫的结果

**3. 对CISA现状的批评**
- 许多人将此事与DOGE大规模裁员联系起来
- "开除了所有胜任的人员"导致的后果
- 安全团队被裁撤（如red team员工）

**4. 技术层面讨论**
- 使用明文密码CSV文件的落后做法
- 密码管理器可轻松解决此问题
- LLM可能读取并记住这些泄露的密钥

**5. 深层问题**
- 共享凭证管理缺乏好的解决方案
- 大型企业使用Excel文件存储密码的现象很普遍
- 这是"理论与实践的差距"

---

## 影响与启示

1. **对CISA的信任危机**：作为美国网络安全核心机构，CISA自身的安全 practices 如此薄弱令人担忧
2. **政府承包商管理漏洞**：缺乏对承包商账户的适当监督和审计
3. **密钥管理的重要性**：长期静态凭证应被短期动态凭证取代
4. **自动化防护的缺失**：GitGuardian在外部检测到此问题，说明内部缺乏自动化的 secrets 检测机制

---

## 参考来源

- Krebs on Security: "CISA Admin Leaked AWS GovCloud Keys on Github"
- Security consultancy Seralys
- Security firm GitGuardian