# GitHub Copilot 代码审查即将开始消耗 GitHub Actions 分钟数

## 基本信息

- **洞察链接**: https://github.blog/changelog/2026-04-27-github-copilot-code-review-will-start-consuming-github-actions-minutes-on-june-1-2026/
- **发布日期**: 2026年4月27日
- **标签**: actions, copilot
- **来源**: GitHub Changelog

## 变更摘要

从2026年6月1日起，GitHub Copilot 代码审查将开始消耗 GitHub Actions 分钟数。

## 详细内容

### 变更内容

GitHub Copilot 代码审查基于智能体架构（agentic tool-calling architecture），运行在 GitHub Actions 上的 GitHub 托管运行器上。

从2026年6月1日起，每次 Copilot 代码审查将按两种方式计费：

1. **AI Credits 计费**: 所有 Copilot 使用（包括代码审查）将根据新的按使用量计费模型计费为 AI Credits
2. **GitHub Actions 分钟数消耗**: 私有仓库每次运行的代码审查将消耗您现有计划配额的 GitHub Actions 分钟数，超出部分按标准 GitHub Actions 费率计费

**注意**: 公开仓库的 Actions 分钟数仍然免费。

### 适用计划

此变更适用于以下计划：

- GitHub Copilot Pro
- GitHub Copilot Pro+
- GitHub Copilot Business
- GitHub Copilot Enterprise

**这包括非授权用户进行的 Copilot 代码审查**，并通过组织直接计费。

### 生效时间

此变更于**2026年6月1日**生效。在此之前，Copilot 代码审查将继续仅从现有的 Copilot 高级请求单元（PRU）配额中扣除，不会消耗 GitHub Actions 分钟数。

### 需采取的行动

#### 1. 审查账单和使用情况

- 查看当前 GitHub Actions 使用情况：账单管理员可以在账户或组织账单设置中查看分钟数消耗和配额
- 检查支出预算限制：确认个人或组织的 Actions 预算与预期使用量相符，可随时调整
- 通过 GitHub Copilot 使用指标、GitHub Actions 指标和账单使用报告监控使用情况
- 查看按使用量计费的公告，了解 Copilot 本身如何计量
- 与账单管理员和工程负责人分享此更新

#### 2. 审查运行器设置

- 如果已启用 GitHub 托管运行器，则无需额外设置
- 如需自定义 GitHub 托管运行器环境，可升级到更大的运行器
- 自托管运行器设置同样可用

## 相关链接

- [Copilot 计费文档](https://docs.github.com/copilot/reference/copilot-billing/models-and-pricing#github-actions-minutes-for-code-review)
- [设置预算](https://docs.github.com/billing/how-tos/set-up-budgets)
- [Copilot 使用指标](https://docs.github.com/copilot/concepts/copilot-usage-metrics)
- [GitHub Actions 指标](https://docs.github.com/enterprise-cloud@latest/organizations/collaborating-with-groups-in-organizations/viewing-github-actions-metrics-for-your-organization)
- [GitHub 社区讨论](https://github.com/orgs/community/discussions/categories/announcements)