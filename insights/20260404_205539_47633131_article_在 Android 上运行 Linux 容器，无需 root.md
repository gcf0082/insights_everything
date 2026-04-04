# 洞察报告：ExTV/Podroid

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://github.com/ExTV/Podroid |
| **仓库名称** | ExTV/Podroid |
| **描述** | Rootless Podman for Android — run Linux containers on your phone |
| **星标数** | 274 |
| **Fork数** | 6 |
| **开源协议** | GNU General Public License v2.0 |
| **主分支** | main |
| **主要语言** | Kotlin 86.9%, Shell 11.2%, Dockerfile 1.9% |
| **最新发布** | v1.0.6 (2026年4月3日) |
| **贡献者数** | 1人 |

## 项目简介

Podroid 是一款无需 root 权限即可在 Android 设备上运行 Linux 容器的应用。它通过 QEMU 在手机上启动一个轻量级的 Alpine Linux 虚拟机，并提供完整的 Podman 容器运行时和内置串行终端。

## 核心特性

1. **容器支持**：可拉取和运行任何 OCI 镜像
2. **终端模拟**：完整的 xterm 仿真，支持 Ctrl、Alt、F1-F12、方向键等
3. **持久化**：包、配置和容器镜像可跨重启保存
4. **网络功能**：开箱即用的互联网访问，支持端口转发
5. **无依赖**：无需 root、无需 Termux、无需宿主二进制文件，仅需安装 APK

## 技术架构

```
Android App
├── Foreground Service (keeps VM alive)
├── PodroidQemu
│   ├── libqemu-system-aarch64.so  (QEMU TCG, no KVM)
│   ├── Serial stdio ←→ TerminalEmulator
│   └── QMP socket (port forwarding, VM control)
└── Alpine Linux VM
    ├── initramfs (read-only base layer)
    ├── ext4 disk (persistent overlay)
    ├── getty on ttyAMA0 (job control)
    └── Podman + crun + netavark + slirp4netns
```

## 项目结构

```
Dockerfile                  # Multi-stage initramfs builder (Alpine aarch64)
docker-build-initramfs.sh   # One-command build script
init-podroid                # Custom /init for the VM

app/src/main/
├── java/com/excp/podroid/
│   ├── engine/             # QEMU lifecycle, QMP client, VM state machine
│   ├── service/            # Foreground service with boot-stage notifications
│   ├── data/repository/    # Settings + port forward persistence
│   └── ui/screens/         # Home, Terminal, Settings (Jetpack Compose)
├── jniLibs/arm64-v8a/      # Pre-built QEMU + libslirp
└── assets/                 # Kernel + initramfs (generated)
```

## 使用要求

- ARM64 Android 设备
- Android 8.0+ (API 26)
- 约 150 MB 可用存储空间

## 快速开始

1. 从 Releases 安装 APK
2. 打开 Podroid 并点击 "Start Podman"
3. 等待启动（约 20 秒）
4. 点击 "Open Terminal"
5. 运行容器：`podman run --rm alpine echo hello`

## 技术亮点

- **两阶段启动**：QEMU 加载 vmlinuz-virt + initrd.img，通过 init-podroid 挂载持久化 ext4 磁盘作为 overlayfs 上层
- **终端接线**：应用无法 fork 宿主进程，TerminalSession 通过反射直接连接到 QEMU 的串行 I/O
- **网络转发**：QEMU 用户模式网络（SLIRR），端口转发使用 QEMU 的 hostfwd

## 致谢

- QEMU — 机器仿真
- Alpine Linux — 虚拟机基础
- Podman — 容器运行时
- Termux — 终端模拟器库
- Limbo PC Emulator — Android 上 QEMU 的先驱

---

*报告生成时间：2026-04-04*
