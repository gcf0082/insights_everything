# 洞察报告：Project Glasswing

## 基本信息

- **洞察链接**：https://www.anthropic.com/glasswing
- **发布日期**：2026年4月
- **来源**：Anthropic
- **主题**：AI时代的关键软件安全

---

## 核心摘要

Project Glasswing是由Anthropic联合多家科技巨头（包括Amazon Web Services、Apple、Broadcom、Cisco、CrowdStrike、Google、JPMorganChase、Linux Foundation、Microsoft、NVIDIA和Palo Alto Networks）发起的网络安全倡议。该项目旨在利用AI技术保护关键软件基础设施，应对AI时代日益严峻的网络安全威胁。

---

## 关键发现

### 1. AI网络能力的重大突破

- **Claude Mythos Preview**：一个通用的高级前沿模型，在网络安全领域展现出超越大多数人类的漏洞发现和利用能力
- 该模型已发现数千个高危漏洞，涵盖所有主流操作系统和网页浏览器
- 在CyberGym基准测试中，Mythos Preview得分83.1%，远超Opus 4.6的66.6%

### 2. 发现的典型漏洞案例

| 漏洞 | 描述 |
|------|------|
| OpenBSD漏洞 | 发现一个存在27年的漏洞，可通过远程连接使任意机器崩溃 |
| FFmpeg漏洞 | 发现一个存在16年的漏洞，自动化测试工具曾500万次触发该行代码都未能发现问题 |
| Linux内核漏洞 | 发现多个漏洞链，可将普通用户权限提升至完全控制权 |

### 3. 模型性能表现

| 基准测试 | Mythos Preview | Opus 4.6 |
|----------|----------------|----------|
| SWE-bench Verified | 93.9% | 80.8% |
| SWE-bench Pro | 77.8% | 53.4% |
| SWE-bench Multilingual | 87.3% | 77.8% |
| Terminal-Bench 2.0 | 82.0% | 65.4% |
| GPQA Diamond | 94.6% | 91.3% |

---

## 项目承诺与投入

### 资金投入

- **模型使用积分**：最高1亿美元，用于Mythos Preview的访问
- **开源安全捐赠**：
  - 250万美元捐赠给Alpha-Omega和OpenSSF
  -150万美元捐赠给Apache Software Foundation

### 合作伙伴权益

- 12家创始合作伙伴可使用Mythos Preview进行防御性安全工作
- 40多家额外组织可获取模型访问权限，用于扫描和保障第一方及开源系统
- 90天内Anthropic将公开分享学习成果

---

## 行业合作伙伴观点

**Cisco** - Anthony Grieco（SVP & Chief Security & Trust Officer）：
> "AI能力已经跨越了一个根本改变保护关键基础设施所需紧迫性的阈值。"

**AWS** - Amy Herzog（VP and CISO）：
> "我们每天分析超过400万亿个网络流量以检测威胁，AI是我们大规模防御能力的核心。"

**Microsoft** - Igor Tsyganskiy（EVP of Cybersecurity and Microsoft Research）：
> "当AI不再受限于纯人类容量时，负责任地使用AI来大规模改善安全和降低风险的机会是前所未有的。"

**CrowdStrike** - Elia Zaitsev（CTO）：
> "漏洞发现和被对手利用之间的时间窗口已经压缩——过去需要数月的事情现在只需几分钟。"

**Linux Foundation** - Jim Zemlin（CEO）：
> "AI增强的安全可以成为每个维护者的可信助手，而不仅仅是那些负担得起昂贵安全团队的维护者。"

---

## 未来计划

1. **漏洞披露流程优化**
2. **软件更新流程改进**
3. **开源和供应链安全加强**
4. **安全开发生命周期实践**
5. **受监管行业标准制定**
6. **分诊扩展和自动化**
7. **补丁自动化**

---

## 结论

Project Glasswing代表了一个重要的行业协作尝试，旨在为AI驱动的网络安全时代提供持久的防御优势。该项目认识到没有单个组织可以单独解决这些网络安全问题，需要前沿AI开发者、软件公司、安全研究人员、开源维护者和各国政府共同参与。

---

*报告生成日期：2026-04-08*
