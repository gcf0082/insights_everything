# 移动设备漏洞管理概念 - 洞察报告

## 基本信息

- **洞察链接**: https://bmi.usercontent.opencode.de/eudi-wallet/wallet-development-documentation-public/latest/architecture-concept/06-mobile-devices/02-mdvm/
- **洞察时间**: 2026-04-05 15:06:28
- **文档来源**: German National EUDI Wallet: Architecture Documentation
- **文档类型**: 架构文档 - 移动设备漏洞管理概念

---

## 一、概述

本文档介绍了德国国家EUDI钱包架构中的移动设备漏洞管理（Mobile Device Vulnerability Management，MDVM）概念。该概念旨在确保用户设备的认证机制能够抵御具有高攻击潜力的攻击者，从而保证凭证的安全性。

### 1.1 背景与动机

在电子身份认证的高保证级别场景下，需要确保：

1. **凭证防复制**：认证手段能够防止攻击者复制凭证绑定密钥
2. **认证防滥用**：防止攻击者滥用用户的认证机制进行凭证演示

第一个保证可以通过使用经过认证的硬件安全模块（HSM）实现的RWSCD来独立实现。第二个保证则依赖于用户设备的安全性，包括：
- **持有因素**：由移动设备的硬件密钥存储（HKS）保护
- **知识因素**：通过移动设备输入

由于移动设备缺乏针对特定攻击潜力的漏洞分析和认证，MDVM通过在运营期间监控已识别的漏洞来降低被利用的风险。

### 1.2 MDVM的核心功能

| 功能 | 描述 |
|------|------|
| 验证设备/应用安全态势 | 提供设备完整性、真实性及钱包应用完整性验证信息 |
| 识别设备类别 | 提供设备型号、操作系统版本、补丁级别和HKS的验证信息 |
| 验证设备类别漏洞 | 提供设备操作系统和HKS的最新漏洞信息 |
| 决定设备/应用使用 | 基于安全态势和漏洞信息，决定是否允许使用RWSCD密钥 |

---

## 二、收集的信号

### 2.1 信号总览

| 信号源 | 威胁 | 协同 | 备注 |
|--------|------|------|------|
| KeyAttestation | rooting、引导加载程序解锁、应用重打包、设备模拟、重放攻击 | LPADB、RASP | 用作DCVDB输入 |
| PlayIntegrity | 应用重打包、重放攻击、rooting、凭证盗窃工具、overlay攻击 | KeyAttestation、RASP | Google专有后端判决 |
| iOS DC Device Check | 重放攻击、证书篡改、设备代理欺骗 | RASP | 需额外安全措施 |
| LPADB | 使用公开泄露的KeyAttestation密钥进行rooting | KeyAttestation | - |
| DCVDB | 因公开已知漏洞导致的不安全设备 | KeyAttestation、RASP | 设备类别识别至关重要 |
| RASP | 应用挂钩/调试/重打包、UD rooting和模拟 | - | 运行时动态监控 |

---

## 三、Android平台信号

### 3.1 Android KeyAttestation信号

| 信号 | 强制级别 | 详情 | 示例值 | 缓解的威胁 |
|------|----------|------|--------|------------|
| SecurityLevel | Hardware | 识别HKS类型 | TrustedEnvironment/StrongBox | 模拟攻击 |
| attestationIdModel | Hardware | 识别设备型号 | "SM-A146P" | 应用克隆 |
| attestationIdProduct | Hardware | 识别设备型号 | "a14xmeea" | 应用克隆 |
| attestationIdDevice | Hardware | 识别设备型号 | "a14xm" | 应用克隆 |
| osVersion | Hardware | 识别OS版本 | "130000" | 应用克隆、降级攻击 |
| osPatchLevel | Hardware | 识别安全补丁级别 | "202508" | - |
| RootOfTrust.deviceLocked | Hardware | 引导加载程序状态 | true/false | rooting |
| RootOfTrust.verifiedBootState | Hardware | 验证启动状态 | Verified | rooting、模拟攻击 |
| attestationChallenge | Hardware | 服务器提供的挑战 | 最大128字节 | 重放攻击 |
| AttestationPackageInfo.package_name | Software | 允许使用密钥的应用 | "org.sprind.wallet" | 应用重打包 |
| AttestationPackageInfo.version | Software | 应用版本代码 | "1" | 应用降级攻击 |
| signature_digests | Software | 应用签名证书的SHA-256摘要 | Base64编码 | 应用重打包 |

**注意**：
- "attestationIdModel"、"attestationIdProduct"和"attestationIdDevice"都可用于识别设备型号，包含全部三个字段以提高识别成功率
- 必须验证KeyAttestation签名和证书（包括完整证书链），并检查Google的吊销列表

### 3.2 Android PlayIntegrity Verdict信号

| 信号 | 提供者 | 详情 | 示例值 | 缓解的威胁 |
|------|--------|------|--------|------------|
| requestPackageName | OS | 包名 | "org.sprind.wallet" | 应用重打包 |
| requestNonce | Wallet Backend | 随机值 | Base64 16-32字节 | 重放攻击 |
| requestTimestamp | Google | 判决产生时间 | Unix ms时间戳 | 重放攻击 |
| appRecognitionVerdict | Google | 应用是否被识别为原始 | "PLAY_RECOGNIZED" | 应用重打包 |
| packageName | Google | 实际执行的包名 | "org.sprind.wallet" | 应用重打包 |
| certificateSha256Digest | Google | 签名证书摘要 | Base64编码 | 应用重打包 |
| versionCode | Google | 应用版本代码 | "1" | 应用降级攻击 |
| deviceRecognitionVerdict | Google | 设备信任级别 | "MEETS_STRONG_INTEGRITY" | rooting + Google专有MDVM判决 |
| sdkVersion | Google | Android SDK API级别 | "33" | 应用克隆 |
| deviceActivityLevel | Google | 每小时完整性令牌请求 | "LEVEL_1"-"LEVEL_4" | Bot流量、模拟器农场 |
| appLicensingVerdict | Google | 应用是否从PlayStore安装 | "LICENSED" | 侧载APK |
| appsDetected | Google | 检测到的其他应用 | "KNOWN_CAPTURING" | 凭证盗窃工具、overlay攻击 |
| playProtectVerdict | Google | Play Protect风险评估 | "NO_ISSUES"/"MEDIUM_RISK" | 已知恶意软件 |

