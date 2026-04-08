# Locker 洞察报告

## 基本信息

- **洞察链接**: https://locker.dev
- **洞察时间**: 2026-04-07
- **文件路径**: data/20260407_230752_47673394_article.md

---

## 产品概述

Locker 是一个开源的自托管文件存储平台，可作为 Dropbox 和 Google Drive 的替代方案。用户可以在自己的基础设施上上传、整理和共享文件。

## 核心特性

### 1. 存储提供商无关
支持本地磁盘、AWS S3、Cloudflare R2 或 Vercel Blob，通过单个环境变量即可切换存储后端。

### 2. 图片和 PDF 搜索
支持文件内容搜索，能够将图片和 PDF 转录为可搜索的文本。

### 3. 虚拟 Bash Shell
提供虚拟文件系统 API，支持使用 ls、cd、find、cat、grep 等命令导航文件。

### 4. 工作区团队协作
支持邀请团队成员，基于角色的访问控制，提供细粒度的权限管理。

### 5. 安全认证
支持邮箱/密码和 Google OAuth 认证，会话通过加密 Cookie 在服务端管理。

### 6. API 密钥
提供程序化访问文件的接口，支持通过 API 密钥构建集成和自动化工作流。

## 技术栈

- **前端/后端**: Next.js 16 (App Router)
- **数据库**: PostgreSQL + Drizzle ORM
- **API**: tRPC (端到端类型安全)
- **认证**: BetterAuth
- **UI**: Tailwind CSS
- **架构**: Turborepo monorepo + pnpm workspaces

## 部署方式

1. 克隆仓库
2. 运行 pnpm install
3. 启动 PostgreSQL 数据库
4. 配置 .env 文件
5. 运行数据库迁移
6. 启动服务

## 价格

完全免费开源，仅需支付托管基础设施的费用。

---

## 总结

Locker 是一个功能完整的开源文件存储解决方案，特别适合需要完全控制数据、避免供应商锁定、并希望降低文件存储成本的用户和团队。其支持多种存储后端、类型安全的 API 以及团队协作功能，使其成为一个有竞争力的自托管选择。