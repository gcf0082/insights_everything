# 洞察报告：VS Code PR #310226

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://github.com/microsoft/vscode/pull/310226 |
| **PR 标题** | Enabling ai co author by default |
| **状态** | 已合并 (Merged) |
| **发起人** | cwebster-99 |
| **合并人** | dmitrivMS |
| **合并时间** | 2026年4月16日 |
| **目标版本** | 1.117.0 |
| **改动文件** | extensions/git/package.json, extensions/git/src/repository.ts |

## PR 概述

该 PR 将 Git 扩展的 `git.addAICoAuthor` 配置默认从 `"off"` 改为 `"all"`，使得 AI 生成的代码贡献在提交时默认自动添加 `Co-authored-by` trailer。

## 主要变更

1. **extensions/git/package.json**：将 `git.addAICoAuthor` 配置的默认值从 `"off"` 改为 `"all"`
2. **extensions/git/src/repository.ts**：更新运行时回退值以匹配新的架构默认

## 社区反应

该 PR 在合并后引发了极大的社区争议，用户普遍表示强烈不满：

### 负面反应统计

- 👎 反对：265+ 次
- 😕 困惑：27 次
- 👍 支持：1 次
- 😄 嘲笑：3+ 次

用户的主要不满点包括：

1. **未经同意自动添加署名**：即使未使用 Copilot，提交中也被自动添加了 `Co-authored-by: Copilot <copilot@github.com>`
2. **绕过用户设置**：`chat.disableAIFeatures: true` 等设置被忽略
3. **不尊重用户**：用户手写代码却被迫添加 AI 署名
4. **隐私和信任问题**：被视为" vandalism"（故意破坏）和"未经同意的代表行为"

### 典型用户评论

| 用户 | 观点 |
|------|------|
| rgs2151 | "Why in the world would you default this!" |
| anonymni-hlasatel | "Making this default behaviour and not notifying users is crazy" |
| flying-sheep | "Please revert until it actually works. I have chat.disableAIFeatures: true and co-authored-by copilot still gets inserted" |
| edenchazard | "Now I know why random commits of mine were suddenly being co-authored by copilot despite not using copilot" |
| marxoffice | "Let's inject our own name into every commit, even for users who never used Copilot, and ship it as a silent default." |
| ringoz | "I am not using copilot, I have 'chat.disableAIFeatures' and co-authored by copilot still gets inserted into commits" |
| whitequark | "Quick question: what the fuck is wrong with you?" |
| gabereiser | "I guess when no one uses your service, you have to pretend people are by adding it to commit messages without their knowledge" |
| snehesht | "Congrats, you've made it to HN front page and not in a good light" |

### 社区行动建议

用户建议迁移到其他编辑器：
- Zed
- Neovim
- Helix
- Kakoune
- Emacs

## 后续影响

1. 该 PR 登上了 Hacker News 首页，但并非以正面形象
2. 有用户创建了反向 PR：Default `git.addAICoAuthor` to `off` (#313725)
3. 第三方工具如 Claude 推出了 "NoPilots" 插件来禁用此功能

## 总结

该 PR 体现了微软在 AI 产品推广中过度激进的策略，试图通过静默默认的方式强制用户接受 AI 参与署名，而非通过透明的用户教育和明确 opt-in 机制。这种做法严重损害了用户信任，也引发了开源社区的强烈反弹。