# 洞察报告：Cog — Claude Code 认知架构

**洞察链接**: https://lab.puga.com.br/cog/  
**来源**: GitHub (marciopuga/cog)  
**发布日期**: 2026年3月  
**作者**: Marcio Puga  

---

## 概述

Cog 是一个纯文本认知架构，专为 Claude Code 设计，旨在为其添加持久记忆、自我反思和前瞻能力。该项目通过纯 Markdown 文件实现，无需服务器或运行时依赖。截至目前，已获得 271 Stars 和 16 Forks。

---

## 核心功能

### 1. 三层记忆系统

| 层级 | 文件位置 | 描述 |
|------|----------|------|
| **热记忆** | `memory/hot-memory.md` | 始终加载，不超过50行，包含当前优先级和活跃状态 |
| **温记忆** | `memory/personal/`、`memory/work/` | 领域特定文件，相关技能激活时加载 |
| **冰河记忆** | `memory/glacier/` | 归档内容，通过 YAML frontmatter 索引，按需检索 |

### 2. 内置技能（Commands）

- `/setup` - 对话式领域设置
- `/personal` - 个人生活管理
- `/reflect` - 挖掘对话、提取模式、提炼要点
- `/evolve` - 审计记忆架构，提出规则改进
- `/foresight` - 跨域战略提醒
- `/scenario` - 决策模拟
- `/housekeeping` - 归档、修剪、链接审计
- `/history` - 跨记忆文件深度搜索

### 3. 渐进式提炼

- **提炼（Condensation）**: 观察 → 模式 → 热记忆，每层比上一层更精简、更具可操作性
- **归档（Archival）**: 旧观察 → 冰河记忆，保留索引、可检索

### 4. 线程系统（Thread）

采用 Zettelkasten 方法，当一个主题在 3+ 观察中出现且跨 2+ 周时，Cog 会将其提升为线程文件，包含：
- 当前状态
- 时间线（按日期排列）
- 洞察（模式、学习）

---

## 快速开始

```bash
git clone https://github.com/marciopuga/cog
cd cog
```

在 Claude Code 中打开项目，然后输入：

```
/setup
```

Cog 会询问您的生活和工作情况，然后生成：域清单、记忆目录、技能文件和路由表。

---

## 技术特点

1. **纯文本设计** - 使用 Unix 工具（grep、find、git diff）操作记忆
2. **单一事实来源（SSOT）** - 每个事实存储在一个规范文件中
3. **Wiki 链接** - 文件间通过 `[[domain/filename]]` 相互引用
4. **L0/L1/L2 分层加载** - 记忆文件带有一行摘要，实现三级检索协议

---

## 集成外部工具

通过 MCP（Model Context Protocol）可连接：
- Google Calendar
- Gmail
- Slack
- GitHub
- Linear/Jira
- Notion/Obsidian

---

## 适用场景

- **长期项目** - 跨数周或数月保持上下文
- **个人知识管理** - 追踪洞察、决策和进度
- **团队入职** - 与新成员分享领域知识和项目历史
- **反思性开发** - 分析编码习惯和决策模式

---

## 架构优势

1. **完全可观察性** - 可查看 Claude 如何组织知识
2. **零锁定** - 停止使用 Cog 后记忆仍可访问
3. **Git 兼容** - 所有变更通过版本控制跟踪
4. **无性能开销** - 文本文件几乎无延迟

---

## 许可证

MIT License

---

## 参考链接

- 官方网站: https://lab.puga.com.br/cog/
- GitHub: https://github.com/marciopuga/cog
- Hacker News: https://news.ycombinator.com/item?id=47524704
