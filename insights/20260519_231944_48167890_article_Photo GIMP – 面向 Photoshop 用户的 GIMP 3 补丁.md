# PhotoGIMP 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://github.com/Diolinux/PhotoGIMP |
| **仓库名称** | PhotoGIMP |
| **作者** | Diolinux |
| **星标数** | 10.6k |
| **Fork数** | 366 |
| **监视数** | 114 |
| **许可证** | GPL-3.0 |
| **主要语言** | CSS |
| **最新版本** | 3.0 (发布于2025年3月17日) |
| **提交数** | 189次 |
| **项目简介** | 一个为GIMP 3+打造的Photoshop用户补丁 |

## 项目概述

PhotoGIMP是一个免费、社区驱动的补丁程序，将GIMP（GNU图像处理程序）转变为让Adobe Photoshop用户感到熟悉的界面布局。如果您正在从Photoshop切换到GIMP并希望立即获得家的感觉，PhotoGIMP正是为您准备的。

GIMP是一个免费且开源的图像编辑器，适用于Linux、macOS和Windows。它可以完成Photoshop的大部分功能——照片修图、图像合成、图形设计等，而且完全免费。PhotoGIMP只是让它看起来和感觉更像Photoshop。

## 核心功能

### 1. Photoshop风格工具布局
工具被重新组织，模拟Adobe Photoshop中的位置，让Photoshop用户能够轻松上手。

### 2. 自定义启动画面
独特的PhotoGIMP启动画面在启动时迎接用户。

### 3. 最大化画布空间
默认设置经过优化，为您提供最大的工作区域。

### 4. Photoshop键盘快捷键
键盘快捷键遵循Adobe官方文档中的Windows版本快捷键。

### 5. 自定义图标和名称
专用的.desktop文件为PhotoGIMP提供自己的图标和应用程序名称。

## 系统要求

- **GIMP 3.0或更高版本**：可从gimp.org或Flathub（Linux）下载
- **必须先运行GIMP一次**：GIMP需要先生成其配置文件，然后PhotoGIMP才能覆盖它们

## 安装方式

### Linux (Flatpak)
1. 确保已从Flathub安装GIMP
2. 打开并关闭GIMP一次
3. 下载最新版本：PhotoGIMP-linux.zip
4. 将.zip文件解压到主文件夹(~)
5. 打开GIMP查看新的PhotoGIMP布局

### Windows
1. 确保已安装GIMP（从官方网站）
2. 打开并关闭GIMP一次
3. 下载：PhotoGIMP.zip
4. 将3.0文件夹复制到%APPDATA%\GIMP
5. 打开GIMP查看新的PhotoGIMP布局
6. 还可选择下载photogimp.ico更改GIMP快捷方式图标
7. 支持通过Chocolatey安装：choco install photogimp

### macOS
1. 确保已安装GIMP
2. 打开并关闭GIMP一次
3. 下载：PhotoGIMP.zip
4. 将3.0文件夹复制到~/Library/Application Support/GIMP
5. 打开GIMP查看新的PhotoGIMP布局

## 补丁内容

PhotoGIMP替换或添加以下文件到GIMP的配置目录：

| 文件/文件夹 | 功能说明 |
|------------|----------|
| shortcutsrc | 键盘快捷键映射以匹配Photoshop |
| toolrc | 工具配置和排序 |
| sessionrc | 窗口布局和面板位置 |
| dockrc | 停靠/面板配置 |
| gimprc | 常规GIMP偏好设置（画布、网格等） |
| activerc | 活动工具/颜色上下文设置 |
| splashes/ | 自定义PhotoGIMP启动画面 |
| theme.css | 细微的UI主题调整 |
| templaterc | 预定义的画布模板 |

在Linux上，补丁还安装：
- 自定义.desktop文件（带有PhotoGIMP名称和图标的应用程序启动器）
- 自定义应用程序图标

## 卸载方式

### Linux
```bash
rm -rf ~/.config/GIMP/3.0
```
然后再次打开GIMP，它将创建全新的默认配置。

### Windows
1. 按Windows + R，输入%APPDATA%\GIMP并按Enter
2. 删除3.0文件夹
3. 打开GIMP，它将重新创建默认设置

### macOS
1. 打开Finder，按Cmd + Shift + G
2. 转到~/Library/Application Support/GIMP
3. 删除3.0文件夹
4. 打开GIMP，它将重新创建默认设置

## 常见问题解答

**PhotoGIMP没有改变任何东西——GIMP看起来一样**
- 确保将文件解压缩到正确的位置
- Linux：.config和.local文件夹必须在主目录(~)中
- Windows：3.0文件夹必须在%APPDATA%\GIMP内
- macOS：3.0文件夹必须在~/Library/Application Support/GIMP内

**安装PhotoGIMP后打开GIMP时出现错误**
- 这通常意味着GIMP版本不匹配。PhotoGIMP专为GIMP 3.0+构建。如果运行GIMP 2.x，将不兼容。

**可以与GIMP 2.10一起使用PhotoGIMP吗？**
- 不能。此版本的PhotoGIMP专门针对GIMP 3.0和更新版本设计。GIMP 2.x和3.x之间的配置格式发生了重大变化。

**PhotoGIMP会删除我的自定义画笔、字体或插件吗？**
- 不会。PhotoGIMP仅替换配置文件（快捷键、布局、偏好设置）。您的个人画笔、字体、渐变和插件保持不变。

## 贡献与翻译

该项目欢迎贡献：
- 报告问题：提交issue
- 提交修复：创建pull request
- 翻译：帮助将README翻译成更多语言

当前已有的翻译版本：
- 意大利语 (Italiano)
- 波兰语 (Polski)
- 巴西葡萄牙语 (Português)
- 俄语 (Русский)

## 鸣谢

- 感谢GIMP团队
- 感谢所有Diolinux的YouTube支持者
- 启动画面和图标来自Adriel Filipe Design

## 许可证

PhotoGIMP根据GNU通用公共许可证v3.0获得许可。