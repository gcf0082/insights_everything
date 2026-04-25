# 安全威胁分析报告

**洞察链接**: https://socket.dev/blog/bitwarden-cli-compromised

**报告日期**: 2026-04-24

**来源**: Socket Research Team

---

## 基本信息

- **事件**: Bitwarden CLI 在 Checkmarx 供应链攻击活动中被入侵
- **受影响版本**: @bitwarden/cli 2026.4.0
- **发布时间**: 2026年4月23日
- **影响范围**: Bitwarden CLI 构建受影响，超过1000万用户和50000多家企业使用该密码管理器
- **攻击向量**: 攻击者滥用 Bitwarden CI/CD 管道中的 GitHub Action

---

## 技术分析

### 恶意载荷

恶意代码位于 `bw1.js` 文件中，与 Checkmarx 供应链活动中分析的 `mcpAddon.js` 共享核心基础设施：

- **C2通信**: 使用相同的 `audit.checkmarx[.]cx/v1/telemetry` 端点，通过 `__decodeScrambled` 函数（种子为 `0x3039`）进行混淆
- **数据窃取目标**:
  - GitHub Actions Runner.Worker 内存中的令牌
  - AWS 凭据（通过 `~/.aws/` 文件和环境变量）
  - Azure 令牌（通过 azd）
  - GCP 凭据（通过 gcloud config config-helper）
  - npm 配置文件（.npmrc）
  - SSH 密钥
  - Claude/MCP 配置文件

- **俄罗斯地域禁用开关**: 如果系统语言环境以 "ru" 开头，程序将静默退出

### 传播机制

1. 通过 GitHub API 创建公共仓库（使用 Dune 主题命名：`{word}-{word}-{3digits}`）
2. 使用令牌 `LongLiveTheResistanceAgainstMachines` 在提交信息中嵌入加密结果
3. 通过 npm 令牌窃取识别可写包并重新发布，带有注入的 preinstall 钩子
4. GitHub Actions 工作流注入以捕获仓库凭据

### 独特指标（Bitwarden 事件特有）

- **锁文件**: 硬编码路径 `/tmp/tmp.987654321.lock` 防止多个实例同时运行
- **Shell 持久化**: 将载荷注入 `~/.bashrc` 和 `~/.zshrc`
- **品牌标识**: 仓库描述为 `Shai-Hulud: The Third Coming`，调试字符串包含 `"Would be executing butlerian jihad!"`

---

## 危害指标（IOCs）

### 恶意包

- @bitwarden/cli@2026.4.0

### 网络指标

- IP 地址: `94[.]154[.]172[.]43`
- C2 端点: `https://audit.checkmarx[.]cx/v1/telemetry`

### 文件系统指标

- `/tmp/tmp.987654321.lock`
- `/tmp/_tmp_<Unix Epoch Timestamp>/`
- `package-updated.tgz`

### 关键搜索词（Dune 主题命名仓库）

atreides, cogitor, fedaykin, fremen, futar, gesserit, ghola, harkonnen, heighliner, kanly, kralizec, lasgun, laza, melange, mentat, navigator, ornithopter, phibian, powindah, prana, prescient, sandworm, sardaukar, sayyadina, sietch, siridar, slig, stillsuit, thumper, tleilaxu

---

## 处置建议

### 立即行动

1. **移除受影响包**: 从开发者系统和构建环境中立即移除恶意 npm 包
2. **轮换凭据**: 轮换可能暴露的所有凭据，包括：
   - GitHub 令牌
   - npm 令牌
   - 云凭据
   - SSH 密钥
   - CI/CD 密钥

3. **GitHub 审计**: 检查是否存在以下情况：
   - 未授权的仓库创建
   - `.github/workflows/` 下的异常工作流文件
   - 可疑的工作流运行
   - 使用 Dune 主题命名模式（`{word}-{word}-{3digits}`）的公共仓库

4. **npm 审计**: 检查未授权的发布、版本更改或新添加的安装钩子

### 云环境审计

- 检查异常的秘密访问
- 检查令牌使用情况
- 检查新颁发的凭据

### 端点和运行器检查

- 排查到 `audit[.]checkmarx[.]cx` 的出站连接
- 检查 Bun 运行时（非常规使用）
- 检查对 .npmrc、.git-credentials、.env 等文件的访问
- 检查云凭据存储的访问
- 检查锁文件 `/tmp/tmp.987654321.lock`
- 检查 `~/.bashrc` 和 `~/.zshrc` 中的 Shell 配置文件修改

### 长期控制措施

- 锁定令牌作用域
- 使用短期凭据
- 限制谁可以创建或发布包
- 强化 GitHub Actions 权限
- 禁用不必要的工件访问
- 监控新创建的公共仓库或异常的工作流变更

---

## 结论

Bitwarden CLI 2026.4.0 在 Checkmarx 供应链活动中被入侵。攻击者通过滥用 Bitwarden CI/CD 管道中的 GitHub Action 来发布恶意代码。该恶意代码与 Checkmarx 事件中使用的恶意软件共享核心基础设施，但运营特征有所不同，可能表明存在不同的攻击者或同一攻击者的策略演变。

建议所有 Bitwarden CLI 用户立即检查并移除受影响版本，轮换可能暴露的任何凭据，并审查系统中的异常活动。