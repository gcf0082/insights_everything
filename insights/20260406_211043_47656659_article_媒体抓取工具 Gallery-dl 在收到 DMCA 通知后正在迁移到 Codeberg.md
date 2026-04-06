# GitHub DMCA Takedown 洞察报告

## 基本信息

- **洞察链接**: https://github.com/mikf/gallery-dl/discussions/9304
- **项目名称**: gallery-dl
- **讨论主题**: DMCA Takedown Notice by FAKKU, LLC
- **讨论时间**: 2026年3月23日 - 2026年4月6日
- **评论数量**: 22 comments · 46 replies
- **报告生成时间**: 2026-04-06 21:10:43

## 事件概述

2026年3月23日，gallery-dl项目维护者mikf收到来自FAKKU, LLC的DMCA投诉通知，涉及gallery-dl以及另外28个仓库。投诉声称该工具侵犯了其版权，并要求使用git-filter-repo重写整个仓库历史以删除相关文件。

## 投诉内容

### 被指侵权的文件

- `gallery_dl/extractor/nhentai.py` - NHentai提取器
- `gallery_dl/extractor/exhentai.py` - E-Hentai/ExHentai提取器
- `gallery_dl/extractor/hitomi.py` - Hitomi提取器
- `gallery_dl/extractor/hentaifoundry.py` - Hentai Foundry提取器
- 支持数十个hentai/doujinshi网站

### 投诉理由

FAKKU声称gallery-dl是"允许从hentai盗版基础设施进行自动批量下载的工具"，构成对DMCA第1201条反规避条款的违反。

## 社区反应

### 主流观点

1. **质疑投诉合理性**: 大多数社区成员认为FAKKU的投诉是恶意行为。许多用户指出，FAKKU本身就已经从其投诉的网站删除了内容，这些网站上并没有FAKKU的内容。

2. **支持迁移到其他平台**: 许多用户建议迁移到Codeberg或其他平台，避免GitHub的DMCA政策限制。

3. **法律建议**:
   - 联系EFF (Electronic Frontier Foundation) 寻求法律帮助
   - 考虑发送DMCA反通知（但存在法律风险）
   - 建议购买版权保险

### 关键讨论点

- **Hentai Foundry争议**: 社区指出Hentai Foundry是一个用户发布自己艺术作品的网站，完全不是盗版网站，FAKKU的投诉显然缺乏研究。
- **DMCA第1201条反规避条款**: 投诉声称工具"规避"了访问控制，但gallery-dl实际上并不连接到FAKKU的服务器，也不绕过任何DRM或访问控制。
- **GitHub的立场**: GitHub在审查后表示"没有找到足够的证据来确定有效的反规避索赔"，但认为该通知包含"其他有效的版权索赔"。

## 项目维护者的应对

### 1. 初始反应

mikf表示"非常犹豫"，更愿意"切换到不同的平台而不是做出任何重大改变"。

### 2. 备份与迁移尝试

- 尝试将仓库迁移到Codeberg和GitLab
- 创建了完整的GitHub迁移存档（不含发布版本约250MB，含发布版本6GB）
- Codeberg迁移失败，最终成功迁移到GitLab

### 3. 最终决定

2026年3月27日，mikf决定妥协：

> "我唯一想做的就是写代码，像这些年一样管理这个项目，而不必担心任何版权、政治等外部问题。最简单的方法就是屈服，所以我打算这样做。"

采取的行动：
- 使用`git-filter-repo`重写历史
- 删除DMCA通知中列出的所有文件，以及任何可能包含FAKKU内容的网站提取器
- 发送邮件通知GitHub Support

### 4. 结果

2026年3月30日，Jacob（FAKKU方面）确认gallery-dl已经没事：

> "我认为这足以满足GitHub的要求，我会告诉我们的DMCA代理你没事了。"

## 受影响的其他项目

根据讨论，以下项目也受到此次DMCA影响：
- HDoujinDownloader
- Hitomi-Downloader
- NHentaiDownloader

其中Hitomi-Downloader已被GitHub完全禁用。

## 后续影响

1. **外部提取器**: gallery-dl支持从外部模块加载提取器（使用`-X, --extractors PATH`参数），社区成员已在Codeberg创建了额外的提取器仓库来维护被删除的功能。

2. **存档**: 完整的GitHub迁移存档已保存，包含issues、PRs、discussions、tags等所有内容。

## 结论

此事件展示了开源项目在面对恶意DMCA投诉时的脆弱性。尽管最终gallery-dl通过妥协保留了项目，但这一案例引发了关于DMCA滥用和开源平台责任的广泛讨论。社区普遍认为FAKKU的投诉缺乏依据，是一种"钓鱼执法"行为，但GitHub的DMCA处理流程还是迫使项目维护者做出了妥协。

## 参考链接

- 原始DMCA通知: https://github.com/github/dmca/blob/master/2026/03/2026-03-23-fakku.md
- Codeberg镜像: https://codeberg.org/mikf/gallery-dl
- GitLab镜像: https://gitlab.com/mikf/gallery-dl
- 外部提取器: https://codeberg.org/parsley-creature/additional-gallery-dl-extractors
