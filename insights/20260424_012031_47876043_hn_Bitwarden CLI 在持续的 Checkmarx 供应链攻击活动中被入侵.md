# 洞察报告：Bitwarden CLI 在持续供应链攻击中沦陷

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://news.ycombinator.com/item?id=47876043 |
| **标题** | Bitwarden CLI Compromised in Ongoing Checkmarx Supply Chain Campaign |
| **来源** | Socket.dev Blog |
| **发布时间** | 3小时前 |
| **得分** | 309 points |
| **评论数** | 149 comments |
| **提交者** | tosh |

## 事件概述

Bitwarden CLI（命令行工具）在持续的Checkmarx供应链攻击活动中被攻陷。恶意版本的 `@bitwarden/cli 2026.4.0` 在发布约19小时后被撤下，但在此期间已有334人下载并安装了受感染的版本。

## 攻击特征

- **发布平台**：npm（@bitwarden/cli）
- **恶意版本**：2026.4.0
- **潜伏时间**：约19小时
- **影响人数**：334人下载
- **类似攻击**：此前axios、ua-parser-js、node-ipc等也曾遭受类似供应链攻击

## 社区讨论焦点

### 1. 防护建议

设置软件包最小发布等待时间：

- **npm** (`~/.npmrc`): `min-release-age=7` (天)
- **pnpm**: `minimum-release-age=10080` (分钟)
- **bun** (`~/.bunfig.toml`): `minimumReleaseAge = 604800` (秒)

### 2. 替代方案讨论

- **rbw**：Rust实现的Bitwarden CLI替代品，依赖更少
- **KeePass**：本地密码库方案，避免云端同步风险
- **vaultwarden**：自托管Rust版本

### 3. 安全建议

- 使用Yubikey等硬件令牌保护重要账户
- 避免浏览器扩展程序，使用桌面/网页版密码库
- 不通过链接导航，直接输入URL

## 相关工具

- https://cooldowns.dev - 帮助配置软件包发布冷却时间
- https://depsguard.com - 自动配置推荐的安全设置

## 总结

这是一起典型的供应链攻击，攻击者通过入侵开发者账户向流行npm包注入恶意代码。社区建议通过设置软件包最小发布等待时间（cooldown）来降低风险，给安全研究人员时间检测恶意更新。尽管无法完全防范长期潜伏的攻击（如event-stream事件），但这种方法可以有效阻止快速爆发的供应链攻击。