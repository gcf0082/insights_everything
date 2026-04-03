# Cloudflare Radar 域名洞察报告：archive.today

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://radar.cloudflare.com/domains/domain/archive.today |
| **域名** | archive.today |
| **其他相关域名** | archive.is, archive.ph, archive.fo, archive.li, archive.vn, archive.md |
| **报告生成时间** | 2026-03-24 16:43:12 |

---

## 域名分类状态

根据 Cloudflare Radar 的数据，archive.today 目前被分类为以下类别：

- **C&C/Botnet** (命令和控制/僵尸网络)
- **DNS Tunneling** (DNS隧道)
- **CIPA Filter**
- **Reference** (参考)

---

## 事件背景

### DDoS 攻击事件

2026年1月11日，archive.today 开始利用其访问者的浏览器对安全研究人员 Jani Patokallio 的博客发起分布式拒绝服务(DDoS)攻击。攻击方式是在 archive.today 的 CAPTCHA 页面中嵌入 JavaScript 代码，每300毫秒执行一次，使毫无戒心的访问者成为攻击工具。

### 关键时间线

| 时间 | 事件 |
|------|------|
| 2025年11月 | FBI 对 archive.today 发出传票 |
| 2026年1月11日 | DDoS 攻击开始 |
| 2026年2月20日 | Wikipedia 将 archive.today 链接列入黑名单 |
| 2026年2月28日 | Wikipedia 开始移除 archive.today 链接 |

### 内容篡改

在攻击期间，Wikipedia 发现 archive.today 篡改了存档页面内容，插入了目标研究人员的信息。这违反了档案服务应该保持历史记录真实性的核心价值主张。

---

## 技术细节

### DNS 解析状态

Cloudflare 的不同 DNS 服务对 archive.today 的解析状态存在差异：

| DNS 服务 | 解析状态 |
|----------|----------|
| 1.1.1.1 (无过滤) | 正常解析 |
| 1.1.1.2 (恶意软件拦截) | 返回 0.0.0.0，状态为 "Censored" |
| 1.1.1.3 (家长控制) | 根据内容分级拦截 |

### 示例查询

通过 DoH 查询 1.1.1.2 的响应示例：

```json
{
  "Status": 0,
  "Question": [{"name": "archive.is", "type": 1}],
  "Answer": [...] // 返回恶意软件拦截标记
}
```

---

## 影响范围

### Wikipedia 影响

- 约 **695,000+** 个 Wikipedia 链接被破坏
- 影响范围包括学术论文、法律文件、新闻报道中依赖 archive.today 的稳定引用
- 造成级联引用失败

### 用户影响

- 使用 Cloudflare 1.1.1.2 DNS 的用户无法通过该服务访问 archive.today
- 使用 1.1.1.1 的用户仍可正常访问
- 芬兰用户报告访问 Patokallio 博客本身也存在困难

---

## 社区反应

Hacker News 社区对这一事件展开了激烈讨论（328 points, 237 comments）：

**支持 Cloudflare 的观点：**
- 保护用户免受武器化是合理的安全决策
- DNS 提供商有权保护用户免受恶意活动侵害

**批评 Cloudflare 的观点：**
- 单方面通过 DNS 级别拦截决定哪些内容可访问，模糊了基础设施与内容审核之间的界限
- DNS 级拦截开创了基础设施提供商成为内容审查者的先例

---

## 替代方案建议

对于需要网页归档服务的用户，可以考虑以下替代方案：

1. **Archive.org (Wayback Machine)**
   - 优点：最全面的选择，约8660亿页面存档，非营利治理（Internet Archive）
   - 缺点：快照速度较慢，基于爬虫

2. **Perma.cc**
   - 优点：专为学术和法律引用设计，稳定可靠
   - 缺点：免费版有限制

3. **ArchiveBox**
   - 优点：可自托管的开源解决方案
   - 缺点：需要自行维护

---

## 总结

archive.today 事件反映了互联网安全生态系统中一个重要问题：

1. **信任破裂**：一个本应保护历史记录完整性的服务，反而篡改了存档内容
2. **基础设施权力**：DNS 提供商越来越能够单方面决定哪些域名可访问
3. **用户武器化**：网站可以将访问者变成无意中的攻击参与者

Cloudflare 对 archive.today 的标记是一个有争议但从安全角度看似合理的决定，但同时也引发了关于互联网基础设施提供商权力边界的讨论。

---

*报告生成时间：2026年3月24日*
