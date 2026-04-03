# HopTab 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://www.royalbhati.com/hoptab |
| **HN讨论链接** | https://news.ycombinator.com/item?id=47474967 |
| **GitHub仓库** | https://github.com/royalbhati/HopTab |
| **生成时间** | 2026-03-24 |

---

## 产品概述

**HopTab** 是一款 macOS 工作区管理工具，旨在解决 macOS 原生应用切换器 `Cmd+Tab` 过于繁琐的问题。该工具允许用户固定（pin）自己关心的应用，然后通过 `Option+Tab` 在这些固定应用之间快速切换，而不必遍历所有打开的应用程序。

---

## 核心功能

### 1. 应用切换器 (App Switcher)
- **固定应用**：用户可以只固定自己关心的应用，Option+Tab 只在这些应用间循环
- **多窗口处理**：当应用有多个窗口时，HopTab 会显示选择器让用户选择具体窗口
- **快捷操作**：在切换器打开时可以使用 Cmd+Q 退出、Cmd+H 隐藏、Cmd+M 最小化

### 2. 窗口平铺 (Window Tiling)
- 全局键盘快捷键将任何窗口snap到屏幕的半屏、三分之一、四分之一或全屏
- 支持17种snap方向，全部可配置
- 快捷键示例：
  - 半屏：Ctrl+Option+方向键
  - 四分之一：Ctrl+Option+UIJK
  - 三分之一：Ctrl+Option+DFG

### 3. 布局模板 (Layout Templates)
- 内置5种布局：50/50分屏、IDE 60/40、三列、2x2网格、全屏
- 可以将应用分配到不同区域并应用布局

### 4. 配置文件 (Profiles)
- 为不同工作流程创建配置文件：如Coding、Design、Research
- 每个配置文件有自己的固定应用、布局
- 将配置文件分配给 macOS Spaces，切换桌面时 HopTab 自动切换配置文件
- 支持保存和恢复窗口会话

### 5. 窗口投掷 (Window Throwing)
- 使用 Ctrl+Option+Cmd+方向键将窗口投掷到另一个显示器
- 支持跨屏幕尺寸的比例放置

---

## 技术实现

- **CGEvent tap**：拦截并吞没全局快捷键
- **AXUIElement API**：强制置顶窗口（修复顽固应用）
- **NSPanel**：非激活覆盖层，层级为 .screenSaver
- **无App Sandbox**：需要 CGEvent 权限
- **语言**：Swift
- **架构**：通用（Apple Silicon + Intel）
- **系统要求**：macOS 14.0+

---

## 安装方式

### 命令行安装
```bash
curl -sL "$(curl -s https://api.github.com/repos/royalbhati/HopTab/releases/latest | grep -o '"browser_download_url": *"[^"]*"' | head -1 | cut -d '"' -f 4)" -o /tmp/HopTab.zip && unzip -o /tmp/HopTab.zip -d /Applications && xattr -c /Applications/HopTab.app
```

### 使用前准备
1. 授予辅助功能权限（系统设置 → 隐私与安全 → 辅助功能）
2. 在设置中固定应用
3. 按 Option+Tab 开始使用

---

## 社区反响

该产品在 Hacker News 上引发讨论，主要观点包括：
- 作为 Rectangle + AltTab 的免费替代品获得认可
- 用户赞赏其将应用切换、窗口平铺和工作区管理整合到一个应用的设计
- 与 AltTab 的主要区别在于：HopTab 固定特定应用而非显示所有应用
- 与 Rectangle 的主要区别：平铺与配置文件和布局集成

---

## 价格与许可

- **价格**：免费
- **许可**：开源 (MIT License)
- **遥测**：无
- **大小**：约 3MB

---

*报告生成时间：2026-03-24 16:43:12*
