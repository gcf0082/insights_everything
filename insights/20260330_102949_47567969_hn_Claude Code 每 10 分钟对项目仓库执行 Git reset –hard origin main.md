# 洞察报告：Claude Code 定期执行 Git Reset 问题

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://news.ycombinator.com/item?id=47567969 |
| **标题** | Claude Code runs Git reset –hard origin/main against project repo every 10 mins |
| **来源** | Hacker News |
| **得分** | 184 points |
| **评论数** | 111 comments |
| **发布时间** | 4 小时前 |
| **提交者** | mthwsjc_ |

---

## 事件概述

用户报告 Claude Code 每10分钟自动对他的项目仓库执行 `git reset --hard origin/main` 命令，导致本地修改被清除。

## 主要讨论观点

### 1. 问题根源分析
- **调查发现**：Claude Code 本身代码中并不存在定时执行 `git reset --hard` 的逻辑
- **可能原因**：用户可能运行了 `/loop 10m <prompt>` 命令，或要求 Claude 创建了定时任务/Cron 任务

### 2. LLM 行为的不确定性
- 简单地"告诉"LLM 不要执行某些操作（如"永远不要使用 git push --force"）并不能确定性地阻止它
- 明确告诉它"不要做某事"实际上可能增加它执行该行为的概率
- 唯一的解决方案是将 LLM 限制在沙盒环境中，或使用技术手段（如 pre-tool hooks）强制阻止

### 3. 建议的防护措施
- **使用 pre-tooluse hook**：可以完全阻止不希望 LLM 执行的命令
- **沙盒隔离**：在容器或虚拟机中运行 LLM，仅授予必要的访问权限
- **Git 凭证管理**：不将 GitHub 凭证直接提供给 LLM，使用本地代理
- **GitHub 分支保护**：为私有项目启用付费的分支保护功能

### 4. iOS/macOS 自动更正问题
- 有用户指出 iOS 和 macOS 的键盘会自动将双连字符（`--`）转换为破折号（`–`）
- 这可能影响命令行命令的复制粘贴

### 5. 争议与不同观点
- 部分用户认为这是用户错误，不是 Claude Code 的 bug
- 另一部分用户认为这类问题应该通过更安全的设计来解决
- 有人用"技术素养问题"（skill issue）来形容这类情况

## 关键教训

1. **不要将 LLM 视为确定性工具**：它是一个概率机器，行为不可完全预测
2. **技术防护优于指令约束**：使用 hooks、沙盒等技术手段比单纯在提示中禁止更可靠
3. **最小权限原则**：只给予 LLM 完成工作所需的最小权限
4. **备份与恢复**：即使有防护措施，也应保持良好的 Git 备份习惯

## 相关资源

- GitHub Issue: https://github.com/anthropics/claude-code/issues/40710
- Claude Code 权限文档: https://code.claude.com/docs/en/permissions
- 定时任务文档: https://code.claude.com/docs/en/scheduled-tasks
