# 洞察报告：VS Code "Co-authored-by: Copilot" 提交信息更新

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://github.com/microsoft/vscode/issues/314311 |
| **仓库** | microsoft/vscode |
| **Issue 编号** | #314311 |
| **Issue 标题** | Update on "Co-authored-by: Copilot" in commit messages |
| **状态** | Open |
| **发布者** | dmitrivMS |
| **发布时间** | 2026年5月5日 |
| **里程碑** | On Deck |

## 问题背景

VS Code 在 1.110 版本中引入了 `git.addAICoAuthor` 设置，用于在提交信息中添加 Copilot 作为联合作者的标识（`Co-authored-by: Copilot copilot@github.com`）。该设置有三个选项：

- `off`：无论 Copilot 是否辅助代码生成，均不添加署名
- `chatAndAgent`：仅当提交包含使用聊天功能生成的代码时添加署名
- `all`：提交包含任何类型的 AI 生成代码（聊天、内联补全、NES）时添加署名

该设置最初默认值为 `off`。

## 问题发生

在 1.117 版本（2026年4月22日开始公开推送）中，VS Code 将该设置的默认值更改为 `all`。然而，代码中存在一个测试中未发现的 bug，导致非 Copilot 代码补全也被错误地归因于 Copilot，使得即使 `disableAIFeatures` 设置已开启，提交信息中仍包含 `Co-authored-by: Copilot copilot@github.com`。该问题记录在 Issue #313064 中。

由于该 bug，VS Code 在 1.118 版本（2026年4月29日开始公开推送）中进一步将设置值更改为 `chatAndAgent`。

## 当前处理进展

1. **回滚默认值**：VS Code 已将 AI 署名功能的默认值回滚至 `off`，并确保当 `disableAIFeatures` 设置为 true 时，无论 `git.addAICoAuthor` 的值如何，该功能都会被禁用。该修复通过 PR #313931 完成，将随 1.119 版本发布（2026年5月6日开始公开推送）。
2. **流程审查**：VS Code 正在审查其测试和发布流程中的漏洞，以防止类似问题再次发生。

## 功能后续计划

VS Code 团队将保留以下改进措施：

1. **智能署名判断**：仅对真正与 AI 相关的代码变更添加署名，不会对非 AI 相关的变更应用署名。
2. **用户同意机制**：在添加提交 trailer 之前，无论设置默认值如何，用户都必须给出同意。
3. **署名方式优化**：考虑采用 `Assisted-by` 署名方式替代当前的 `Co-authored-by`，详见 Issue #313962。
4. **模型信息扩展**：同样的机制可支持添加具体模型信息，详见 Issue #297353。

## 临时解决方案

如需立即禁用该功能，可在用户设置中显式关闭：

```json
"git.addAICoAuthor": "off"
```
