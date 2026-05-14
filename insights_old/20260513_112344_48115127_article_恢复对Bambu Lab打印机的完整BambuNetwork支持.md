# GitHub 仓库洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **仓库链接** | https://github.com/FULU-Foundation/OrcaSlicer-bambulab |
| **仓库名称** | FULU-Foundation/OrcaSlicer-bambulab |
| **描述** | OrcaSlicer 的一个版本，恢复了对 Bambu Lab 打印机的完整 BambuNetwork 支持 |
| **星标数** | 1.3k |
| **观看者数** | 35 |
| **Fork 数** | 342 |
| **许可证** | AGPL-3.0 |
| **最新版本** | v1.0.0 (2026年5月12日) |
| **主要语言** | C++ (82.5%), C (9.9%), JavaScript (4.0%), HTML (1.7%) |

## 项目概述

OrcaSlicer-bambulab 是 OrcaSlicer 的一个特殊分支版本，主要特点是恢复了对 Bambu Lab 打印机的完整 BambuNetwork 支持。该版本允许用户不仅限于局域网使用，还能通过互联网远程连接和控制 Bambu Lab 打印机，实现完整的打印功能。

## 核心功能

1. **BambuNetwork 完整支持** - 支持通过互联网远程访问 Bambu Lab 打印机
2. **跨平台支持** - 支持 Windows (需要 WSL 2)、Linux 和 macOS
3. **开源免费** - 采用 AGPL-3.0 开源许可证

## 技术栈

- **主要语言**：C++ (82.5%)
- **辅助语言**：C (9.9%), JavaScript (4.0%), HTML (1.7%), CMake (0.7%), Shell (0.2%)
- **构建系统**：CMake
- **目标平台**：Windows、Linux、macOS

## 安装说明

### Windows
需要启用 WSL 2，运行以下命令后重启系统：
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

### Linux
正常安装即可

### macOS
开发中

## 相关资源

- BMCU 固件可在作者的其他仓库中找到
- 官方 Logo 位于 resources/images/OrcaSlicer.png

## 总结

OrcaSlicer-bambulab 为 Bambu Lab 用户提供了一个重要的替代选择，特别是那些需要远程打印功能的用户。凭借 1.3k 星标和 342 Fork 的社区认可，该项目在 3D 打印社区中具有一定的影响力。