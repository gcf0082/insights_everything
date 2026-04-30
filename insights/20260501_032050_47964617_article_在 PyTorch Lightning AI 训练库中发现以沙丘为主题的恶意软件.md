# 洞察报告：PyTorch Lightning AI训练库中发现Shai-Hulud主题恶意软件

**洞察链接**: https://semgrep.dev/blog/2026/malicious-dependency-in-pytorch-lightning-used-for-ai-training/

**发布日期**: 2026年4月30日

**来源**: Semgrep Security Research

---

## 概述

PyPI包"lightning"（广泛使用的深度学习框架）在2026年4月30日发布的2.6.2和2.6.3版本中遭受供应链攻击，被植入恶意代码。该恶意软件以"Shai-Hulud"（沙虫）为主题，可执行凭证窃取恶意程序。

## 受影响版本

- `lightning` 版本 2.6.2
- `lightning` 版本 2.6.3

## 攻击机制

恶意版本包含隐藏的`_runtime`目录，其中包含混淆的JavaScript有效载荷，在模块导入时自动执行。攻击流程：

1. **入口点**: 用户运行`pip install lightning`即可激活恶意代码
2. **跨生态系统传播**: 恶意代码从PyPI进入，但通过npm进行蠕虫式传播
3. **如果发现npm发布凭证**: 恶意软件向每个可发布的包注入`setup.mjs`dropper和`router_runtime.js`，设置`scripts.preinstall`执行dropper，递增补丁版本并重新发布

## 数据窃取目标

恶意软件针对本地文件、环境变量、CI/CD管道和云提供商的多类凭证：

- **文件系统**: 扫描80+凭证文件路径，查找`ghp_`、`gho_`和`npm_`令牌
- **Shell/环境**: 执行`gh auth token`并转储所有环境变量
- **GitHub Actions**: 在Linux运行器上转储Runner.Worker进程内存，提取所有标记为`isSecret:true`的秘密
- **AWS**: 尝试环境变量、~/.aws/credentials配置文件、IMDSv2和ECS获取STS凭证，枚举 Secrets Manager 和 SSM 参数
- **Azure**: 使用DefaultAzureCredential枚举订阅并访问Key Vault秘密
- **GCP**: 通过GoogleAuth认证，枚举并获取所有Secret Manager秘密

## 四条数据外泄通道

1. **HTTPS POST到C2服务器**: 立即将窃取的数据POST到攻击者控制的服务器（端口443）
2. **GitHub提交搜索死Drop**: 轮询GitHub提交搜索API，查找前缀为`EveryBoiWeBuildIsAWormyBoi`的提交消息
3. **攻击者控制的公共GitHub仓库**: 创建名为Dune词汇的公共仓库，描述为"A Mini Shai-Hulud has Appeared"
4. **推送到受害者自己的仓库**: 如果获得`ghs_` GitHub服务器令牌，将窃取的数据推送到受害者的所有分支

## 持久化机制

恶意软件针对两种常见开发者工具植入持久化钩子：

- **Claude Code**: 在`.claude/settings.json`中写入`SessionStart`钩子，每次打开Claude Code时触发
- **VS Code**: 在`.vscode/tasks.json`中写入`runOn: folderOpen`任务，每次打开项目文件夹时触发

## 恶意GitHub Actions工作流

如果恶意软件持有具有写访问权限的GitHub令牌，会推送名为"Formatter"的工作流，每次推送时通过`${{ toJSON(secrets) }}`转储所有仓库秘密，并将其作为可下载的Actions工件上传。

## 威胁参与者归属

Semgrep认为此次攻击与之前的mini Shai-Hulud活动出自同一威胁参与者。IOC结构一致：恶意提交消息遵循相同的Dune主题命名约定，本次活动使用前缀`EveryBoiWeBuildIsAWormyBoi`区分于原始mini Shai-Hulud攻击。

## 指标（IOCs）

### 包版本
- `lightning@2.6.2`
- `lightning@2.6.3`

### 提交消息前缀
- `EveryBoiWeBuildIsAWormyBoi`（死Drop令牌载体，可通过GitHub提交搜索查找）

### GitHub仓库
- 描述为"A Mini Shai-Hulud has Appeared"的仓库（攻击者外泄仓库，可直接搜索）

### 文件/系统工件
- `_runtime/start.py`: Python加载器
- `_runtime/router_runtime.js`: 混淆的JavaScript有效载荷（14.8 MB）
- `.claude/router_runtime.js`: 注入到受害者仓库的恶意软件副本
- `.claude/settings.json`: Claude Code钩子配置
- `.claude/setup.mjs`: 注入到受害者仓库的dropper
- `.vscode/tasks.json`: VS Code自动运行任务
- `.vscode/setup.mjs`: 注入到受害者仓库的dropper

## 建议措施

1. 触发新扫描检查项目
2. 检查 advisories 页面确认是否安装了这些包版本
3. 检查依赖过滤器是否匹配
4. 如果匹配，审计仓库中注入的文件（.claude/和.vscode/目录）
5. 轮换可能出现在受影响环境中的GitHub令牌、云凭证或API密钥

任何在受影响窗口期间导入恶意包的机器都应被视为完全受感染。