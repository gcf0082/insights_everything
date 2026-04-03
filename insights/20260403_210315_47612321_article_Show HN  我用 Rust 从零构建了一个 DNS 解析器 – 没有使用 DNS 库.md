# Numa 项目洞察报告

**洞察链接**: https://github.com/razvandimescu/numa

## 基本信息

| 项目属性 | 详情 |
|---------|------|
| 项目名称 | Numa |
| 作者 | razvandimescu |
| 星标数 | 168 |
| 分叉数 | 8 |
| 主要语言 | Rust (72.6%) |
| 许可证 | MIT |
| 最新版本 | v0.9.1 (2026年4月2日) |
| 提交数 | 126 次 |
| 主题标签 | resolver, dns, rust, service-discovery, reverse-proxy, tokio, developer-tools, dns-server, local-development, ad-blocking |

## 项目简介

**Numa** 是一个用 Rust 编写的便携式 DNS 解析器，单一二进制文件即可运行。它具有以下核心功能：

- **广告拦截**: 通过 Hagezi Pro 列表拦截 385K+ 域名
- **本地服务域名**: 支持 `.numa` 域名的自动 TLS 证书和 WebSocket 代理
- **局域网服务发现**: 通过 mDNS 自动发现局域网内的服务
- **开发者覆盖**: 提供 REST API 允许动态覆盖任意主机名
- **递归解析**: 支持从根域名服务器直接解析，无需上游 DNS
- **DNSSEC 验证**: 支持完整的信任链验证，包括 RSA、ECDSA、Ed25519

## 核心特性

### 1. 本地服务代理

开发者可以为本地服务分配 `.numa` 域名，无需记忆端口号：

```bash
curl -X POST localhost:5380/services \
  -d '{"name":"frontend","target_port":5173}'
```

访问 `https://frontend.numa` 即可获得有效的 TLS 证书和 WebSocket 支持。

### 2. 三种解析模式

- **forward** (默认): 透明代理到现有系统 DNS，支持所有网络环境
- **recursive**: 直接从根域名服务器解析，保护隐私
- **auto**: 启动时探测根服务器，可递归则递归，否则回退到 DoH

### 3. 性能指标

- 缓存查询延迟: 691ns
- 吞吐量: ~2.0M qps
- 递归查询平均延迟: 237ms (SRTT 预热后)
- ECDSA P-256 DNSSEC 验证: 174ns

### 4. 跨平台支持

支持 macOS、Linux、Windows，通过 Homebrew、curl install 或 Cargo 安装，可设置为系统 DNS。

## 与同类项目对比

| 特性 | Pi-hole | AdGuard Home | Unbound | Numa |
|------|---------|--------------|---------|------|
| 本地服务代理 + 自动 TLS | - | - | - | ✅ |
| .numa 域名/WebSocket | - | - | - | ✅ |
| mDNS 零配置 | - | - | - | ✅ |
| 开发者 REST API | - | - | - | ✅ |
| DNSSEC 验证 | - | - | ✅ | ✅ |
| 广告拦截 | ✅ | ✅ | - | ✅ |
| 便携式(笔记本) | - | - | - | ✅ |
| 单一二进制 | - | - | - | ✅ |

## 技术亮点

1. **从零实现**: 不使用任何 DNS 库，手工解析 RFC 1035 协议
2. **零堆分配**: 热点路径无堆内存分配
3. **SRTT 选路**: 基于平滑往返时间选择最优域名服务器
4. **完整 DNSSEC**: RRSIG 签名验证、DNSKEY 验证、DS 委托验证、NSEC/NSEC3 拒绝证明
5. **Hub 模式**: 可作为局域网 DNS 中心，为其他设备提供广告拦截和 .numa 解析

## 路线图

- DNS 转发、缓存、广告拦截、开发者覆盖 ✅ 已完成
- .numa 本地域名、自动 TLS、路径路由、WebSocket 代理 ✅ 已完成
- LAN 服务发现 - mDNS、跨机器 DNS + 代理 ✅ 已完成
- DNS-over-HTTPS 加密上游 ✅ 已完成
- 递归解析 + DNSSEC ✅ 已完成
- SRTT 域名服务器选择 ✅ 已完成
- pkarr 集成 - 通过 Mainline DHT 的自托管 DNS
- 全局 .numa 名称 - DHT 支持，无需注册商

## 总结

Numa 是一个功能全面的 DNS 解决方案，特别适合开发者使用。它将广告拦截、本地服务命名、局域网发现和隐私保护集成为单一的便携式二进制文件，无需云账户或专用硬件即可在任何网络上使用。

---
*报告生成时间: 2026-04-03 21:03:15*