# WhatCable 项目洞察报告

## 基础信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://github.com/darrylmorley/whatcable |
| **项目名称** | WhatCable |
| **作者** | Darryl Morley |
| **编程语言** | Swift (95.2%), Shell (4.8%) |
| **许可证** | MIT |
| **星标数** | 267 |
| ** forks 数** | 8 |
| **最新版本** | v0.4.7 (2026年5月1日) |
| **主题标签** | macOS, Swift, menu bar, USB-C, Thunderbolt, IOKit, SwiftUI |

## 项目概述

WhatCable 是一款 macOS 菜单栏应用，旨在以通俗易懂的英语告诉用户连接到 Mac 的每根 USB-C 线缆的实际功能，以及为什么 Mac 可能会充电缓慢。

## 核心功能

### 1. 端口状态显示
每个端口以简洁的标题形式显示：
- Thunderbolt / USB4
- USB 设备
- 仅充电
- 慢速 USB / 仅充电线缆
- 未连接

### 2. 充电诊断
当有线缆连接时，应用会显示瓶颈原因：
- "Cable is limiting charging speed"（线缆额定功率低于充电器）
- "Charging at 30W (charger can do up to 96W)"（Mac 请求功率较低，如电池接近充满）
- "Charging well at 96W"（一切匹配良好）

### 3. 线缆 e-marker 信息
显示线缆的实际：
- 速度：USB 2.0、5/10/20/40/80 Gbps
- 电流额定值：3A/5A（对应 60W/100W/240W）
- 芯片供应商

### 4. 充电器 PDO 列表
显示充电器支持的每个电压配置（5V/9V/12V/15V/20V 等），并实时高亮当前协商的配置文件。

### 5. 连接设备识别
从 PD Discover Identity 响应中解码供应商名称和产品类型。

### 6. 活动传输协议
显示 USB 2/USB 3/Thunderbolt/DisplayPort 等协议。

## 技术实现

### IOKit 服务
WhatCable 读取三个 IOKit 服务系列：
1. `AppleHPMInterfaceType10/11/12`（M3 系列）和 `AppleTCControllerType10`（M1/M2）- 端口状态、连接、传输、插头方向、e-marker 存在
2. `IOPortFeaturePowerSource` - 连接的电源的完整 PDO 列表及实时"获胜"PDO
3. `IOPortTransportComponentCCUSBPDSOP` - SOP（端口伙伴）和 SOP'（线缆 e-marker）的 PD Discover Identity VDO

### 技术特点
- 无权限要求
- 无私有 API
- 无辅助守护进程
- 使用 SwiftUI 构建界面

## 系统要求

- **macOS 14 (Sonoma) 或更高版本**
- 通用二进制（Apple Silicon + Intel）
- 已签名并经过公证

## 安装方式

从 Releases 页面下载最新的 `WhatCable.zip`，解压后将应用拖入 `/Applications` 目录。

## 构建要求

- Swift 5.9 (Xcode 15+)
- 执行 `swift run WhatCable` 运行

## 使用限制

1. **e-marker 信息仅显示于带有芯片的线缆** - 大多数 60W 以下的 USB-C 线缆无标记
2. **应用信任 e-marker** - 假冒或刷错的线缆可能虚假宣传性能，软件无法验证
3. **PD 规范覆盖** - 解码器针对 PD 3.0/3.1，PD 3.2 EPR 变体可能需要调整
4. **仅限 macOS** - iOS 沙盒使 USB-C e-marker 访问更加困难
5. **未在 App Store 上架** - App Sandbox 阻止了依赖的 IOKit 读取

## 代码结构

主要源文件入口：
- `Sources/WhatCable/ContentView.swift` - UI 入口
- `Sources/WhatCable/PortSummary.swift` - 通俗英语逻辑
- `Sources/WhatCable/PDVDO.swift` - 位操作解码

## 价值分析

WhatCable 解决了用户在实际使用中的真实痛点：USB-C 接口外观相同但功能差异巨大，从仅能充电的 USB 2.0 线缆到 240W/40 Gbps 的 Thunderbolt 4 线缆，用户往往不清楚线缆的实际能力。该应用通过读取系统底层 IOKit 信息，以用户友好的方式展示，帮助用户诊断充电慢等问题的根本原因。

## 总结

WhatCable 是一款设计精巧的 macOS 实用工具，将复杂的技术信息转化为直观的用户体验。它展示了如何利用系统底层 API（IOKit）创建有价值的实用应用，同时保持界面的简洁易用。对于经常使用 USB-C 设备的 Mac 用户来说，这是一个非常实用的工具。