# 洞察报告：printervention 项目

**洞察链接**：https://printervention.app/details  
**来源**：printervention: the backstory  
**作者**：George MacKerron  
**日期**：2026年4月

---

## 项目概述

printervention 是一个让旧照片打印机重获新生的 Web 应用，通过浏览器直接连接打印机进行打印，无需安装任何软件。

## 核心问题

- 旧款 Canon SELPHY 照片打印机已被 Mac 和 Windows 放弃支持
- 这些打印机在 eBay 上价格低廉，但普通用户无法使用
- 作者希望让每个人都能使用这些打印机，"拯救百万打印机免于填埋"

## 技术方案

### 架构设计

1. **浏览器端虚拟化**：使用 v86 在浏览器中运行完整的 x86 虚拟机
2. **Alpine Linux 环境**：虚拟机运行 Alpine Linux + CUPS + Gutenprint
3. **WebUSB 连接**：浏览器通过 WebUSB 直接与物理打印机通信
4. **双向数据桥接**：使用 USB/IP 和 tcpip.js 实现虚拟机与浏览器之间的双向 USB 通信

### 关键技术实现

- **驱动程序匹配**：使用三 grams 算法匹配 Gutenprint 驱动程序
- **PDF 嵌入**：将 JPEG 嵌入 PDF 以解决 CUPS 纸张尺寸问题
- **HEIC 转换**：使用 libheif-js 和 wasm-mozjpeg 处理苹果 HEIC 格式
- **ICC 色彩配置**：确保色彩从 JPEG 正确传递到 PDF

## 解决的关键问题

1. **单向数据流问题**：最初只能发送数据到打印机，无法接收状态信息。通过 USB/IP + tcpip.js 实现了双向通信，现在 CUPS 可以完全了解打印机状态。

2. **照片比例问题**：CUPS 会压缩照片适应纸张，通过手写 PDF 包装器解决。

3. **苹果格式支持**：支持 Apple Photos 和 AirDrop 带来的 HEIC 格式。

## 未来计划

- 支持更多 Gutenprint 支持的打印机型号
- 添加 brlaser 和 splix 驱动包
- 推出扫描功能（yes-we-scan.app）
- 考虑商业化：希望打印机耗材公司付费白标

---

*报告生成时间：2026年4月8日*
