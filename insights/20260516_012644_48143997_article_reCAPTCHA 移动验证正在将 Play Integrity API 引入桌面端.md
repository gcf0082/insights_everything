# 洞察报告：reCAPTCHA Mobile Verification正在将Play Integrity API引入桌面平台

## 基本信息

- **洞察链接**：https://discuss.grapheneos.org/d/35428-recaptcha-mobile-verification-is-bringing-the-play-integrity-api-to-desktops
- **来源**：GrapheneOS Discussion Forum
- **主题**：reCAPTCHA Mobile Verification将硬件认证扩展到桌面系统

## 洞察摘要

### 核心观点

Apple和Google正在逐步扩大其硬件认证的使用范围，说服越来越多的服务采用硬件认证机制。Google的Play Integrity API和Apple的App Attest API非常相似。Apple已通过Privacy Pass将其带入网络，Google也打算效仿。

### Play Integrity API的影响

- **强认证要求**：Google的Play Integrity API要求"强完整性级别"必须使用硬件认证，并正在逐步对更常用的"设备完整性级别"也要求硬件认证
- **GrapheneOS被禁**：Google的Play Integrity API禁止使用GrapheneOS，尽管其安全性远高于Google允许的任何设备，也禁止使用任何其他替代操作系统
- **安全借口站不住脚**：Google允许10年未打安全补丁的设备，却不允许更安全的操作系统，这清楚地表明这不是真正的安全问题

### reCAPTCHA Mobile Verification

Google的reCAPTCHA计划采用以下方法：
- 在Apple设备上使用Privacy Pass
- 在Google移动服务Android设备上使用自己的方法
- 使用二维码扫描系统，要求Windows和其他系统用户使用iOS或认证的Android设备

当前的reCAPTCHA Mobile Verification可以在GrapheneOS的沙盒Google Play上运行，但它存在的目的是为在原本没有硬件认证的系统上开始使用硬件认证铺路。

### 反竞争问题

- Google对Android的认证要求包括强制捆绑Google Chrome等
- 控制reCAPTCHA使Google能够要求用户必须使用iOS或认证的Android设备才能访问大量网站
- 政府服务正越来越多地强制使用Apple App Attest和Google Play Integrity

### 欧盟的角色

欧盟正在带头要求数字支付、身份验证、年龄验证等使用这些认证系统。欧盟不是阻止Apple和Google的反竞争行为，而是通过自己的服务直接参与排斥竞争。

### 替代方案

统一认证（Unified Attestation）是由多家欧洲公司推动的另一个反竞争系统，同样会将人们锁定在特定的硬件和软件之外。

### 社区评论

- 有评论指出可以通过工作量证明（Proof of Work）等其他方法来提高请求成本，而不是硬件认证
- 有人对欧盟在反竞争方面的诉讼前景表示悲观
- 有人建议寻找Privacy Pass的替代方案，如Proton Captcha等开源替代品

## 结论

reCAPTCHA Mobile Verification不仅仅是一个验证码系统，它是Google和Apple将硬件认证扩展到网络和桌面平台的战略一部分。这将导致用户必须使用Apple或Google认证的设备才能访问大量网络服务，严重限制了硬件和操作系统的选择自由。