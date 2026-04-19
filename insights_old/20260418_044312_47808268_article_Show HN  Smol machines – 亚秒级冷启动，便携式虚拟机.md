# SmolVM 仓库洞察报告

## 基本信息

| 项目 | 信息 |
|------|------|
| **仓库链接** | https://github.com/smol-machines/smolvm |
| **描述** | Tool to build & run portable, lightweight, self-contained virtual machines |
| **Stars** | 624 |
| **Forks** | 29 |
| **许可证** | Apache-2.0 |
| **主要语言** | Rust 82.3%, Shell 15.3%, TypeScript 2.4% |
| **提交数** | 471 |
| **贡献者** | 7 |
| **最新版本** | v0.5.17 (2026年4月17日) |

## 项目概述

SmolVM 是一个 CLI 工具，用于构建和运行轻量级、自包含的虚拟机器。其核心特点是：

1. **快速启动** - 冷启动时间 <200ms
2. **跨平台** - 支持 macOS 和 Linux
3. **弹性内存** - 通过 virtio balloon 实现按需分配和回收内存
4. **可移植打包** - 可将虚拟机打包为单个 `.smolmachine` 文件

## 核心技术

- **虚拟化技术**: 使用 Hypervisor.framework (macOS) 或 KVM (Linux)
- **VMM**: 基于 libkrun
- **内核**: 自定义内核 libkrunfw
- **默认配置**: 4 vCPU, 8 GiB RAM

## 主要功能

### 1. 沙箱隔离
运行不受信任的代码，使用硬件级隔离。默认无网络访问，可通过白名单控制出站流量。

### 2. 便携式打包
将任意工作负载打包为自包含二进制文件，无需安装步骤，依赖预装，<200ms 启动。

### 3. 持久化开发环境
创建、停止、启动虚拟机，安装的包在重启后保留。

### 4. SSH 代理转发
将主机 SSH 代理转发到虚拟机，私钥永不进入客户机。

### 5. Smolfile 声明式配置
使用 TOML 文件声明环境配置，实现可复现的虚拟机配置。

## 平台支持

| 主机 | 客户机 | 要求 |
|------|--------|------|
| macOS Apple Silicon | arm64 Linux | macOS 11+ |
| macOS Intel | x86_64 Linux | macOS 11+ |
| Linux x86_64 | x86_64 Linux | KVM (/dev/kvm) |
| Linux aarch64 | aarch64 Linux | KVM (/dev/kvm) |

## 与其他方案对比

| 特性 | SmolVM | 容器 | Colima | QEMU | Firecracker |
|------|--------|------|--------|------|-------------|
| 隔离级别 | VM per workload | Namespace | Namespace | Separate VM | Separate VM |
| 启动时间 | <200ms | ~100ms | ~seconds | ~15-30s | <125ms |
| macOS 原生 | Yes | Via Docker | Yes | Yes | No |
| 可移植产物 | .smolmachine | Images | No | No | No |

## 已知限制

- 网络默认关闭，需使用 `--net` 开启
- Volume 挂载仅支持目录，不支持单文件
- macOS 二进制需要签名和 Hypervisor.framework 权限
- `--ssh-agent` 需要主机运行 SSH 代理

## 适用场景

1. **安全沙箱** - 隔离运行不受信任的程序
2. **便携应用** - 打包预配置环境为单一可执行文件
3. **开发环境** - 持久化的开发环境配置
4. **安全开发** - 使用 git 和 SSH 而不暴露主机密钥
5. **CI/CD** - 可复现的构建和测试环境