**注意**：
- 最低Android版本为Android 13，检查"MEETS_STRONG_INTEGRITY"
- MEETS_STRONG_INTEGRITY要求设备在最近12个月内收到安全补丁

### 3.3 Android RASP（运行时应用自保护）

| 检测功能 | 详情 | 缓解的威胁 |
|----------|------|------------|
| 应用挂钩/调试 | 监控动态调试器附加、库注入、Frida/Xposed/LSPosed/DobbyHook等框架 | 逆向工程、动态运行时操作 |
| 应用重打包 | 检测应用包修改、重新签名、注入框架 | 应用重打包/篡改/欺骗 |
| 应用篡改 | 识别二进制补丁、代码段更改、关键逻辑修改 | 禁用保护、修补逻辑 |
| UD rooting | 检测rooting指标：沙箱违规、特权文件访问、工具链 | 权限提升、绕过完整性控制 |
| UD模拟 | 检测虚拟化/自动化/非真实硬件环境 | 自动化欺诈、Bot攻击 |

**注意**：
- RASP提供在应用运行时持续动态监控应用和设备完整性
- Root检测对Android尤为重要，因为存在公开的泄露KeyAttestation密钥方法

---

## 四、iOS平台信号

### 4.1 iOS DeviceCheck.AppAttest Attestation信号

| 信号 | 提供者 | 详情 | 示例值 | 缓解的威胁 |
|------|--------|------|--------|------------|
| attestationObject (x5c) | Secure Enclave + Apple服务器 | 证明App Attest密钥在Secure Enclave中生成 | CBOR结构 | 重放攻击、应用重打包、设备模拟、越狱 |
| credentialId | Secure Enclave | App Attest密钥的32字节标识符 | Base64字符串 | 密钥克隆、设备代理欺骗 |
| clientDataHash | Wallet Backend | 服务器挑战的SHA-256哈希 | 32字节摘要 | 重放攻击 |
| RP ID | OS + Secure Enclave | App ID前缀+BundleID | SHA-256(TeamID + "." + BundleID) | 应用重打包 |
| environment (aaguid) | OS + Secure Enclave | 环境指示 | "production" | 泄露的调试构建 |
| counter | Secure Enclave | 密钥使用次数 | "0" | - |

**注意**：
- iOS不提供设备型号或OS版本/补丁级别的硬件级信息，需在确保设备未被篡改后从OS查询

### 4.2 iOS DeviceCheck.AppAttest Assertions信号

| 信号 | 提供者 | 详情 | 示例值 | 缓解的威胁 |
|------|--------|------|--------|------------|
| assertionObject | Secure Enclave | 包含App Attest密钥签名和状态的CBOR对象 | CBOR结构 | 重放攻击 |
| keyId | Secure Enclave | 用于签名断言的App Attest密钥标识符 | Base64字符串 | 应用克隆 |
| clientDataHash | Wallet Backend | 挑战和可选载荷的SHA-256哈希 | 32字节摘要 | 重放攻击 |
| RP ID | OS + Secure Enclave | App ID前缀+BundleID | - | 应用重打包 |
| counter | Secure Enclave | 密钥使用次数 | "42" | 重放攻击、设备代理 |

**注意**：
- 计数器必须单调递增，大跳跃可能表示设备被用于代理其他应用/设备的认证

### 4.3 iOS RASP

| 检测功能 | 详情 | 缓解的威胁 |
|----------|------|------------|
| 应用挂钩/调试 | 监控动态调试器附加、库注入、Frida/Substrate等框架 | 逆向工程、动态运行时操作 |
| 应用重打包 | 检测应用包修改、重新签名、注入框架 | 应用重打包/篡改/欺骗 |
| 应用篡改 | 识别二进制补丁、代码段更改、关键逻辑修改 | 禁用保护、修补逻辑 |
| UD rooting | 检测越狱指标：沙箱违规、特权文件访问、工具链 | 权限提升、绕过完整性控制 |
| UD模拟 | 检测虚拟化/自动化/非真实硬件环境 | 自动化欺诈、Bot攻击 |

**注意**：
- Apple平台提供强大的安装时保护：App Sandbox、代码签名、App Store审查、系统完整性保护
- App Attest服务可断言应用真实性，但无法提供运行时挂钩或工具检测信息

---

## 五、总结

MDVM概念通过整合多种信号源（KeyAttestation、PlayIntegrity、Apple DeviceCheck、RASP）来实现设备安全态势的全面评估。主要特点包括：

1. **多层次验证**：结合硬件级KeyAttestation和应用级RASP进行多层验证
2. **平台特定方案**：Android和iOS采用不同的验证机制，但目标一致
3. **漏洞数据库**：维护设备类别漏洞数据库（DCVDB）以跟踪已知漏洞
4. **动态监控**：RASP提供运行时动态监控，弥补平台级验证的不足
5. **隐私考量**：在实现安全验证的同时需考虑用户隐私保护

该方案确保在存在已知漏洞的情况下，防止使用RWSCD密钥，从而维护凭证的安全性和可靠性。

---

*报告生成时间: 2026-04-05 15:06:28*
