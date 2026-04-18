# 洞察报告：从 DigitalOcean 迁移到 Hetzner

**洞察链接**: https://isayeter.com/posts/digitalocean-to-hetzner-migration/

**发布日期**: 2026-03-17

**作者**: Isa Yeter

---

## 摘要

本文记录了一次从 DigitalOcean 迁移到 Hetzner 专用服务器的真实生产迁移案例。原服务器月费用为 $1,432，迁移后仅为 $233/月，节省约 $14,388/年，同时获得更强大的硬件配置。整个迁移过程实现了零停机，耗时约 24 小时。

---

## 迁移动因

土耳其的通胀和里拉贬值使美元计价的云服务成本急剧上升。原 DigitalOcean 配置：
- 192GB RAM
- 32 vCPU
- 600GB SSD + 2x1TB 数据卷
- 月费用：$1,432

新 Hetzner AX162-R 配置：
- 256GB DDR5
- AMD EPYC 9454P（48核心/96线程）
- 1.92TB NVMe Gen4 RAID1
- 月费用：$233

---

## 迁移内容

原服务器运行：
- 30 个 MySQL 数据库（248GB 数据）
- 34 个 Nginx 虚拟主机
- GitLab EE（42GB 备份）
- Neo4j 图数据库（30GB）
- Supervisor 管理的后台工作者
- Gearman 任务队列
- 服务于数十万用户的移动应用

操作系统：从 CentOS 7 升级到 AlmaLinux 9.7

---

## 零停机迁移策略

### 第一阶段：新服务器全栈安装

安装与原服务器相同配置的所有服务：Nginx（从源码编译）、PHP（通过 Remi 仓库）、MySQL 8.0、Neo4j、GitLab EE、Node.js、Supervisor、Gearman。

SSL 证书通过 rsync 复制整个 `/etc/letsencrypt/` 目录。

### 第二阶段：Web 文件同步

使用 rsync 复制 `/var/www/html` 目录（约 65GB，150万文件），使用 `--checksum` 标志验证完整性。

### 第三阶段：MySQL 主从复制

使用 mydumper 进行并行导出（32线程），比传统 mysqldump 快得多。记录 binlog 位置用于启动复制。

### 第四阶段：DNS TTL 降低

通过 DigitalOcean API 将 A 和 AAAA 记录的 TTL 从 3600 秒降至 300 秒，等待 1 小时让旧 TTL 过期。

### 第五阶段：旧服务器 Nginx 转换为反向代理

编写 Python 脚本解析所有 34 个 Nginx 配置，将其转换为指向新服务器的代理配置。在 DNS 切换期间，仍访问旧 IP 的请求会被透明转发。

### 第六阶段：DNS 切换和 decommission

执行 DNS 切换，将所有 A 记录指向新服务器 IP。旧服务器作为冷备保留一周后关闭。

---

## MySQL 迁移细节

### 数据导出

```bash
mydumper --threads 32 --compress --trx-consistency-only --skip-definer --chunk-filesize 256 -v 3 --outputdir /root/mydumper_backup/
```

### 数据导入

```bash
myloader --threads 32 --overwrite-tables --ignore-errors 1062 --skip-definer -v 3 --directory /root/mydumper_backup/
```

### MySQL 5.7 到 8.0 升级问题

迁移期间发现 `mysql.user` 表列数不匹配（45列 vs 51列），导致认证失败。解决方法是删除损坏的 sys 数据库并重新运行升级：

```sql
DROP DATABASE sys;
mysqld --upgrade=FORCE --user=mysql &
```

### 复制配置

复制启动后遇到重复键错误，通过设置 `slave_exec_mode = IDEMPOTENT` 解决。

### SUPER 权限问题

发现所有 PHP 应用用户都有 SUPER 权限，这会绕过 `read_only` 设置。必须从所有 24 个应用用户撤销 SUPER 权限才能正确保护从库。

---

## 切换步骤

1. 新服务器：STOP SLAVE
2. 新服务器：SET GLOBAL read_only = 0
3. 新服务器：RESET SLAVE ALL
4. 新服务器：supervisorctl start all
5. 旧服务器：nginx -s reload（代理生效）
6. 旧服务器：supervisorctl stop all
7. 执行 DNS 切换脚本
8. 等待约 5 分钟传播
9. 旧服务器：注释掉所有 crontab 条目

---

## 切换后问题

迁移后发现许多 GitLab 项目 Webhook 仍指向旧服务器 IP，通过 GitLab API 批量更新解决。

---

## 最终结果

| 项目 | 迁移前 | 迁移后 |
|------|--------|--------|
| 月费用 | $1,432 | $233 |
| CPU | 32 vCPU | 96 逻辑线程 |
| RAM | 192 GB | 256 GB DDR5 |
| 存储 | ~2.6 TB 混合 | 2 TB NVMe RAID1 |
| 停机时间 | - | 0 分钟 |

**年度节省**：$14,388

---

## 关键经验

1. **MySQL 复制是零停机迁移的最佳方案**：提前设置复制，让其同步，然后自信地进行切换。

2. **检查 MySQL 用户权限**：SUPER 权限会绕过 read_only 设置。

3. **所有操作脚本化**：DNS 更新、Nginx 配置重写、Webhook 更新，手动操作容易出错且耗时。

4. **mydumper/myloader 显著优于 mysqldump**：对于大型数据集，32 线程并行导出/导入将数天工作缩短为数小时。

5. **云服务对稳定工作负载来说价格昂贵**：如果不使用自动扩展或临时基础设施，专用服务器通常能以更低的价格提供更好的性能。

---

## 开源脚本

所有迁移脚本已在 GitHub 开源：https://github.com/isayeter/digitalocean_to_hetzner