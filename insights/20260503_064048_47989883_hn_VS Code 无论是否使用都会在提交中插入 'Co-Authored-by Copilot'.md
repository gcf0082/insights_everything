# 洞察报告

**链接**: https://news.ycombinator.com/item?id=47989883
**标题**: VS Code inserting 'Co-Authored-by Copilot' into commits regardless of usage
**来源**: Hacker News
**发布者**: indrora
**发布时间**: 2小时前
**得分**: 395
**评论数**: 186

## 内容摘要

这是一个关于VS Code在用户不知情的情况下，自动在Git提交中添加"Co-Authored-by Copilot"签名的讨论。原PR将配置项`git.addAICoAuthor`的默认值从"off"改为"all"，导致无论用户是否实际使用Copilot编写代码，提交都会被添加AI共同创作的标记。

### 核心问题

1. **欺骗性行为**: 用户只是在VS Code中编写代码或使用简单的自动补全功能，但提交时会被自动添加Copilot的共同创作签名前缀
2. **透明性缺失**: 这个签名不会在提交前向用户展示，用户完全看不到这个操作
3. **诚信问题**: 提交记录是法律和技术记录，伪造作者信息违背了开发者的信任

### 社区反应

讨论获得了大量关注（395分、186条评论），开发者们普遍表达了对微软的不满：

- 微软花了数十年重建的声誉因AI被付诸一炬
- 这与当年的"Sent from my iPhone"营销类似，但更恶劣——因为后者至少可以被用户看到和删除
- 被用作营销工具，而开发者成了免费的广告渠道
- 可能影响代码版权保护——AI生成的代码目前无法获得版权

### 后续发展

有用户指出，微软随后又将默认值从"all"改为"chatAndAgent"，但修改记录显示：off → on → chatAndAgent。

---