# 洞察报告

**洞察链接**: https://astrid.tech/2026/03/24/0/curl-to-dev-sda/

**发布日期**: 2026-03-24

**标签**: Linux, http

---

## 文章概述

本文是四部分系列文章的第0部分，探讨如何在Linux启动早期执行一些"奇怪"的操作。

## 核心内容

### 基本原理

可以通过以下命令直接将下载的磁盘镜像写入磁盘：

```bash
curl https://something.example/foo.img > /dev/sda
```

无需先保存到文件，直接通过管道将数据写入磁盘设备。这是因为 `/dev/sdX`、`/dev/nvmeX` 等磁盘设备可以直接从文件系统访问，遵循古老的Unix"一切皆文件"概念。

### 作者的动机

作者最初这样做是因为不想每月多付$1.50来购买对象存储来托管VPS镜像。这是一个有趣的"梗"，效仿经典的 `curl | sh` 争论。

### 逐步演进

1. **传统方式**：使用 `sudo dd bs=1M if=your_image_file.img of=/dev/sdx`
2. **更懒**：用 `curl` 替代浏览器下载 `curl https://... | sudo dd bs=1M of=/dev/sdx`
3. **更懒**：直接重定向 `sudo curl https://... > /dev/sdx`
4. **更蠢**：甚至可以在运行的系统上直接写入磁盘（虽然会崩溃）

### 实际操作

作者在Contabo的VPS上测试，通过救援模式启动系统，然后执行：

```bash
wget -O- https://something.example/foo.img | dd bs=1M of=/dev/sda status=progress
```

### 进一步思考

既然写入磁盘所需的所有工具已经存在于磁盘上，理论上可以将这些工具复制到内存中卸载所有内容，然后从内存中重写磁盘来实现原地交换。

---

*本文是系列文章的第0部分，后续内容请关注第1部分。*