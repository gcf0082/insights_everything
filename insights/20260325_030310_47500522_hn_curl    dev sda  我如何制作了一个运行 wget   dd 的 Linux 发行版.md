# 洞察报告：curl > /dev/sda: How I made a Linux distro that runs wget | dd

**洞察链接**: https://news.ycombinator.com/item?id=47500522  
**来源**: Hacker News  
**发布日期**: 2026年3月24日  
**得分**: 126 points  
**评论数**: 52 comments  
**原文链接**: https://astrid.tech/2026/03/24/0/curl-to-dev-sda/

---

## 基本信息

本文作者分享了如何创建一个可以在运行中直接通过 `wget | dd` 命令将系统镜像写入磁盘的 Linux 发行版。这是一个看似疯狂但实际可行的技术探索，目标是实现"把系统镜像直接dump到磁盘然后重启"的极简安装方式。

---

## 核心洞察

### 1. 安全风险

- **内核仍会写入**: 即使用户主动写入磁盘，内核仍可能向其认为是旧文件系统的设备写入数据，导致镜像损坏
- **解决方案**: 在第二部分中通过从 initrd 挂载只读来解决此问题

### 2. 技术方案

| 方案 | 描述 |
|------|------|
| **kexec** | 切换到新内核+initramfs，停止所有文件系统访问后再写入镜像 |
| **pivot_root** | 将文件移动到新目录，执行 pivot_root 后安装新系统 |
| **initramfs** | 使用 rd.break 内核参数停留在 initramfs 环境 |
| **LVM 技巧** | 将 LVM PV 移到 /dev/shm，整个系统运行在内存中 |
| **sysrq** | 使用 sysrq 强制只读挂载 |

### 3. 虚拟机启动物理分区

- QEMU 可以使用 overlay 模式（snapshot=on）从 /dev/sda 启动，避免穿透写入
- VirtualBox 可以直接启动物理 Windows 分区（需避免安装 guest 驱动）

### 4. 内核配置

- Linux 6.8 引入了 CONFIG_BLK_DEV_WRITE_MOUNTED 选项，允许在挂载状态下写入块设备

---

## 社区反馈亮点

1. **实用场景**: 在 VPS 上安装不受支持的操作系统（如在 Oracle Cloud 上安装 Alpine）
2. **历史呼应**: 这种方式类似于过去的软盘安装系统
3. **企业应用**: 有公司使用 rsync 从存储服务器同步镜像来批量部署机器
4. **限制**: GPT 头对齐问题可能导致不同扇区大小的目标驱动器失败

---

## 系列文章

本文是多部分系列的一部分：
- Part 1: https://astrid.tech/2026/03/24/0/curl-to-dev-sda/
- Part 2: https://astrid.tech/2026/03/24/1/swap-out-the-root-before-boot/
- Part 3: https://astrid.tech/2026/03/24/2/how-to-pass-secrets-between-reboots/

---

## 总结

这是一个极具创意的技术实验，虽然存在数据损坏风险，但社区提供了多种安全实现的方案。该项目展示了 Linux 系统的灵活性，以及在受限环境下（如 VPS）安装任意操作系统的可能性。