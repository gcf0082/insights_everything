# 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://www.phoronix.com/news/Linux-7.0-AWS-PostgreSQL-Drop |
| **来源** | Phoronix |
| **发布日期** | 2026年4月4日 |
| **作者** | Michael Larabel |
| **分类** | Linux Kernel |

---

## 摘要

AWS工程师报告称，Linux 7.0开发内核导致PostgreSQL数据库性能下降约一半。在Graviton4服务器上，Linux 7.0的PostgreSQL吞吐量仅为之前内核版本的约0.51倍。

---

## 详细内容

### 问题描述

- **性能影响**：PostgreSQL吞吐量下降至原来的一半
- **延迟影响**：同时出现延迟退化
- **测试环境**：AWS Graviton4服务器
- **根本原因**：Linux 7.0限制了内核抢占模式的可用性

### 问题根源

性能回归被追溯到Linux 7.0的一个变更：限制了可用的抢占模式。这一变更与Linux 7.0的调度器更新相关。

### 修复尝试与争议

1. **官方补丁**：有人在Linux内核邮件列表上提交了恢复PREEMPT_NONE作为默认值的补丁
2. **争议回应**：原代码作者Peter Zijlstra回应称"修复方案是让PostgreSQL使用Restartable Sequences (RSEQ)时间片扩展"
3. **现状**：该修复可能不会被接受，PostgreSQL可能需要自行适配

### 时间线

- Linux 7.0稳定版预计约两周后发布
- Linux 7.0也是即将于4月发布的Ubuntu 26.04 LTS的内核版本

---

## 影响与建议

PostgreSQL用户需要关注Linux 7.0正式发布后的性能表现。在问题修复前，某些场景下可能出现显著的性能下降。PostgreSQL社区可能需要采用RSEQ时间片扩展来应对这一变化。

---

## 相关链接

- [Linux 7.0 限制抢占模式新闻](https://www.phoronix.com/news/Linux-Restrict-Preempt-Modes)
- [Linux 7.0 调度器更新](https://www.phoronix.com/news/Linux-7.0-Scheduler)
- [RSEQ时间片扩展支持](https://www.phoronix.com/news/Linux-TIP-Time-Slice-Extension)