# 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **链接** | https://news.ycombinator.com/item?id=47461321 |
| **标题** | Show HN: We built a terminal-only Bluesky / AT Proto client written in Fortran |
| **作者** | FormerLabFred |
| **得分** | 145 |
| **评论数** | 82 |
| **发布时间** | 3天前 |
| **GitHub** | https://github.com/FormerLab/fortransky |

## 项目概述

Fortransky 是一个终端专用的 Bluesky / AT Proto 客户端，完全使用 Fortran 编写。这是一个非常独特的选择，因为 Fortran 是一种诞生于1950年代的编程语言，通常用于科学计算和超级计算机领域。

### 技术栈

- **70% Fortran**：核心逻辑
- **Rust、C、Python**：辅助组件
- **Fortran 版本**：使用 Fortran 90（包含模块）
- **JSON 解析器**：手写的 Fortran 实现，带深度跟踪的键扫描器

### 功能特性

- 仅支持键盘导航
- `l+ENTER`：点赞帖子
- `n+ENTER`：查看通知
- 支持直接连接 Bluesky firehose 或通过 Jetstream
- 下一个版本计划添加图片支持，使用 Apple 1984 年的 Floyd-Steinberg 抖动算法

## 社区反馈分析

### 正面评价

1. **创新精神**：社区对使用"古老"语言构建现代应用表示赞赏
2. **实用性**：证明了不需要庞大技术栈也能构建有用的软件
3. **怀旧情结**：唤起了人们对早期编程经历的回忆

### 讨论焦点

#### 1. 为什么选择 Fortran？

作者表示：
- 现代语言建立在 Fortran 之上
- Fortran 非常快速
- 代码可移植性强，可能50年不变
- 循环执行效率高

#### 2. Fortran vs COBOL

- 作者同时在开发 Cobolsky（COBOL 版本）
- COBOL 更痛苦，Fortran 更好
- 如果 AT Proto 使用固定宽度记录而非 JSON，COBOL 会很强大

#### 3. 关于 AI 的使用

- 开发过程中曾使用 Claude 协助调试
- 主要问题是发送消息时 CPU 占用过高（循环300次尝试发送）
- 作者表示更喜欢手动编码

#### 4. 其他 AT Protocol 应用

社区还讨论了其他基于 AT Protocol 的应用：
- Tangled：GitHub 替代品
- dropb.at：Dropbox 替代品
- stream.place：直播服务
- leaflet.pub：博客平台

## 结论

Fortransky 是一个有趣的技术实验，展示了 Fortran 在现代应用开发中的潜力。项目获得了社区的积极响应，作者表示将继续开发并保持、开源精神，拒绝风险投资。