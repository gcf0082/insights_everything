# Canonical Ubuntu 软件包镜像服务异常事件报告

**洞察链接**: https://status.canonical.com/#/incident/KNms6QK9ewuzz-7xUsPsNylV20jEt5kyKsd8A-3ptQEHpOd8VQ40ZQs-KD81fboQXeGZB94okNHdHBGlCv58Sw==

**报告日期**: 2026-05-02

**事件概述**: Canonical (Ubuntu) 软件包镜像服务出现降级 performance，影响用户使用 apt-get 命令及 Docker 构建。

---

## 事件详情

### 受影响服务
- **archive.ubuntu.com** - Ubuntu 主软件包归档服务器
- **security.ubuntu.com** - Ubuntu 安全更新服务器
- 相关的 Docker 构建服务

### 事件影响
1. **apt-get 命令超时**: 用户在运行 `apt-get update` 或 `apt-get install` 时可能出现连接超时问题
2. **Docker 构建失败**: 使用 Ubuntu 基础镜像的 Docker 构建遇到降级 performance 和错误
3. **镜像同步延迟**: 部分镜像服务器与主服务器之间的同步出现延迟

### 技术细节
- 表现为下载软件包列表极慢
- 请求返回 403 错误
- 无法连接到镜像服务器
- 构建 Docker 容器时 Stateless 下载大量软件包时问题尤为突出

---

## 临时解决方案

### 方案一：切换到替代镜像
```bash
# Ubuntu 22.04 及更早版本
sed -i 's%http://\(archive\|security\).ubuntu.com/%http://azure.archive.ubuntu.com/%g' /etc/apt/sources.list

# Ubuntu 24.04 及更高版本
sed -i 's%http://\(archive\|security\).ubuntu.com/%http://azure.archive.ubuntu.com/%g' /etc/apt/sources.list.d/ubuntu.sources
```

### 方案二：使用云服务商提供的镜像
- AWS 镜像: `http://us-east-2.ec2.archive.ubuntu.com/ubuntu/`
- Azure 镜像: `http://azure.archive.ubuntu.com/`

### 方案三：使用其他社区镜像
- MIT 镜像: `http://mirrors.mit.edu/ubuntu`
- 其他国家镜像: https://launchpad.net/ubuntu/+archivemirrors

**注意**: 建议在服务恢复后还原这些更改，以保持与基础镜像的一致性。

---

## 事件时间线

| 时间 | 事件 |
|------|------|
| 2025-05-29 | 用户开始报告 Ubuntu 镜像连接问题 |
| 2025-05-30 | Canonical 确认问题并开始修复 |
| 2025-05-30 | 服务返回正常水平，事件已解决 |

---

## 受影响范围

- 所有使用 Ubuntu 软件包管理器的用户
- 使用 Ubuntu 基础镜像的 Docker 用户
- 需要从 Canonical 服务器下载软件包和更新的系统

---

## 参考链接

- Canonical 状态页面: https://status.canonical.com/
- Ubuntu 镜像列表: https://launchpad.net/ubuntu/+archivemirrors
- Ask Ubuntu 相关讨论: https://askubuntu.com/questions/1549622

---

## 总结

此次事件导致 Ubuntu 软件包镜像服务出现降级 performance，影响了用户的系统更新和 Docker 构建流程。Canonical 在发现问题后进行了修复，并通过状态页面发布了更新。建议用户关注官方状态页面以获取最新信息，并在遇到问题时考虑使用替代镜像作为临时解决方案。