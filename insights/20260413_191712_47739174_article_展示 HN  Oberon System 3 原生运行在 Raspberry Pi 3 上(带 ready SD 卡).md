# OberonSystem3Native 项目发布总结

**洞察链接**: https://github.com/rochus-keller/OberonSystem3Native/releases

**项目概述**: Oberon System 3 是一个原生实现的操作系统，本仓库实现了在 Raspberry Pi 和 x86 架构上的移植。

**发布日期**: 2026-04-13

---

## 发布版本汇总

### 1. Native Raspberry Pi 3b 版本 (2026-04-10)

**最新版本** - 终于成功在 Raspberry Pi 3b 上运行！

**主要特性**:
- 成功在 Raspberry Pi 3b 上原生运行
- 支持的型号: Raspberry Pi 3b, Pi 2b (>= v1.2), Pi Zero 2
- 这些型号将持续生产至 2028-2030 年
- 未来计划迁移到 Raspberry Pi 4

**发布内容**:
- 预编译镜像: oberon-rpi3.img
- Raspberry Pi 启动文件
- 预编译的 Linux x64 工具链

**构建说明**:
- Linux 系统: 使用 `sudo dd if=oberon-rpi3.img of=/dev/sdX bs=4M conv=fsync status=progress && sync`
- Windows/Mac: 可使用 Raspberry Pi Imager 或 Etcher 烧录

---

### 2. ARMv7 版本 - MVP (2026-04-02)

**版本名称**: MVP ARMv7 version (QEMU raspi2b)

**完成的工作**:
- 内核 (Kernel)、实数 (Reals)、文件系统 (File System) 已完全移植到 32 位 ARM 架构
- 平台特定驱动已移植: 显示 (Display)、USB、数学 (Math)
- 系统在 QEMU 10.2 模拟的 Raspberry Pi 2B 上成功启动和运行

**构建速度**: 在现代机器上编译模块、静态链接核心、生成 AosFs 分区并填充所有运行时文件，整个过程**不到一分钟**完成

**后续计划**:
- 通过 JTAG 在真实硬件上调试
- 目标硬件: Raspberry Pi 2B, 3B, Zero 2
- 移植网络驱动 (至少以太网，WiFi 可能工作量过大)

---

### 3. i386 版本 - MVP (2026-03-06)

**版本名称**: MVP i386 version

**完成状态**:
- 355/358 个模块成功构建 (来自 i386 和 portable 目录)
- 系统运行足够稳定

**构建速度**: 完整的构建包括创建驱动和上传所有文件，在 Lenovo T480 (Debian Bookworm x64) 上仅需 **51 秒**

---

## 技术亮点

1. **快速构建**: 使用自定义 C99 工具链，整个系统从零构建非常快速
2. **多平台支持**: 已支持 x86 和 ARM (Raspberry Pi 系列)
3. **持续维护**: Raspberry Pi 3b 将持续生产至 2028 年，确保项目长期可用

---

## 总结

OberonSystem3Native 项目目前已发布三个主要版本，从最初的 i386 MVP 到 ARMv7 QEMU 版本，最新版本已成功在真实 Raspberry Pi 3b 硬件上运行。项目展现了出色的移植性和快速构建能力，为 Oberon System 3 在现代硬件上的运行提供了可能。