# 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://www.buchodi.com/chatgpt-wont-let-you-type-until-cloudflare-reads-your-react-state-i-decrypted-the-program-that-does-it/ |
| **来源** | Buchodi's Threat Intel |
| **发布日期** | 2026年3月29日 |
| **作者** | Jamie Larson |

---

## 摘要

本文揭示了ChatGPT每次发送消息时都会触发Cloudflare Turnstile程序在浏览器中静默运行。作者通过解密377个这类程序，发现其功能远超标准的浏览器指纹识别。

该程序检查55个属性，涵盖三个层面：浏览器层（GPU、屏幕、字体）、Cloudflare网络层（城市、IP、地区）以及ChatGPT React应用本身（`__reactRouterContext`、`loaderData`、`clientBootstrap`）。Turnstile不仅验证是否使用真实浏览器，还验证是否运行了完整启动的特定React应用。

---

## 关键发现

### 加密机制

- Turnstile字节码以加密形式到达，服务器在准备响应中发送`turnstile.dx`字段（28,000个字符的base64编码，每次请求都会变化）
- 外层使用准备请求中的`p`令牌进行XOR解密
- 内层使用不同的XOR密钥（服务器生成的浮点数97.35），嵌入在字节码指令中
- 解密链只需要HTTP请求和响应，无需特殊权限

### 检查的55个属性

**第一层：浏览器指纹**
- WebGL（8个属性）：`UNMASKED_VENDOR_WEBGL`、`UNMASKED_RENDERER_WEBGL`等
- 屏幕（8个属性）：`colorDepth`、`pixelDepth`、`width`、`height`等
- 硬件（5个属性）：`hardwareConcurrency`、`deviceMemory`等
- 字体测量（4个属性）
- DOM探测（8个属性）
- 存储（5个属性）

**第二层：Cloudflare网络**
- 边缘头部（5个属性）：`cfIpCity`、`cfIpLatitude`、`cfIpLongitude`等

**第三层：应用状态**
- React内部（3个属性）：`__reactRouterContext`、`loaderData`、`clientBootstrap`

这是应用层的机器人检测，而非浏览器层。

### 其他组件

**Signal Orchestrator**（271条指令）：安装事件监听器监控36个属性，包括按键计时、鼠标速度、滚动模式等行为生物特征。

**Proof of Work**：25字段指纹+SHA-256哈希现金机制，72%在5ms内解决。

---

## 关键数据

| 指标 | 数值 |
|------|------|
| 解密程序数 | 377/377（100%） |
| 观察到的唯一用户 | 32 |
| 每个程序的属性数 | 55（所有样本一致） |
| 每个程序的指令数 | 417-580（平均480） |
| 唯一XOR密钥（50样本） | 41 |

---

## 结论

XOR密钥是服务器生成的浮点数，嵌入在发送的字节码中。"加密"只是用同一数据流中的密钥进行XOR，防止了随意检查，但无法防止分析。

该程序的核心创新在于不仅检测浏览器真实性，还验证React应用是否完整渲染和注水，使得绕过检测变得极其困难。

---

*本报告基于公开信息整理，仅供研究参考。*
