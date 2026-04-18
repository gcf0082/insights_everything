# PanicLock 项目洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **仓库链接** | https://github.com/paniclock/paniclock |
| **项目名称** | PanicLock |
| **项目描述** | Instantly disable Touch ID and lock your Mac with one click or keyboard shortcut. |
| **所有者** | paniclock |
| **许可证** | MIT License |
| **主要语言** | Swift (72.6%), Shell (27.4%) |
| **主题标签** | macos, apple, locking, security-tools, privacy-tools |
| **创建时间** | 暂无公开信息 |
| **最新版本** | v1.0.9 (2026年4月17日) |
| **仓库星标数** | 148 |
| **关注者数** | 2 |
| **分支数** | 1 (main) |
| **提交数** | 54 |
| **发布版本数** | 8 |
| **贡献者数** | 3 |

## 核心功能

PanicLock 是一款 macOS 菜单栏实用工具，用于在需要时立即禁用 Touch ID 并锁定屏幕。该工具填补了 macOS 的功能空白：系统没有内置方式在关键时刻立即禁用 Touch ID。

### 主要特性

- **一键紧急锁定** - 点击菜单栏图标或按热键即可立即锁定
- **合盖锁定** - 合上笔记本盖时自动锁定并禁用 Touch ID
- **临时禁用 Touch ID** - 强制使用密码解锁
- **自动恢复** - 解锁后恢复原有的 Touch ID 设置
- **键盘快捷键** - 可配置全局热键（如 ⌃⌥⌘L）
- **登录时启动** - 登录时自动启动

## 技术架构

### 项目结构

```
paniclock/
├── .github/                    # GitHub 配置文件
├── PanicLock/                  # 主应用
├── PanicLockHelper/            # 特权辅助工具
├── Shared/                     # 共享代码
├── assets/                     # 资源文件
├── scripts/                    # 脚本文件
├── PanicLock.xcodeproj/        # Xcode 项目文件
├── LICENSE                     # MIT 许可证
├── README.md                   # 项目说明
├── SECURITY.md                 # 安全政策
└── ExportOptions.plist         # 导出配置
```

### 技术实现

PanicLock 使用特权辅助工具（通过 SMJobBless 安装）来修改 Touch ID 超时设置：

1. 通过 `bioutil -r -s` 读取当前超时设置
2. 通过 `bioutil -w -s -o 1` 将超时设置为 1 秒
3. 通过 `pmset displaysleepnow` 锁定屏幕
4. 约 2 秒后恢复原始超时设置

## 安全特性

- **最小权限原则** - 辅助工具仅运行 3 个硬编码命令（bioutil、pmset）
- **无命令注入** - 超时参数为 Swift Int 类型，非字符串
- **代码签名的 XPC** - 辅助工具验证连接应用的 bundle ID、team ID 和证书
- **无网络活动** - 应用 100% 离线，无遥测或分析
- **无数据收集** - 仅存储偏好设置（图标风格、键盘快捷键）
- **开源** - 完整代码可供审计

## 安装方式

### Homebrew 安装

```bash
brew install paniclock/tap/paniclock
```

### 手动下载

从 releases 页面下载最新的 DMG 文件：https://github.com/paniclock/paniclock/releases/latest

## 系统要求

- macOS 14.0 (Sonoma) 或更高版本
- 配备 Touch ID 的 Mac

## 使用说明

| 操作 | 效果 |
|------|------|
| 左键点击图标 | 立即触发紧急锁定 |
| 右键点击图标 | 打开菜单（偏好设置、卸载、退出）|

### 合盖锁定

在偏好设置中启用后，合上 Mac 的盖子将自动禁用 Touch ID 并锁定屏幕。Touch ID 将保持禁用状态，直到您使用密码重新登录。如果屏幕因其他原因锁定（屏幕保护程序、显示器睡眠等），Touch ID 将正常工作。

## 发布流程

发布脚本处理构建、签名、公��和打包：

```bash
./scripts/release.sh
```

**功能特点：**

- 自动从 Xcode 项目中提取版本号
- 使用 Developer ID 进行签名（App Store 之外分发）
- 提交给 Apple 进行公证（可能需要几分钟到几小时）
- 创建已公证的 DMG 用于分发
- 支持并行公证 - 每个版本都有自己的构建目录

## 贡献者

| 贡献者 | 头像 | 描述 |
|--------|------|------|
| @seanieb |  | Seanie Byrne - 项目维护者 |
| @baka-yuki |  | Yuki - 贡献者 |
| github-advanced-security[bot] |  | GitHub 高级安全机器人 |

## 结论

PanicLock 是一个专注于隐私和安全的 macOS 实用工具，针对需要在特定情况下快速禁用生物识别认证的场景。该工具设计简洁、功能明确，提供了额外的安全保护层，特别适用于需要保护个人设备免受未经授权访问的用户。项目目前在 GitHub 上拥有 148 颗星标和 2 个分支，表明其在社区中获得了一定的关注度。

---
*报告生成时间：2026-04-18 10:24:36*