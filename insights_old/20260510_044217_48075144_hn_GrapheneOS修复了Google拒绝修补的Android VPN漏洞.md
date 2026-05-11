# 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **链接** | https://news.ycombinator.com/item?id=48075144 |
| **标题** | GrapheneOS fixes Android VPN leak Google refused to patch |
| **来源** | Hacker News |
| **发布时间** | 6小时前 |
| **得分** | 195 points |
| **评论数** | 56 comments |
| **提交者** | Georgelemental |

---

## 摘要

GrapheneOS在其最新版本中修复了一个Android VPN漏洞，该漏洞允许流量绕过VPN连接。值得注意的是，Google此前拒绝修复此安全问题，并将其归类为"非安全公告级别"的问题。

## 技术细节

### 漏洞原理

1. **问题核心**：Android的`system_server`进程具有 elevated networking privileges（提升的网络权限），不受VPN路由限制
2. **根本原因**：一个新出现的"隐藏"网络API `registerQuicConnectionClosePayload(fd, payload)` 允许进程设置任意字节数组，由操作系统代表其发送
3. **权限检查缺失**：在通过操作系统拥有的UDP socket发送数据时，没有针对调用uid/process进行("paranoid networking")权限检查
4. **沙箱逃逸**：该漏洞实际上实现了app sandbox escape和privilege escalation

### GrapheneOS的解决方案

GrapheneOS在release 2025050400中通过禁用`registerQuicConnectionClosePayload`优化来修复此漏洞，有效中和了攻击向量。

## 讨论焦点

### 1. Google的行为受到质疑

- Google将此漏洞归类为"非安全公告级别"引发广泛批评
- 评论者指出，Android的lockdown mode明确承诺"没有流量可以绕过VPN"，但当系统本身通过物理接口发送数据包时，该承诺在内核层面被打破
- 有评论指出这可能是feature而非bug，Google作为广告公司和国防承包商，可能有动机让VPN用户泄露流量

### 2. 其他操作系统的类似问题

- **iOS**：存在相同问题，唯一解决方式是需要企业证书（250+设备）
- **macOS**：也有自己的应用可以绕过always-on VPN的案例

### 3. 获取GrapheneOS设备的讨论

- 目前GrapheneOS仅支持Pixel手机
- 二手Pixel价格通常超过300美元，新Pixel 10a约为449美元
- GrapheneOS已宣布与Motorola合作，预计年底前将有支持GrapheneOS的Motorola设备
- 建议购买Pixel 6或更高版本（Pixel 7或更高更佳），确保bootloader可解锁
- 运营商销售的Pixel通常无法解锁bootloader

### 4. QUIC协议的争议

- 有评论指出GrapheneOS通过"禁用优化"来修复漏洞
- QUIC协议可能服务于他人利益，但对用户来说权衡并不值得
- QUIC在Google分发的软件（如Android）中默认开启，某些情况下无法禁用

### 5. 隐私与安全替代方案

- 有用户建议使用privacy gateways上游的方案，因为隐私不是这些公司的利润中心
- 讨论了CalyxOS作为更简单的替代方案
- 讨论了Play Integrity的反竞争性质及其对独立操作系统的限制

---

## 关键评论精选

> "The technical detail that makes this egregious is that the leak happens in system_server, a privileged process. Android's own lockdown mode explicitly promises that no traffic bypasses the VPN."

> "How can someone consider unwanted disclosure of personal information a security issue, and work at Google?"

> "It's a feature for them not a bug. Google is an ad company and an offense contractor they want VPN users leaking packets for both reasons."

> "I finally left Verizon after nearly 20 years... I switched to US Mobile and on the Darkstar (AT&T) network. I have no regrets."

---

## 总结

此事件揭示了Android安全模型的重大缺陷。尽管用户期望VPN提供隐私保护，但Google拒绝修复这一根本性的安全漏洞。GrapheneOS作为专注于安全的替代Android发行版，通过禁用相关优化来保护用户隐私。社区对此事件的反应显示了对Google处理方式的不满，同时也反映了移动操作系统生态系统中隐私与商业利益之间的持续冲突。