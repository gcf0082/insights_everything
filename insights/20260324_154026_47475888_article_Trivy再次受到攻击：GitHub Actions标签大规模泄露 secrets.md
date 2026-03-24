# Trivy GitHub Actions 供应链攻击分析报告

**洞察链接**: https://socket.dev/blog/trivy-under-attack-again-github-actions-compromise

**发布日期**: 2026年3月20日

**来源**: Socket安全研究团队

---

## 事件概述

2026年3月，Trivy生态系统遭受了第二次供应链攻击。此次攻击针对Trivy的官方GitHub Actions（aquasecurity/trivy-action），攻击者通过强制更新75个版本标签来分发恶意代码，窃取CI/CD环境中的敏感凭证。这是继本月初OpenVSX上Trivy VS Code扩展被入侵后的又一起安全事件。

---

## 攻击影响范围

- **受影响的仓库**: aquasecurity/trivy-action（官方GitHub Action）
- **受污染的标签数量**: 75个（共76个版本标签，仅0.35.0幸免）
- **潜在影响**: GitHub上超过10,000个工作流文件引用了该Action
- **首次检测时间**: 约2026年3月19日19:15 UTC
- **唯一幸免标签**: @0.35.0

---

## 攻击技术分析

### 攻击手法

攻击者并未通过推送分支或创建新发布来入侵，而是采用了一种精细的标签欺骗技术：

1. **获取凭证**: 攻击者通过早期入侵（3月初的CI环境凭证泄露）获得仓库的写权限
2. **创建恶意提交**: 从master分支最新提交（57a97c7e）的文件树开始，仅替换entrypoint.sh为恶意代码
3. **伪造元数据**: 复制原标签指向的原始提交的作者信息、时间戳和提交消息
4. **强制推送**: 将75个现有标签强制更新为指向新的恶意提交
5. **发布"不可变"版本**: 攻击者甚至利用GitHub的Immutable Releases功能锁定恶意状态

### 识别特征

以下迹象可识别被污染的标签：
- 提交无GPG签名（原始提交由GitHub Web UI签名）
- 提交日期显示2021/2022年，但父提交日期为2026年3月
- 每个恶意提交仅修改entrypoint.sh，而原提交通常修改多个文件
- 发布页面显示"0 commits to master since this release"

---

## 恶意载荷分析

### 载荷结构

恶意entrypoint.sh共204行，前102行为恶意代码，后102行为合法的Trivy扫描代码。由于恶意代码先执行，用户可能不会注意到任何异常。

### 三阶段攻击

#### 第一阶段：凭证收集

**进程环境变量扫描**（第4-29行）:
- 查找GitHub Actions Runner进程（Runner.Worker、Runner.Listener等）
- 从/proc/<pid>/environ读取环境变量
- 筛选包含"env"或"ssh"的关键变量
- 读取变量值指向的文件内容（如SSH密钥）

**内存dump（仅GitHub托管运行器）**（第30-32行）:
- 在GitHub托管的Linux运行器上，利用无密码sudo权限
- 解码并执行Python脚本，定位Runner.Worker进程
- 读取进程内存，搜索GitHub Actions存储 secrets的JSON格式：`"<name>":{"value":"<secret>","isSecret":true}`

**文件系统凭证窃取（自托管运行器）**（第34-36行）:
- 嵌入式Python脚本，自称为"TeamPCP Cloud stealer"
- 广泛搜索以下凭证类别：
  - SSH密钥（~/.ssh/）
  - Git凭证（~/.git-credentials、~/.gitconfig）
  - 云服务：AWS、GCP、Azure凭证
  - Kubernetes服务账户令牌
  - Docker注册表凭证
  - 数据库连接（MySQL、PostgreSQL、MongoDB、Redis）
  - 环境变量文件（.env等）
  - CI/CD配置文件（Terraform、Jenkins、GitLab CI）
  - TLS私钥
  - VPN配置
  - Shell历史记录
  - 加密货币钱包（Bitcoin、Ethereum、Solana等）

#### 第二阶段：加密（第39-68行）

使用混合加密方案保护窃取的凭证：
- 生成32字节随机会话密钥
- 使用AES-256-CBC加密收集的数据
- 使用RSA-4096 OAEP包装会话密钥
- 将加密有效载荷和密钥打包为tpcp.tar.gz

#### 第三阶段：数据外传（第70-104行）

**主通道**:
- HTTPS POST到typosquat域名：scan[.]aquasecurtiy[.]org（"security"拼写错误）
- 使用自定义头部X-Filename: tpcp.tar.gz

**备用通道**:
- 如果主通道失败且存在INPUT_GITHUB_PAT，在受害者GitHub账户创建名为"tpcp-docs"的公开仓库
- 上传加密包作为发布资产
- 攻击者可通过搜索GitHub找到该仓库下载数据

---

## 归属分析

恶意代码自称为"TeamPCP Cloud stealer"，该组织也被称为DeadCatx3、PCPcat和ShellForce。这是一个已知的云原生威胁组织，曾被Flare和The Hacker News报道。该组织以以下活动闻名：
- 滥用配置错误的Docker API
- 攻击Kubernetes集群
- 入侵Ray dashboards
- 攻击Redis服务器
- 传播蠕虫驱动勒索软件
- 加密货币挖矿

---

## 处置建议

### 立即行动

1. **停止使用版本标签**: 立即停止通过版本标签引用trivy-action
2. **安全引用方式**:
   - 固定到完整提交SHA：57a97c7e7821a5776cebc9bb87c984fa69cba8f1
   - 或仅使用标签@0.35.0
3. **凭证轮换**: 任何执行过受污染标签的管道应视为完全泄露，需轮换所有可访问的凭证（云凭证、SSH密钥、API令牌、数据库密码等）

### 调查工作

1. 在GitHub组织中搜索"tpcp-docs"仓库
2. 审查2026年3月19日约19:00 UTC之后的GitHub Actions日志

---

## 威胁指标（IOCs）

### 网络指标
- scan[.]aquasecurtiy[.]org（typosquat域名）

### 文件哈希
- SHA256: 18a24f83e807479438dcab7a1804c51a00dafc1d526698a66e0640d1e5dd671a（entrypoint.sh）

### 受影响的GitHub Actions标签
（完整列表共75个，从@0.0.1到@0.34.2）

---

## 总结

这是一起复杂且影响广泛的供应链攻击。攻击者利用了GitHub标签的可变性和GitHub Actions的信任模型，通过伪造提交元数据和滥用"不可变发布"功能，成功绕过了常规安全检查。攻击者不仅窃取敏感凭证，还设计了具有韧性的数据外传机制，使用受害者自己的GitHub基础设施存储被盗数据。

此事件再次证明：依赖版本标签消费GitHub Actions存在固有风险，组织应始终固定到完整的commit SHA以确保真正不可变。