# 洞察报告：GrapheneOS修复Android VPN漏洞

## 基本信息

- **洞察链接**: https://cyberinsider.com/grapheneos-fixes-android-vpn-leak-google-refused-to-patch/
- **发布日期**: 2026年5月6日
- **来源**: CyberInsider
- **作者**: Alex Lekander

## 摘要

GrapheneOS发布了一项更新，修复了一个新披露的Android VPN绕过漏洞，该漏洞能够泄露用户的真实IP地址。即使在Android的"始终保持VPN连接"和"阻止未使用VPN的连接"保护已启用的情况下，该漏洞仍然可以导致IP泄露。

## 漏洞详情

### 影响范围
- **受影响版本**: Android 16
- **漏洞类型**: VPN绕过漏洞
- **影响设备**: Google Pixel设备

### 技术原理
该漏洞源于Android网络堆栈中新引入的QUIC连接关闭功能。安全研究员"lowlevel/Yusuf"发现，利用该漏洞的API允许仅拥有自动授予的INTERNET和ACCESS_NETWORK_STATE权限的普通应用程序，向system_server注册任意的UDP有效载荷。

当应用程序的UDP套接字被销毁时，Android的特权system_server进程会通过设备的物理网络接口直接传输存储的有效载荷，而不是通过VPN隧道。由于system_server具有提升的网络权限且不受VPN路由限制，数据包完全绕过了Android的VPN锁定保护。

### 攻击演示
研究人员在运行Android 16并启用Proton VPN和Android锁定模式的Pixel 8设备上演示了该漏洞。报告显示，尽管VPN保护已完全启用，应用程序仍能将设备的真实公网IP地址泄露到远程服务器。

## 谷歌的回应

谷歌将该问题分类为"不会修复（不可行）"和"NSBC"（不属于安全公告类别），表示该问题不符合纳入Android安全公告的阈值。研究人员申诉该决定，认为任何应用程序都可以仅使用标准权限泄露可识别的网络信息，但谷歌维持其立场，并于4月29日授权公开披露。

## GrapheneOS的修复

GrapheneOS在最新版本（2026050400）中禁用了底层的registerQuicConnectionClosePayload优化，有效中和了受支持Pixel设备上的攻击向量。GrapheneOS在不到一周的时间内发布了修复补丁。

## 其他更新

除VPN漏洞修复外，最新版本还包括：
- 完整的2026年5月Android安全补丁级别
- 多个hardened_malloc改进
- Android 6.1、6.6和6.12分支的Linux内核更新
- libpng中CVE-2026-33636的后向端口修复
- 较新的Vanadium浏览器版本
- 扩展的动态代码加载限制

## 缓解措施

对于标准Android用户，研究人员指出可以通过ADB手动禁用close_quic_connection DeviceConfig标志来临时缓解该问题。但是，该解决方法需要开发者访问权限，如果谷歌在未来更新中移除该功能标志，可能无法永久生效。

## 总结

这一事件再次凸显了隐私优先操作系统与主流Android在安全响应方面的差异。GrapheneOS作为专注于隐私和安全的Android替代系统，展现了对用户安全需求的快速响应能力，而谷歌则选择不修复该漏洞。