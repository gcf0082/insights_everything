# LocalSend 项目洞察报告

**洞察链接**: https://github.com/localsend/localsend
**生成时间**: 2026-04-29

## 项目概述

LocalSend 是一个免费、开源的跨平台应用，允许用户通过本地网络在附近设备之间安全地共享文件和消息，无需互联网连接。

## 基本信息

| 项目指标 | 数据 |
|----------|------|
| 项目名称 | LocalSend |
| 类型 | 跨平台文件/消息传输应用 |
| 主语言 | Dart (87.5%) |
|Stars | 79.6k |
|Forks | 4.3k |
|Commit数 | 1,848 |
|许可证 | Apache-2.0 |

## 核心特性

- **跨平台支持**: Windows、macOS、Linux、Android、iOS、Fire OS
- **离线传输**: 无需互联网连接，通过本地网络进行设备间的直接通信
- **安全加密**: 使用 REST API 和 HTTPS 加密传输，所有数据通过 TLS/SSL 证书保护
- **无需第三方服务器**: 完全基于本地网络通信，不依赖外部服务器

## 技术架构

- **框架**: Flutter + Rust
- **语言占比**:
  - Dart: 87.5%
  - Rust: 8.8%
  - C++: 0.7%
  - Swift: 0.6%
  - CMake: 0.5%
  - 其他: 1.4%

## 兼容性

| 平台 | 最低版本 |
|------|---------|
| Android | 5.0 |
| iOS | 12.0 |
| macOS | 11 Big Sur |
| Windows | 10 |

## 下载渠道

- **Windows**: Winget, Scoop, Chocolatey, 便携版 ZIP
- **macOS**: App Store, Homebrew, DMG 安装包
- **Linux**: Flathub, Snap, AUR, Nixpkgs, AppImage, DEB
- **Android**: Play Store, F-Droid, APK
- **iOS**: App Store
- **Fire OS**: Amazon

## 工作原理

LocalSend 使用安全的通信协议，允许设备通过 REST API 相互通信。所有数据通过 HTTPS 安全发送，每个设备动态生成 TLS/SSL 证书，确保最大安全性。

## 端口配置

| 流量类型 | 协议 | 端口 |
|----------|------|------|
| 传入 | TCP, UDP | 53317 |
| 传出 | TCP, UDP | 任意 |

## 社区信息

- **Issues**: 890
- **Pull Requests**: 44
- **贡献者**: 众多来自全球的开发者
- **翻译支持**: 支持 20+ 种语言（包括中文）

## 最近更新

- 最新版本: v1.17.0 (2025年2月20日)
- 便携模式支持 (v1.13.0)
- 隐藏启动支持 (v1.15.0)

## 总结

LocalSend 是一个设计精良的开源项目，作为 AirDrop 的跨平台替代方案，具有良好的安全性和广泛的平台支持。项目拥有活跃的社区和大量的 Stars，表明其受欢迎程度和实用性。