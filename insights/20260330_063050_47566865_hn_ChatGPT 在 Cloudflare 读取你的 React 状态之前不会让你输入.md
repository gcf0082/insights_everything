# Hacker News 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://news.ycombinator.com/item?id=47566865 |
| **标题** | ChatGPT Won't Let You Type Until Cloudflare Reads Your React State |
| **来源** | https://www.buchodi.com/chatgpt-wont-let-you-type-until-cloudflare-reads-your-react-state-i-decrypted-the-program-that-does-it/ |
| **作者** | alberto-m |
| **得分** | 187 points |
| **评论数** | 126 comments |
| **发布时间** | 2小时前 |

---

## 洞察摘要

本文揭示了ChatGPT网页版如何使用Cloudflare和React状态检测来阻止用户输入，直到完成客户端检查。这是一种反机器人保护机制，但同时也给合法用户带来了使用障碍。

### 核心要点

**1. OpenAI官方回应**
- OpenAI Integrity团队的Nick解释称，这些检查是为了保护产品免受 bots、爬虫、欺诈等滥用
- 目的是确保有限的GPU资源流向真实用户
- 登录用户也会受到这些保护，因为登录只是一个信号，不能完全防止自动化攻击

**2. 用户体验问题**
- 长对话时ChatGPT界面性能极差，输入延迟可达数秒
- 使用VPN（如Mullvad）的用户经常遇到界面问题或超时错误
- 即便是付费Pro订阅用户，使用VPN也会遇到麻烦

**3. 隐私与功能的矛盾**
- 用户需要至少两个浏览器：一个用于允许各种客户端检查以使用关键服务，另一个用于保护隐私
- Firefox用户配合隐私保护设置更容易被标记为"可疑"
- CGNAT用户因邻居可能运行僵尸网络而频繁遭遇验证码

**4. 社区批评**
- 讽刺的是，强调"反爬虫"的OpenAI本身却在大量爬取互联网数据
- Cloudflare的检测机制对正常用户造成了比 bots 更大的麻烦
- 很多人选择离开那些过度使用Cloudflare的网站

**5. 技术细节**
- Cloudflare使用类似SVM的机器学习模型来检测 bots
- User-Agent是其中之一的影响因素
- 遵守隐私的浏览器（如Firefox）和VPN用户更容易被拦截

---

## 热门评论精选

> "我们真的需要人类证明吗？我们不需要Worldcoin，不需要证件。我们不需要。'证明你的人性/年龄或其他属性'这种机制会迅速走向你不想让它去的地方。" — ctoth

> "现在的情况是，用户至少需要两个浏览器。一个用于允许这些糟糕的客户端检查以便使用关键服务，另一个用于防止被追踪。" — everdrive

> "这太荒谬了，Cloudflare让使用他们认为是'可疑'的浏览器或IP地址的网页变得无法使用。我最近因为使用Firefox而陷入大量的验证码中。" — lxgr

---

## 相关讨论

- 有人提到可以使用Firefox的多容器功能来分离不同用途的浏览会话
- Mullvad VPN被多次推荐为注重隐私的VPN服务
- 有人建议用VSCode编写内容然后粘贴到ChatGPT来规避输入延迟问题
