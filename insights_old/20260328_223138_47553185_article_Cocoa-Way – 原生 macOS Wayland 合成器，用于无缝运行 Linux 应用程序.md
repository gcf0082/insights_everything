# 洞察报告：Cocoa-Way

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://github.com/J-x-Z/cocoa-way |
| **项目名称** | Cocoa-Way |
| **项目类型** | 开源软件 - 原生 macOS Wayland 合成器 |
| **编程语言** | Rust (97.4%), Ruby (1.4%), Shell (1.2%) |
| **许可证** | GPL-3.0 |
| **星标数** | 313 |
| **分支数** | 4 |
| **提交数** | 28 |
| **最新版本** | v0.2.0 (2026年1月21日) |

---

## 项目概述

Cocoa-Way 是一个用 Rust 编写的原生 macOS Wayland 合成器，允许用户在 macOS 上无缝运行 Linux 应用程序，无需使用 XQuartz。

## 核心特性

| 特性 | 描述 |
|------|------|
| **原生 macOS** | 使用 Metal/OpenGL 渲染，无缝桌面集成 |
| **零 VM 开销** | 通过 socket 直接传输 Wayland 协议，无虚拟化 |
| **HiDPI 支持** | 针对 Retina 显示器优化，支持 proper scaling |
| **精致 UI** | 服务端装饰、阴影和焦点指示器 |
| **硬件加速** | 高效的 OpenGL 渲染管线 |

## 技术架构

```
┌─────────────────┐     ┌─────────────────┐
│    macOS        │     │   Linux VM/Container │
│                 │     │                   │
│  Cocoa-Way      │     │   waypipe         │
│  (合成器)        │◄───►│   (服务器)        │
│                 │     │                   │
│  waypipe        │     │   Linux 应用      │
│  (客户端)        │     │   (Firefox等)     │
└─────────────────┘     └─────────────────┘
        │                       │
        ▼                       ▼
   Metal/OpenGL           Wayland 协议
        │
   macOS 显示器
```

## 安装方式

### Homebrew（推荐）

```bash
brew tap J-x-Z/tap
brew install cocoa-way waypipe-darwin
```

### 从源码构建

```bash
brew install libxkbcommon pixman pkg-config
git clone https://github.com/J-x-Z/cocoa-way.git
cd cocoa-way
cargo build --release
```

## 使用方法

1. 启动合成器：`cocoa-way`
2. 通过 SSH 连接 Linux 应用：`./run_waypipe.sh ssh user@linux-host firefox`

## 与其他方案对比

| 方案 | 延迟 | HiDPI | 原生集成 | 配置复杂度 |
|------|------|-------|----------|------------|
| **Cocoa-Way** | ⚡ 低 | ✅ | ✅ 原生窗口 | 🟢 简单 |
| XQuartz | 🐢 高 | ⚠️ 部分 | ⚠️ X11 quirks | 🟡 中等 |
| VNC | 🐢 高 | ❌ | ❌ 全屏 | 🟡 中等 |
| VM GUI | 🐢 高 | ⚠️ 部分 | ❌ 独立窗口 | 🔴 复杂 |

## 路线图

- ✅ macOS 后端 (Metal/OpenGL)
- ✅ Waypipe 集成
- ✅ HiDPI 缩放
- 🚧 Windows 后端 ([win-way](https://github.com/J-x-Z/win-way))
- 📱 Android NDK 后端（计划中）
- 多显示器支持
- 剪贴板同步

## 研究背景

该项目是 **"Turbo-Charged Protocol Virtualization"** 研究计划的一部分，探索通过 Rust trait 单态化 + SIMD 加速像素转换实现零成本跨平台 Wayland。

## 技术亮点

1. **协议级别的原生支持**：直接处理 Wayland 协议，而非模拟
2. **Unix Socket 直连**：通过 SSH/Socket 实现 Linux 应用与 macOS 的低延迟通信
3. **渲染优化**：利用 Metal/OpenGL 实现高效的硬件加速渲染

---

*报告生成时间：2026年3月28日*
