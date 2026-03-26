# WolfGuard 项目洞察报告

## 基本信息

- **洞察链接**: https://github.com/wolfssl/wolfguard
- **项目名称**: wolfSSL/wolfGuard
- **项目类型**: FIPS合规的WireGuard VPN实现
- **编程语言**: C (88.3%), Roff (3.7%), C++ (3.7%), Shell (2.4%), Makefile (1.9%)
- **许可证**: GPL-2.0
- **Star数**: 94
- **Fork数**: 8

## 项目概述

WolfGuard是wolfSSL对Linux内核版WireGuard（由Jason Donenfeld设计和编写）的FIPS 140-3合规重构版本。功能使用与WireGuard基本相同。主要包含两个核心组件：`wolfguard.ko`内核模块和`wg-fips`配置工具。

## 核心特性

### 算法映射

WolfGuard将WireGuard的密码学算法重新映射如下：

| 算法类别 | WireGuard | WolfGuard |
|---------|-----------|-----------|
| ECDH | Curve25519 | SECP256R1 |
| AEAD | XChaCha20-Poly1305 | AES-256-GCM |
| 摘要 | Blake2s | SHA2-256 |
| 认证摘要 | Blake2s-HMAC | SHA2-256-HMAC |
| 内部哈希 | SipHash | SHA2-256 |
| DRBG | ChaCha20 DRBG | SHA2-256 Hash-DRBG |

### 主要特性

- **FIPS 140-3合规**: 使用经过FIPS认证的密码学算法
- **与WireGuard共存**: 两者可同时存在于同一系统，建立各自的隧道
- **透明替换**: 通过符号链接实现对WireGuard的透明替换
- **高性能**: 启用Intel ASM加速后，性能可达到或超过CPU加速的WireGuard
- **压缩公钥支持**: 可选支持压缩公钥

### 性能说明

- 启用`--enable-intelasm`选项后，WolfGuard性能可达到或超过CPU加速的WireGuard
- 未启用时，性能略低于CPU加速的WireGuard，但仍足以在现代CPU上饱和千兆以太网

## 构建与安装

### 组件依赖

- `wolfguard.ko`内核模块依赖`libwolfssl.ko`内核模块
- `wg-fips`依赖`libwolfssl.so`用户库
- 这些依赖项从同一wolfSSL源代码构建，分别配置为内核模块和用户库

### 构建流程（非FIPS）

1. 克隆wolfSSL和wolfGuard源代码
2. 构建并安装libwolfssl用户库
3. 构建并安装wg-fips用户工具
4. 构建并安装libwolfssl内核模块
5. 构建并安装wolfguard内核模块

### 构建流程（FIPS）

需要使用FIPS认证版本的wolfSSL源代码，并联系fips@wolfssl.com获取。

## 目录结构

```
wolfguard/
├── kernel-src/    # 内核模块源代码
├── user-src/      # 用户空间工具源代码
├── COPYING        # 许可证文件
├── README.md      # 项目说明
└── .gitignore     # Git忽略文件
```

## 使用说明

- 配置文件位置：`/etc/wolfguard`（替代WireGuard的`/etc/wireguard`）
- 密钥生成：使用WolfGuard的`wg-fips`工具
- 现有WireGuard playbook和脚本可直接使用，只需替换配置目录和密钥生成工具

## 联系与支持

- 联系邮箱：fips@wolfssl.com
- 需要FIPS认证版本请联系上述邮箱

## 总结

WolfGuard是一个将WireGuard转换为FIPS 140-3合规解决方案的项目，通过使用SECP256R1、AES-256-GCM、SHA2-256等经认证的密码学算法替代原WireGuard中使用的Curve25519、XChaCha20-Poly1305、Blake2s等算法。它允许用户在保持与WireGuard相似使用体验的同时，满足FIPS合规要求。
