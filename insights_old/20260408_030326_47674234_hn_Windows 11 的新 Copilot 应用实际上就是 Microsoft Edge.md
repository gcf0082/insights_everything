# 洞察报告

- **洞察链接**: https://news.ycombinator.com/item?id=47674234
- **标题**: "The new Copilot app for Windows 11 is really just Microsoft Edge"
- **来源**: Hacker News
- **得分**: 101 points
- **发布者**: bundie
- **发布时间**: 6 hours ago
- **评论数**: 64 comments

---

## 摘要

Windows 11 新版 Copilot 应用本质上只是披着外壳的 Microsoft Edge。调查显示，该应用实际上使用了完整的 Edge 浏览器包（约 850 MB），而不是利用系统已有的 WebView2 组件，这引发了用户对微软产品策略的质疑。

## 主要发现

1. **Copilot 就是 Edge**: 新版 Windows 11 Copilot 应用实际上只是一个封装在独立应用中的 Microsoft Edge 浏览器。

2. **资源浪费**: 该应用在系统中安装了一个完整的 Edge 副本（约 850 MB），而大多数系统已经安装了 Edge，这种做法造成了磁盘空间的浪费。

3. **技术实现**: Microsoft 从 Electron 切换到了自己的 WebView2，但 Copilot 没有使用这种更高效的方案，而是直接绑定了整个 Edge 浏览器。

4. **企业环境问题**: 在企业 Mac 设备上，Copilot 登录时唯一的选择是一个指向网页版 Copilot 的超链接，无法使用 Entra ID 进行身份验证。

5. **功能局限性**: Copilot 的"连接体验"实际上只是一个聊天窗口，与嵌入的应用程序没有真正的集成。用户反映 Copilot in Excel 的功能甚至不如 Claude。

6. **API 限制**: 有用户发现 Outlook 中的 Copilot 只能访问会议的前 10 位参会者，这是一个人为设置的限制，严重影响了其实用性。

## 社区反应

- 这是微软"形式重于实质"的产品策略的体现
- 有人将其比作 MS-DOS 中 EDIT.COM 调用 QBASIC 的做法，但那时是为了节省磁盘空间
- 多个 Edge 副本的安装导致 Intune/Defender 在 MacOS 上标记为不合规
- 评论区对微软缺乏倾听用户反馈的做法表示困惑

## 结论

社区普遍认为微软的 Copilot 推送更多是一种营销行为而非真正的生产力提升。用户对微软持续推出功能受限、缺乏真正应用集成的 AI 产品感到失望。
