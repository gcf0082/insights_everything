# 洞察报告：Fortran 编写的终端 Bluesky/AT Proto 客户端

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://news.ycombinator.com/item?id=47461321 |
| **标题** | Show HN: We built a terminal-only Bluesky / AT Proto client written in Fortran |
| **作者** | FormerLabFred |
| **得分** | 145 |
| **评论数** | 82 |
| **发布时间** | 3天前 |
| **GitHub** | https://github.com/FormerLab/fortransky |

## 项目概述

FortranSky 是一个纯终端运行的 Bluesky / AT Proto 客户端，使用 Fortran 语言编写。没错，就是那个 Fortran——上世纪50年代诞生的编程语言。

项目采用键盘导航操作方式：
- `l+ENTER` 点赞帖子
- `n+ENTER` 查看通知

技术栈分布：70% Fortran，其余为 Rust、C 和少量 Python。

## 讨论热点

### 1. 为什么选择 Fortran？

作者表示：
- Fortran 语言直观简单，循环执行速度快
- 代码可移植性强，预计可正常工作50年不需修改
- 现代语言都建立在 Fortran 之上，理应传承这门语言

### 2. COBOLsky 即将推出

团队同时在开发 COBOLsky（COBOL 版本），作者认为 COBOL 写这类应用更痛苦，Fortran 更好。如果 AT Proto 使用固定宽度记录而非 JSON，COBOL 会很强。

### 3. AI 在开发中的作用

团队在开发过程中尝试使用 Claude AI，但未能解决主要遇到的 bug（CPU 100% 问题，最终发现是代码尝试发送消息300次导致）。Claude 主要被用于编写 README。

### 4. 未来规划

- 图片合成与解码器（计划使用 1984 年 Apple 的 Floyd-Steinberg 抖动算法）
- Fortran 驱动的快速 feed 构建器
- x86-64 汇编解码器用于 AT Proto firehose 帧解析

## 社区反应

总体反馈非常积极，社区认为这是"很酷且有趣"的项目，证明使用所谓"古老"语言也能写出有用的软件。有人评论："Fortran 会比蟑螂活得更久，即使世界404了"。

## 总结

FortranSky 是一个有趣的实验性项目，展现了老派编程语言在现代社交网络应用中的可能性。项目获得了社区的高度认可，证明了不必使用庞大技术栈也能交付有用的产品。