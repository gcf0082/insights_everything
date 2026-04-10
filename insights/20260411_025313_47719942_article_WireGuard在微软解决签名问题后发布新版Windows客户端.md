# 洞察报告：WireGuardNT v0.11 和 WireGuard for Windows v0.6 发布

**洞察链接**: https://lists.zx2c4.com/pipermail/wireguard/2026-April/009561.html

**发布者**: Jason A. Donenfeld

**发布日期**: 2026年4月10日

---

## 摘要

WireGuard 团队发布了 Windows 平台的更新版本，包括底层内核驱动 WireGuardNT v0.11 和上层管理软件 WireGuard for Windows v0.6。这是自上次更新以来间隔较久的重要发布。

## 主要更新内容

### 新功能
- 支持单独移除 Allowed IPs 而不丢包（已在 Linux 和 FreeBSD 版本中实现）
- 支持在 IPv4 连接上设置极低的 MTU

### 主要改进
- 大量累积的 bug 修复
- 性能提升
- 代码大幅精简：通过提高最低支持的 Windows 版本，消除了数十年的兼容性代码、替代路径、奇怪逻辑和动态调度等问题
- 工具链全面更新：
  - EWDK 版本
  - Clang/LLVM/MingW 版本
  - Go 版本
  - EV 证书和签名基础设施

### 测试范围
- 包括在 Windows 10 1507 Build 10240（最古老的受支持版本）上测试

## 关于签名账户事件的说明

发布者解释了最近的新闻报道：团队在向 Microsoft 提交新 NT 内核驱动签名时，账户曾被暂停。Jason Donenfeld 先在 Hacker News 上评论，随后在 Twitter 上发布。事件发生后引发网络讨论，但经 Microsoft 介入后，账户很快被解封。发布者认为这是官僚流程失控的个例，并无恶意或阴谋，目前新闻报道尚未更新此后续进展。

## 获取方式

- 下载链接: https://download.wireguard.com/windows-client/wireguard-installer.exe
- 安装页面: https://www.wireguard.com/install/
- 项目信息:
  - WireGuard for Windows: https://git.zx2c4.com/wireguard-windows/about/
  - WireGuardNT: https://git.zx2c4.com/wireguard-nt/about/

---

*报告生成时间: 2026-04-11*