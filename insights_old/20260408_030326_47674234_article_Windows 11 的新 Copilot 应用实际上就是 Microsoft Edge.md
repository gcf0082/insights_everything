# 洞察报告：Windows 11 Copilot 应用实际上是微软 Edge 浏览器

## 基本信息

- **洞察链接**: https://twitter.com/TheBobPony/status/2041112541909205001
- **原始来源**: Twitter 用户 @TheBobPony (Bob Pony)
- **发布时间**: 2026年4月6日
- **相关讨论**: Hacker News (https://news.ycombinator.com/item?id=47674234)
- **报告生成日期**: 2026-04-08
- **标签**: Microsoft, Copilot, Windows 11, Edge, 浏览器

## 洞察摘要

Twitter 用户 TheBobPony 发现并揭示了一个令人惊讶的事实：**Windows 11 的新版 Copilot 应用实际上就是微软 Edge 浏览器的翻版**。这一发现引发了广泛的技术讨论和批评。

## 核心发现

### 技术细节

1. **本质上是 Edge 浏览器**
   - 将 `mscopilot.exe` 重命名为 `msedge.exe`
   - 将其文件夹从 "Copilot" 改为 "Edge"
   - 即可直接启动 Microsoft Edge 浏览器

2. **资源占用问题**
   - 该应用安装了一个完整的 Microsoft Edge 浏览器包
   - 占用约 850 MB 磁盘空间
   - 在大多数系统已经安装 Edge 的情况下，这是重复安装

3. **技术实现**
   - 使用 WebView2 而非传统的 Electron
   - WebView2 可以在多个应用间共享运行时，减少分发大小
   - 但 Copilot 似乎采用独立打包的方式

## 社区反应

### 批评观点

1. **资源浪费**：在用户已经安装 Edge 的情况下，重复安装 850 MB 的浏览器被认为是严重的资源浪费

2. **虚假宣传**：微软将此包装为"原生"AI 助手，但实际上只是一个网页应用

3. **功能限制**：用户报告 Copilot 在实际使用中存在诸多限制，例如：
   - Outlook 中的 Copilot 无法访问日历数据
   - 会议参加人数限制为 10 人
   - 与实际应用程序的集成非常有限

### 历史类比

有用户指出这类似于 DOS 时代的做法：`EDIT.COM` 调用 `QBASIC.EXE`。但不同之处在于，那时是为了节省磁盘空间，而现在的做法是浪费空间。

## 技术背景

- **WebView2**：微软自己的 Chromium 渲染引擎，比传统 Electron 更轻量
- **Electron**：Node.js + Chromium，部分评论认为 Copilot 的做法比 Electron 更糟糕
- **微软的 Chromium 策略**：由于上游 Chromium 更新，Edge 能够保持持续更新

## 相关报道

多家科技媒体对此进行了报道：

- TweakTown：新版 Copilot 是 Edge 的伪装
- Windows Central：微软的"原生"Copilot 仍然只是一个网站
- Windows Latest：Copilot 包含完整的 Edge 包，占用更多内存

## 结论

这一发现揭示了微软在 AI 产品推广中的一个有趣现象：将一个简单的网页应用包装成"原生应用"，实际上并没有为用户带来任何额外的价值，反而增加了系统资源负担。这与微软宣称的"原生 AI 助手"形象形成了鲜明对比。

---

*本报告基于公开信息来源整理，仅供参考。*