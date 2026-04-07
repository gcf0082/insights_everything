# 洞察报告：Show HN - Stop paying for Dropbox/Google Drive, use your own S3 bucket instead

## 基本信息

| 项目 | 内容 |
|------|------|
| **链接** | https://news.ycombinator.com/item?id=47673394 |
| **标题** | Show HN: Stop paying for Dropbox/Google Drive, use your own S3 bucket instead |
| **发布者** | Zm44 |
| **发布时间** | 3小时前 |
| **得分** | 151 points |
| **评论数** | 126 comments |
| **网站** | locker.dev |

## 项目概述

Locker 是一个开源的 Google Drive/Dropbox 替代方案，具有以下特点：
- **提供商无关**：支持 S3、R2、Vercel Blob、本地存储
- **自带存储（BYOB）**：用户自备存储桶
- **虚拟文件系统**
- **QMD Search 插件**

## 主要讨论观点

### 1. 这真的是 Dropbox 替代品吗？

**质疑方**：
- Dropbox/Google Drive 的核心价值在于深度集成操作系统的桌面和移动客户端，提供如同本地文件夹般的同步体验
- 仅靠存储层无法替代完整的同步客户端体验
- 文件同步涉及复杂问题，如多线程冲突写入处理、数据一致性和持久性

**反驳方**：
- 用户可以 self-host 并自行验证源代码
- Syncthing 已提供类似的开源同步解决方案

### 2. 现有替代方案

- **Nextcloud**：开源 Dropbox 替代品，支持 iOS Files 应用集成
- **Syncthing**：免费开源，支持电脑和手机，可穿透 NAT，支持本地发现
- **OpenCloud**：基于 S3 存储，易于安装
- **Maestral**：macOS 上的开源 Dropbox 客户端
- **rclone**：支持客户端加密，可无缝对接 S3 兼容存储

### 3. 成本对比

| 方案 | 价格 |
|------|------|
| Dropbox 2TB | $120/年 |
| Cloudflare R2 | $30/月 |
| Wasabi | $14/月 |
| Backblaze | $10/月 |
| AWS S3 | $20-30/月（仅存储）|

评论指出，S3 存储成本可能比 Dropbox 更高，特别是考虑到流量费用。

### 4. 移动端集成挑战

- iOS 上 Dropbox 与 Files 应用集成，用户可标记文件夹为"离线可用"
- 移动平台本身在对抗"文件夹同步"模式，鼓励应用将数据保留在私有数据库中
- 数据库实际上是更好的多设备、间歇连接同步的数据模型

### 5. 项目成熟度担忧

- 项目仅存在一周，被质疑为"vibe-coded"
- 没有保证一个月后仍会修复安全漏洞
- 文件同步不是那么简单——第一次三方冲突就会导致问题

### 6. 自托管的优缺点

**优点**：
- 数据主权
- 可验证源代码
- 无供应商锁定

**缺点**：
- 维护成本高
- 需要处理同步失败和排除规则
- 硬件和托管费用（如 OVH 16TB 服务器约 $35/月）

### 7. 存储提供商建议

- **Minio**：开源 S3 兼容存储引擎
- **Garage**：开源 S3 引擎
- **Rustfs**：Rust 编写的 S3 兼容存储

## 总结

Locker 项目代表了使用自有 S3 存储桶替代商业云存储的尝试，但社区普遍认为：

1. **存储只是冰山一角**：Dropbox 的真正价值在于其成熟的客户端和同步体验
2. **现有解决方案丰富**：Nextcloud、Syncthing 等已提供成熟的开源替代
3. **成本需仔细计算**：自托管可能比直接购买 Dropbox 更昂贵
4. **项目需时间验证**：作为一周大的项目，其长期维护和安全性存疑

对于大多数用户而言，直接使用现有服务（如 Dropbox）可能仍是更务实的选择，除非有特殊的数据主权或成本优化需求。