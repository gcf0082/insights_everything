# PWA功能支持洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://pwa.gripe/ |
| **数据来源** | caniuse.com |
| **更新日期** | 2026-03-23 |
| **对比平台** | Chrome 146 (Android) vs Mobile Safari 26.4 (iOS/iPadOS) |
| **主题** | Apple iOS Safari对PWA功能的限制 |

## 洞察摘要

pwa.gripe 是一个专门揭示Apple iOS和iPadOS上Safari浏览器对渐进式网页应用（PWA）功能限制的网站。该网站通过对比Android平台Chrome浏览器与iOS平台Safari浏览器的Web API支持情况，展示了Apple如何通过限制Safari的功能来维护App Store的商业利益，从而牺牲开放网络的体验。

## 核心发现

### 一、iOS Safari完全不支持的功能（12项）

| 功能 | Chrome (Android) | Mobile Safari | 说明 |
|------|-----------------|---------------|------|
| 协议处理 (Protocol Handling) | ✔ | ✘ | 无法注册自定义URL协议 |
| 文件处理 (File Handling) | ✔ | ✘ | 无法将文件关联到PWA |
| 联系人选择器 (Contact Picker) | ✘ | ✘ | 两平台均不支持 |
| 面部检测 (Face Detection) | ✔ | ✘ | 无法在网页上检测面部 |
| 振动 (Vibration) | ✔ | ✘ | 无法使用设备振动功能 |
| 元素捕获 (Element Capture) | ✔ | ✘ | 无法捕获特定网页区域 |
| 后台同步 (Background Sync) | ✔ | ✘ | 无法在后台同步数据 |
| 后台获取 (Background Fetch) | ✔ | ✘ | 无法后台预取内容 |
| 蓝牙 (Bluetooth) | ✔ | ✘ | 无法使用Web蓝牙 |
| NFC | ✔ | ✘ | 无法使用近场通信 |
| AR/VR | ⚠ | ✘ | iOS完全不支持增强现实 |
| 音频会话 (Audio Session) | ✘ | ✘ | 两平台均不支持 |

### 二、iOS Safari部分支持的功能（6项）

| 功能 | Chrome (Android) | Mobile Safari | 限制说明 |
|------|-----------------|---------------|----------|
| 通知 (Notifications) | ✔ | ⚠ | Safari支持但有限制 |
| Web推送 (Web Push) | ✔ | ⚠ | Safari支持但有限制 |
| 视图转换 (View Transitions) | ✔ | ⚠ | Safari支持但有限制 |
| 网络信息 (Network Info) | ✔ | ⚠ | Safari支持但有限制 |
| 设备方向 (Orientation) | ⚠ | ⚠ | 两者均部分支持 |
| 语音识别 (Speech Recognition) | ⚠ | ⚠ | 两者均部分支持 |

### 三、iOS Safari完全支持的功能（16项）

以下功能在两个平台上均得到良好支持：

- 离线支持 (Offline Support)
- 地理位置 (Geolocation)
- 媒体捕获 (Media Capture)
- 图片-in-图片 (Picture-in-Picture)
- 压缩流 (Compression Streams)
- 身份认证 (Authentication)
- Web分享 (Web Share)
- 虚拟键盘 (Virtual Keyboard)
- 条码检测 (Barcode Detection)
- 音频录制 (Audio Recording)
- 媒体会话 (Media Session)
- 存储 (Storage)
- 支付 (Payment)
- 唤醒锁 (Wake Lock)
- 运动传感器 (Motion)
- 语音合成 (Speech Synthesis)
- 多点触控 (Multi Touch)

## 结论

Apple iOS Safari对PWA功能的限制极为广泛，主要体现在：

1. **关键Web API缺失**：12项核心Web功能在iOS Safari上完全不可用
2. **后台能力受限**：后台同步、后台获取等PWA核心功能被禁用
3. **硬件交互受限**：蓝牙、NFC、振动等硬件功能无法使用
4. **用户体验差距**：与Android平台Chrome相比，iOS用户无法获得完整的PWA体验

这种策略被认为是为了保护App Store的商业利益，通过限制Web应用的能力迫使用户下载原生App。

---

*报告生成时间：2026-03-24*
*数据来源：caniuse.com*
