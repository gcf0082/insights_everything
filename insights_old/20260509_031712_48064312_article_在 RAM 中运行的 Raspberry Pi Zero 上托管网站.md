# 洞察报告

## 基本信息

- **洞察链接**：https://btxx.org/posts/memory/
- **来源网站**：btxx.org
- **文章标题**：Serving a Website on a Raspberry Pi Zero Running Entirely in RAM
- **发布日期**：2026-05-08
- **报告生成时间**：2026-05-09

---

## 核心洞察

本文介绍如何在一台仅有512MB内存的Raspberry Pi Zero上运行一个完全运行在内存中的网站，展示了极简自我托管的可行性。

### 主要内容

1. **硬件配置**：使用Raspberry Pi Zero v1.3配合512MB microSD卡（仅用于启动），通过Waveshare Ethernet HAT或OTG适配器连接网络。

2. **操作系统**：采用Alpine Linux的磁盘无模式（diskless mode），整个系统运行在RAM中。

3. **软件栈**：
   - `darkhttpd`（轻量级静态HTTP服务器）
   - `dropbear`（轻量级SSH服务器）
   - `lbu`（Alpine的配置持久化工具）
   - `rsync`（文件同步）

4. **架构设计**：使用外部VPS（TierHive，128MB内存，$4/年）处理TLS终止和流量转发，Pi Zero仅负责提供HTTP内容。

5. **备份方案**：通过`dd`命令创建microSD卡的字节级镜像备份。

### 关键亮点

- **内存充足时代**：作者调侃512MB内存"虽然便宜但也无法忽视"，暗示极低配置硬件足以运行网站。
- **TLS处理外置**：将TLS终止委托给VPS，让Pi Zero专注于静态内容服务。
- **备份简便**：由于系统完全运行在内存中，可以随时拔除microSD卡进行离线备份。

### 技术细节

- 使用`lbu commit -d`保存所有配置更改到SD卡
- 通过VPS上的`socat`将互联网流量转发到本地Pi Zero
- TierHive提供内置的自动SSL续期功能

### 总结

该方案展示了利用极低成本硬件（Pi Zero + 廉价VPS）实现完全自我托管网站的可行性，适合个人项目或技术爱好者实验。
