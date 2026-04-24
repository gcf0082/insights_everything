# Infisical Agent Vault 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **项目名称** | Agent Vault |
| **组织/开发者** | Infisical |
| **GitHub链接** | https://github.com/Infisical/agent-vault |
| **项目描述** | HTTP凭证代理和保险库，为AI代理提供_credentials管理 |
| **许可证** | MIT |
| **Stars** | 636 |
| **Forks** | 29 |
| **Commits** | 129 |
| **Releases** | 15 (最新版本 v0.10.0, 2026年4月23日) |
| **编程语言** | Go 71.0%, TypeScript 27.0%, HTML 0.9%, Shell 0.7%, CSS 0.2%, Dockerfile 0.1%, Makefile 0.1% |

## 项目概述

Agent Vault是由Infisical开发的开源HTTP凭证代理和保险库，旨在解决AI代理_credentials管理中的安全风险。传统密钥管理依赖直接将凭证返回给调用者，但这在AI代理场景下存在安全隐患——AI代理是非确定性系统，容易受到提示注入攻击，可能被诱导泄露密钥。Agent Vault采用不同的方法：永不向代理暴露存储的凭证，而是通过本地代理路由HTTP请求，在网络层注入正确的凭证。

## 核心特性

### 1. 代理访问模式
- 代理获得作用域会话和本地HTTPS_PROXY
- 代理正常调用目标API，Agent Vault在网络层注入凭证
- 凭证永远不会返回给代理

### 2. 广泛兼容性
- 支持自定义Python/TypeScript代理
- 支持沙盒进程
- 支持编码代理：Claude Code、Cursor、Codex、OpenClaw、Hermes、OpenCode

### 3. 静态加密
- 使用AES-256-GCM加密
- 随机数据加密密钥(DEK)
- 可选主密码通过Argon2id包装DEK
- 支持无密码模式部署

### 4. 请求日志
- 记录每个代理请求：方法、主机、路径、状态、延迟、涉及的凭证密钥
- 不记录请求体、_headers和查询字符串
- 可配置保留策略

## 安装方式

### 脚本安装 (macOS / Linux)
```bash
curl -fsSL https://get.agent-vault.dev | sh
agent-vault server -d
```

### Docker安装
```bash
docker run -it -p 14321:14321 -p 14322:14322 -v agent-vault-data:/data infisical/agent-vault
```

### 源码安装
需要Go 1.25+和Node.js 22+
```bash
git clone https://github.com/Infisical/agent-vault.git
cd agent-vault
make build
sudo mv agent-vault /usr/local/bin/
agent-vault server -d
```

服务启动：
- HTTP API: 端口14321
- TLS加密透明HTTPS代理: 端口14322
- Web UI: http://localhost:14321

## 快速开始

### CLI模式 - 本地代理
```bash
agent-vault run -- claude
agent-vault vault run -- agent
agent-vault vault run -- codex
agent-vault vault run -- opencode
```

沙盒模式（容器隔离）：
```bash
agent-vault run --sandbox=container --share-agent-dir -- claude
```

### SDK模式 - 沙盒代理
安装TypeScript SDK：
```bash
npm install @infisical/agent-vault-sdk
```

使用示例：
```typescript
import { AgentVault, buildProxyEnv } from "@infisical/agent-vault-sdk";

const av = new AgentVault({
  token: "YOUR_TOKEN",
  address: "http://localhost:14321",
});
const session = await av.vault("default").sessions.create({ vaultRole: "proxy" });

const env = buildProxyEnv(session.containerConfig!, certPath);
const caCert = session.containerConfig!.caCertificate;
```

## 开发相关

构建命令：
- `make build` - 构建前端+Go二进制
- `make test` - 运行测试
- `make web-dev` - Vite开发服务器（端口5173）
- `make dev` - Go+Vite开发服务器
- `make docker` - 构建Docker镜像

## 与付费版对比

开源版本采用MIT许可证，例外是`ee`目录包含需要Infisical许可证的企业功能。

## 总结

Agent Vault是一个创新的开源安全工具，专门解决AI代理场景下的凭证管理安全问���。通过代理访问模式而非凭证返回模式，有效防止提示注入攻击导致的凭证泄露。项目的特点是支持多种AI代理、静态加密、请求审计日志，并且提供CLI和SDK两种集成方式，满足不同部署需求。