# WiiFin 项目洞察报告

## 基本信息

- **洞察链接**: https://github.com/fabienmillet/WiiFin
- **项目名称**: WiiFin
- **项目描述**: Nintendo Wii 的 Jellyfin 客户端
- **星标数**: 87
- **语言**: C (78.2%), C++ (21.2%)
- **许可证**: GPLv3
- **最新版本**: v0.1.1 (2026年4月12日)
- **提交数**: 51

## 项目概述

WiiFin 是一个实验性的 homebrew 应用，专门为 Nintendo Wii 游戏机开发，用于连接 Jellyfin 媒体服务器。它使用 C++ 编写，结合了 GRRLIB 图形库和 MPlayer CE 播放器，为 Wii 用户提供轻量级的媒体浏览和播放体验。

## 功能特性

### ✅ 已实现功能

1. **认证系统**
   - 用户名/密码登录
   - QuickConnect 快速连接（可在其他设备批准）

2. **用户配置**
   - 多账户存储（仅存储访问令牌，不存储密码）

3. **媒体库浏览**
   - 电影、电视节目、音乐库
   - 封面图片加载

4. **详情页面**
   - 简介、评分、类型、演员、导演
   - 音频/字幕轨道选择

5. **内容推荐**
   - "继续观看"行
   - "下一个"推荐

6. **电视播放**
   - 剧集和分集导航

7. **视频播放**
   - 服务器端转码流式传输
   - MPlayer CE 引擎集成

8. **音乐播放**
   - 音频库、专辑/曲目导航

9. **播放器界面**
   - 进度条、音量控制
   - 上一集/下一集切换
   - 音频和字幕轨道切换
   - 跳过大纲功能

10. **播放报告**
    - 进度信息发送回 Jellyfin 服务器

11. **网络支持**
    - HTTPS 连接（通过 mbedTLS）
    - 支持自签名证书

12. **输入设备**
    - Wiimote 红外指针
    - 虚拟屏幕键盘

13. **其他功能**
    - 菜单背景音乐
    - 提供 .dol 和 .wad 格式分发

### ⚠️ 已知限制

1. 不支持直接播放，所有视频需服务器转码
2. 不支持 5.1 多声道音频（仅支持立体声转码）
3. 字幕依赖服务器嵌入到视频流中

## 项目结构

```
WiiFin/
├── source/
│   ├── core/        # 应用生命周期、背景音乐、工具函数
│   ├── input        # Wiimote 和 USB 键盘输入
│   ├── jellyfin     # Jellyfin HTTP API 客户端（HTTPS 通过 mbedTLS）
│   ├── player       # MPlayer CE 集成、播放器界面
│   └── ui           # 所有视图：连接、库、配置、设置
├── data/            # PNG/TTF 图形资源
├── libs/            # 捆绑的 mbedTLS
├── tools/           # WAD 打包器、图标生成器
├── Makefile         # devkitPro 兼容的构建脚本
└── apps/WiiFin/     # Homebrew Channel 元数据
```

## 技术栈

- **编程语言**: C, C++
- **图形库**: GRRLIB, libpngu, freetype, libjpeg
- **媒体播放器**: MPlayer CE
- **加密库**: mbedTLS
- **开发工具**: devkitPro (devkitPPC, libogc, wii-dev)

## 路线图

1. 排序/筛选功能（按年份、类型、评分）
2. 从 Wii 标记收藏
3. 多个 UI 配色主题

## 构建说明

### 环境要求
- devkitPro (含 devkitPPC, libogc, wii-dev)
- GRRLIB, libpngu, freetype, libjpeg
- mbedTLS（自动跨编译）
- 可选：编译为 libmplayer.a 的 MPlayer CE（用于视频播放）

### 构建命令
```bash
./build.sh
```

### 运行方式
- **Dolphin 模拟器**: `dolphin-emu -e WiiFin.dol`
- **真机**: 将 WiiFin.dol 复制到 `SD:/apps/WiiFin/boot.dol`，或使用 WAD 管理器安装 WiiFin.wad

## 总结

WiiFin 是一个专为 Nintendo Wii 设计的 Jellyfin 客户端，虽然目前仍处于实验阶段，但已实现了大部分核心功能。该项目为 Wii 用户提供了一种在老旧硬件上访问现代媒体服务器的方式，对于 homebrew 开发者和怀旧游戏爱好者具有较高的参考价值。项目采用 GPLv3 许可证，代码结构清晰，适合学习 Wii homebrew 开发。