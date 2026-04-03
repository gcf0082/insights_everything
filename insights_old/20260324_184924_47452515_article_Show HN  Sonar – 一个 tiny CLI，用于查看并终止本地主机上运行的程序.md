# 项目洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **仓库名称** | sonar |
| **仓库链接** | https://github.com/RasKrebs/sonar |
| **描述** | CLI tool for inspecting and managing services listening on localhost ports |
| **所有者** | raskrebs |
| **星标数** | 678 |
| **Fork数** | 19 |
| **Watch数** | 1 |
| **主要语言** | Go (85.5%), Swift (9.9%), Shell (2.7%), PowerShell (1.9%) |
| **许可证** | MIT |
| **最新版本** | v0.2.4 (2026年3月24日) |
| **提交数** | 32 |
| **主题标签** | macos, linux, docker, cli, golang, powershell, developer-tools, localhost, portscanner |

---

## 项目概述

**Sonar** 是一款用于检查和管理本地主机端口上服务的命令行工具。作者受够了每次端口被占用时都要运行复杂的 `lsof` 命令，所以开发了这款工具。

该工具可以显示所有监听 localhost 的服务信息，包括 Docker 容器名称、Compose 项目、资源使用情况，以及可点击的 URL。用户可以直接通过端口号来终止进程、查看日志、进入容器等。

---

## 核心功能

### 1. 端口列表查看
- 显示所有监听端口
- 支持显示 CPU、内存、状态、运行时间等统计信息
- 支持按类型（docker）、名称排序
- 支持 JSON 输出
- 支持自定义列显示
- 支持 HTTP 健康检查
- 支持通过 SSH 扫描远程机器

### 2. 端口检查
- 显示端口的完整信息：完整命令、用户、绑定地址、CPU/内存/线程数、运行时间、健康检查结果、Docker 详情

### 3. 终止进程
- 支持 SIGTERM 和 SIGKILL
- 支持按过滤器停止所有 Docker 容器
- 支持按项目停止 Compose 项目

### 4. 日志查看
- Docker 容器使用 `docker logs -f`
- 本地进程通过 `lsof` 发现日志文件并 tail
- 回退到 macOS `log stream` 或 Linux `/proc/<pid>/fd`

### 5. 连接到服务
- 进入 Docker 容器或 TCP 连接

### 6. 监控变化
- 轮询并显示差异
- 实时资源统计（类似 docker stats）
- 桌面通知（端口上下线时）

### 7. 依赖关系图
- 显示服务之间的连接关系

### 8. 配置文件
- 保存项目的预期端口快照
- 检查预期端口是否运行
- 停止配置文件中的所有端口

### 9. 端口等待
- 阻塞直到端口接受连接
- 支持 HTTP 200 等待
- 适合在 `docker compose up -d` 后使用

### 10. 端口映射
- 代理流量，使服务可在多个端口访问

### 11. 查找空闲端口
- 查找下一个空闲端口
- 支持端口范围

### 12. 其他功能
- 浏览器打开端口
- macOS 菜单栏应用

---

## 安装方式

### Linux/macOS
```bash
curl -sfL https://raw.githubusercontent.com/raskrebs/sonar/main/scripts/install.sh | bash
```

### Windows (PowerShell)
```powershell
irm https://raw.githubusercontent.com/raskrebs/sonar/main/scripts/install.ps1 | iex
```

### 使用 Go
```bash
go install github.com/raskrebs/sonar@latest
```

---

## 支持平台

- **macOS** (使用 lsof)
- **Linux** (使用 ss)
- **Windows** (使用 netstat)

---

## 技术栈

| 组件 | 占比 | 说明 |
|------|------|------|
| Go | 85.5% | 主要实现语言 |
| Swift | 9.9% | macOS 菜单栏应用 |
| Shell | 2.7% | 安装脚本 |
| PowerShell | 1.9% | Windows 安装脚本 |

---

## 使用示例

```
$ sonar list
PORT   PROCESS                      CONTAINER                    IMAGE             CPORT   URL
1780   proxy (traefik:3.0)          my-app-proxy-1               traefik:3.0       80      http://localhost:1780
3000   next-server (v16.1.6)                                                               http://localhost:3000
5432   db (postgres:17)             my-app-db-1                  postgres:17       5432    http://localhost:5432
6873   frontend (frontend:latest)   my-app-frontend-1            frontend:latest   5173    http://localhost:6873
9700   backend (backend:latest)     my-app-backend-1            backend:latest    8000    http://localhost:9700

5 ports (4 docker, 1 user)
```

---

## 洞察结论

1. **实用性强**：解决了开发者日常遇到的端口管理痛点，集成度高
2. **跨平台支持**：支持 macOS、Linux、Windows 三大平台
3. **功能丰富**：涵盖端口查看、检查、终止、日志、监控、依赖图等完整功能
4. **Docker 深度集成**：与 Docker 容器管理紧密结合
5. **活跃度高**：678 星标，32 次提交，持续维护更新
6. **开源免费**：MIT 许可证，可自由使用和修改
