# 洞察报告

**链接**: https://news.ycombinator.com/item?id=48167890
**来源**: Hacker News
**发布日期**: 2026-05-17
**提交者**: SockThief

---

## 摘要

本项目是PhotoGIMP，一个为GIMP 3+设计的免费社区化补丁，旨在将GIMP（GNU图像处理程序）转变为Photoshop用户熟悉的界面布局。

## 项目详情

**项目名称**: PhotoGIMP
**GitHub地址**: https://github.com/Diolinux/PhotoGIMP
**星标数**: 10.6k
**Fork数**: 366
**许可证**: GPL-3.0

### 主要功能

- **Photoshop风格的工具布局** - 工具位置重新排列，模仿Adobe Photoshop中的位置
- **自定义启动画面** - 独特的PhotoGIMP启动画面
- **最大化的画布空间** - 默认设置优化以提供最大的工作区域
- **Photoshop键盘快捷键** - 键盘快捷键遵循Adobe官方文档（Windows版本）
- **自定义图标和名称** - 专用的.desktop文件为PhotoGIMP提供自己的图标和应用程序名称

### 支持平台

- Linux (Flatpak或其他包管理器)
- Windows
- macOS

### 系统要求

- GIMP 3.0或更高版本
- 需要先运行一次GIMP以生成配置文件

### 安装方式

**Linux (Flatpak)**:
1. 从Flathub安装GIMP
2. 打开并关闭GIMP一次
3. 下载PhotoGIMP for Linux并解压到主目录

**Windows**:
1. 从官网安装GIMP
2. 打开并关闭GIMP一次
3. 下载PhotoGIMP for Windows并解压到%APPDATA%\GIMP

**macOS**:
1. 从官网安装GIMP
2. 打开并关闭GIMP一次
3. 下载PhotoGIMP并复制到~/Library/Application Support/GIMP

### 项目内容

补丁包含以下配置文件：
- `shortcutsrc` - 键盘快捷键映射
- `toolrc` - 工具配置和排序
- `sessionrc` - 窗口布局和面板位置
- `dockrc` - 停靠/面板配置
- `gimprc` - 通用GIMP首选项
- `contextrc` - 活动工具/颜色上下文设置
- `splashes/` - 自定义启动画面
- `theme.css` - UI主题调整
- `templaterc` - 预定义的画布模板

### 卸载说明

只需删除GIMP的配置文件夹并重新打开GIMP，它将重新生成默认设置。

## 总结

PhotoGIMP是一个实用的工具，帮助从Photoshop迁移到GIMP的用户减少学习成本，通过提供熟悉的界面布局和快捷键。它是免费开源的，获得了大量社区支持（10.6k星标）。