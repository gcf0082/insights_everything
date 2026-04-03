# 洞察报告：Claude Code 每10分钟执行 git reset --hard 导致数据丢失

## 基本信息

- **洞察链接**: https://github.com/anthropics/claude-code/issues/40710
- **Issue 编号**: #40710
- **报告日期**: 2026年3月30日
- **Issue 状态**: 已关闭（标记为"不计划修复"）
- **标签**: area:core, bug, data-loss, platform:macos

---

## 问题概述

Claude Code 每10分钟通过程序化git操作（无外部git二进制文件）对用户项目仓库执行 `git fetch origin` + `git reset --hard origin/main`。这会静默销毁所有已跟踪文件的未提交更改，仅未跟踪文件得以保留。Git worktrees不受影响。

## 环境信息

- **Claude Code版本**: 2.1.87 (Homebrew cask, compiled Bun binary)
- **操作系统**: macOS 15.4 (Darwin 25.3.0, arm64)
- **Shell**: zsh

## 证据详情

### 1. Git reflog 显示精确的10分钟间隔

reflog中记录了95+条记录，间隔精确为10分钟：
```
e8ea2c9 HEAD@{2026-03-29 22:19:09 +0200}: reset: moving to origin/main
e8ea2c9 HEAD@{2026-03-29 22:09:09 +0200}: reset: moving to origin/main
e8ea2c9 HEAD@{2026-03-29 21:59:09 +0200}: reset: moving to origin/main
...
```

### 2. 实时重现验证

1. 修改了 `src/lib/api.ts`（已跟踪文件）并创建了 `.canary-test.txt`（未跟踪文件）
2. 每15秒监控一次
3. 在下一个10分钟时刻，`api.ts` 被静默回滚 — 修改丢失
4. `.canary-test.txt`（未跟踪）幸存
5. 在4个连续周期中持续重现

### 3. fswatch 捕获的文件操作

在精确的重置时间，fswatch 捕获到：
```
23:59:10.349 .git/refs/remotes/origin/HEAD.lock  Created IsFile Removed AttributeModified
23:59:10.352 .git/logs/HEAD                       IsFile Updated
23:59:10.354 .git/refs/heads/main.lock            Created IsFile Removed AttributeModified
```

这是 `git fetch origin` + `git reset --hard origin/main` 的经典模式。

### 4. 唯一候选进程

`lsof` 确认 Claude Code CLI 进程（PID 70111, `claude --dangerously-skip-permissions`）是唯一在受影响仓库中有 CWD 的进程。

### 5. 未生成外部git二进制

在重置时间点以0.1秒间隔进行进程监控，未发现任何 `git` 进程。操作是通过程序化方式（libgit2或类似库）在Claude Code进程内执行的。

### 6. Worktrees免疫

Worktree的reflog显示**零条** `reset: moving to origin` 条目。重置仅针对主工作树。

## 排除的其他原因

经过彻底调查排除了所有外部原因：

| 原因 | 结论 | 详情 |
|------|------|------|
| Git hooks | 排除 | 均为`.sample`（未激活），无husky/lint-staged |
| Claude Code用户hooks | 排除 | 仅有peon-ping（音频），无git相关 |
| 插件市场更新器 | 排除 | 删除后重置继续 |
| macOS云同步 | 排除 | 无同步工具覆盖此目录 |
| Cron/LaunchAgents | 排除 | 无crontab，无相关LaunchAgent |
| Vite/SvelteKit dev server | 排除 | 所有文件写入输出目录 |
| IDE/编辑器 | 排除 | nvim在不同仓库 |
| Time Machine | 排除 | 本地快照为只读APFS |
| 文件监视器 | 排除 | 无fswatch/entr/watchman/guard运行 |

## 二进制分析

从编译后的二进制文件分析：
- `hg1()` 函数执行 `["fetch","origin"]` 通过 `t_(C8(), _)` **无明确CWD**，默认为 `process.cwd()`
- `io1()` 函数是git pull包装器，记录 `git pull: cwd=${H} ref=${_??"default"}`
- `fileHistory` 状态跟踪 `{snapshots: [], trackedFiles: new Set, snapshotSequence: 0}`

## 影响

在主工作树中，任何已跟踪文件的未提交更改每10分钟就会被静默销毁。在2小时的会话中，更改必须重新应用3次以上才能确定原因。当所有更改都已提交时，该bug不可见（重置为空操作），使其看起来像是间歇性问题。

## 变通方案

1. **使用git worktrees** — 确认免疫（worktree reflog中零重置条目）
2. **频繁提交** — 提交的更改在重置后仍保留

## 相关Issue

- [#8072](https://github.com/anthropics/claude-code/issues/8072) - Critical Bug: Code Revisions Being Repeatedly Reverted
- [#7232](https://github.com/anthropics/claude-code/issues/7232) - [MODEL] CRITICAL: Claude executed git reset --hard without authorization causing data destruction
- [#32793](https://github.com/anthropics/claude-code/issues/32793) - [BUG] `claude install` corrupts project git remote URL to anthropics/claude-plugins-official on macOS

---

*本报告基于 GitHub Issue #40710 的内容生成*
