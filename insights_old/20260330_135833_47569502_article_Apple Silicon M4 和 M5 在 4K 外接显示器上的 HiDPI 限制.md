# 洞察报告：Apple Silicon M4/M5 HiDPI 限制问题

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://smcleod.net/2026/03/new-apple-silicon-m4-m5-hidpi-limitation-on-4k-external-displays/ |
| **发布日期** | 2026年3月29日 |
| **作者** | Sam McLeod |
| **原文标题** | New Apple Silicon M4 & M5 HiDPI Limitation on 4K External Displays |
| **标签** | Apple, 4K, Monitor, Software, Hardware, Bugs |

---

## 洞察摘要

从M4开始（包括全新的M5系列芯片），macOS不再为外接4K显示器提供或允许全分辨率HiDPI模式。

**核心问题**：在3840x2160面板上，现在可用的最大HiDPI模式仅为3360x1890（背衬存储为6720x3780，而非7680x4320）。M2/M3机型没有此限制。

**用户面临的选择**：
- 使用全屏幕工作空间4K（3840x2160），但文字模糊（HiDPI被禁用）
- 使用减少的屏幕工作空间3.3K（3360x1890），文字清晰（HiDPI），但可用工作空间显著减少，且macOS UI看起来过大

**这不是硬件限制**：DCP（显示协处理器）在M2 Max和M5 Max上报告相同显示的能力相同。M5 Max硬件支持8K（7680x4320）@60Hz。软件栈中某处（DCP与WindowServer之间）拒绝生成该模式。

---

## 技术分析

### 测试环境

| 属性 | M5 Max（受影响） | M2 Max（正常） |
|------|------------------|----------------|
| 芯片 | Apple M5 Max | Apple M2 Max |
| 型号ID | Mac17,6 | Mac14,6 |
| GPU核心 | 40 | 38 |
| macOS | 26.4 (25E246) | 26.4 (25E246) |
| 显示器 | LG HDR 4K 32UN880 | LG HDR 4K 32UN880 |
| 原生分辨率 | 3840x2160 | 3840x2160 |
| 连接方式 | USB-C/Thunderbolt, HBR3, 4通道 | USB-C/Thunderbolt, HBR3, 4通道 |
| 最大HiDPI模式 | **3360x1890** | **3840x2160** |

两台机器报告的DCP参数完全相同：
- MaxActivePixelRate = 497,664,000
- MaxTotalPixelRate = 537,600,000
- MaxW = 3840
- MaxH = 2160
- MaxBpc = 10

### 诊断尝试（均未成功）

1. **显示覆盖Plist（scale-resolutions）**：在M5 Max上无效，M2 Max上相同plist可正常工作
2. **EDID修补（软件覆盖）**：无效果，但BetterDisplay开发者已确认可在M4上通过欺骗更高原生分辨率来生成8K帧缓冲区
3. **刷新显示器EEPROM**：失败，LG 32UN880的EEPROM可能是只读的
4. **IOKit注册表覆盖**：DCP驱动程序拒绝用户空间属性写入（kern_return=-536870201）
5. **显示重新探测**：无效果
6. **WindowServer缓存清除**：无效果
7. **减少连接显示器数量**：无效果

### 问题根源

根据BetterDisplay开发者waydabber的描述：

> "通常情况下，由于系统资源分配的新动态特性，M4代Mac在任何非8K显示器上都无法使用3840x2160 HiDPI。可能存在例外情况——当系统判断没有其他显示器可以连接且仍有资源用于更高分辨率帧缓冲区时。但通常系统会分配尽可能小的帧缓冲区大小，预留空间给可能连接的其他显示器。"

可观察到的事实：
- DCP在M2 Max和M5 Max上报告相同的能力参数
- M5 Max上WindowServer的模式列表最高为3360x1890 HiDPI（背衬存储6720x3780），而非3840x2160 HiDPI（背衬存储7680x4320）
- 断开其他显示器不会改变这一点
- M4/M5上的缩放分辨率模式来自系统认为的显示器原生分辨率
- M2/M3上会生成高达原生分辨率2.0倍的HiDPI模式
- M4/M5上最高约为1.75倍

---

## 可能的解决方案

这需要Apple进行更改——要么修改模式生成逻辑中的缩放比例限制，要么提供用户可用的覆盖选项。

作者已向Apple提交反馈：FB22365722

5K或8K显示器可能不会达到相同的限制，因为其EDID原生分辨率足够高，1.75倍缩放仍可提供可用的背衬存储。

---

## 结论

这是一个影响M4和M5代Apple Silicon芯片用户的外接4K显示器HiDPI限制问题。该问题不是硬件限制，而是软件层面的限制，可能是Apple为了节省资源而引入的新动态资源分配策略所致。目前没有已知的用户可用的临时解决方案。
