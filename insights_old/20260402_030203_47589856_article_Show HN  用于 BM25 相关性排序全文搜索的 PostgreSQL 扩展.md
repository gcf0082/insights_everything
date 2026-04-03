# pg_textsearch 仓库洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://github.com/timescale/pg_textsearch |
| **项目名称** | timescale/pg_textsearch |
| **描述** | PostgreSQL extension for BM25 relevance-ranked full-text search |
| **星标数** | 3.5k |
| **分叉数** | 94 |
| **主语言** | C (51.0%), PL/pgSQL (26.1%), Shell (17.1%) |
| **许可证** | PostgreSQL license |
| **最新版本** | v1.0.0 (2026年3月27日) |
| **状态** | 生产就绪 (Production ready) |

---

## 项目概述

pg_textsearch 是 Timescale 公司开发的 PostgreSQL 扩展，提供基于 BM25 算法的相关性排名全文搜索功能。该项目原名为 "Tapir"（Textual Analysis for Postgres Information Retrieval），现在仍使用 tapir 作为吉祥物。

### 核心特性

- **简单语法**: `ORDER BY content <@> 'search terms'`
- **BM25 排名**: 可配置的 k1、b 参数
- **多语言支持**: 支持 PostgreSQL 文本搜索配置（english、french、german 等）
- **Top-k 查询优化**: 通过 Block-Max WAND 优化加速
- **并行索引构建**: 支持大规模表的并行索引创建
- **分区表支持**: 支持 PostgreSQL 分区表

### 技术规格

- **PostgreSQL 版本支持**: PostgreSQL 17 和 18
- **编程语言**: C (51.0%), PL/pgSQL (26.1%), Shell (17.1%), HTML (4.9%), Makefile (0.9%)
- **提交数**: 209 次提交

---

## 主要功能

### 1. 索引创建

```sql
CREATE INDEX docs_idx ON documents USING bm25(content) WITH (text_config='english');
```

支持自定义参数：
- `text_config`: PostgreSQL 文本搜索配置（必需）
- `k1`: 词频饱和参数（默认 1.2）
- `b`: 长度归一化参数（默认 0.75）

### 2. 查询语法

```sql
SELECT * FROM documents
ORDER BY content <@> 'database system'
LIMIT 5;
```

注意：`<@>` 返回负 BM25 分数，因为 PostgreSQL 只支持 ASC 顺序索引扫描。

### 3. 数据类型

- **bm25query**: 用于 BM25 评分的查询类型
- 支持显式索引名称指定和嵌入式索引名称语法

### 4. 性能优化

- **Memtable 架构**: 高效的写入支持
- **并行索引构建**: 自动使用并行工作者
- **段压缩**: 默认启用，减少索引大小并提升查询性能
- **Block-Max WAND 优化**: 跳过不可能贡献 top 结果的块

### 5. 配置参数

| 参数 | 默认值 | 描述 |
|------|--------|------|
| pg_textsearch.default_limit | 1000 | 无 LIMIT 子句时的最大评分文档数 |
| pg_textsearch.compress_segments | on | 压缩新段中的 posting 块 |
| pg_textsearch.segments_per_level | 8 | 自动压缩前每层段数 |
| pg_textsearch.bulk_load_threshold | 100000 | 每事务术语数阈值 |
| pg_textsearch.memtable_spill_threshold | 32000000 | 自动溢出前的 posting 条目数 |

---

## 限制与注意事项

1. **不支持短语查询**: BM25 索引存储词频但不存储词位置，无法原生评估短语查询
2. **不支持表达式索引**: 每个 BM25 索引只覆盖单个文本列
3. **无内置分面搜索**: 需使用标准 PostgreSQL 查询机制实现
4. **插入/更新性能**: 持续写入密集型工作负载尚未完全优化
5. **无后台压缩**: 段压缩目前在 memtable 溢出期间同步运行
6. **分区表分数计算**: 每个分区维护独立的统计数据，分数可能不可直接比较
7. **词长度限制**: 继承 PostgreSQL 的 tsvector 词长度限制（2047 字符）
8. **PL/pgSQL 支持**: 在 PL/pgSQL 中需使用显式索引名称

---

## 安装方式

### 预构建二进制

从 Releases 页面下载，适用于 Linux 和 macOS（amd64 和 arm64），支持 PostgreSQL 17 和 18。

### 源码构建

```bash
cd /tmp
git clone https://github.com/timescale/pg_textsearch
cd pg_textsearch
make
make install
```

需要配置 `shared_preload_libraries`:

```sql
shared_preload_libraries = 'pg_textsearch'
```

---

## 监控与调试

```sql
-- 查看索引使用情况
SELECT schemaname, tablename, indexname, idx_scan, idx_tup_read, idx_tup_fetch
FROM pg_stat_user_indexes
WHERE indexrelid::regclass::text ~ 'pg_textsearch';

-- 合并段以提升查询速度
SELECT bm25_force_merge('docs_idx');

-- 强制 memtable 溢出到磁盘
SELECT bm25_spill_index('docs_idx');

-- 查看索引统计信息
SELECT bm25_summarize_index('docs_idx');
```

---

## 使用场景示例

### 基础搜索

```sql
CREATE TABLE articles (id serial PRIMARY KEY, title text, content text);
CREATE INDEX articles_idx ON articles USING bm25(content) WITH (text_config='english');

INSERT INTO articles (title, content) VALUES
    ('Database Systems', 'PostgreSQL is a powerful relational database system'),
    ('Search Technology', 'Full text search enables finding relevant documents quickly'),
    ('Information Retrieval', 'BM25 is a ranking function used in search engines');

SELECT title, content <@> 'database search' as score
FROM articles
ORDER BY score;
```

### 预过滤与后过滤

- **预过滤**: 先用 B-tree 等索引过滤，再对匹配行评分
- **后过滤**: 先用 BM25 索引评分，再过滤结果

---

## 相关资源

- [Releases](https://github.com/timescale/pg_textsearch/releases)
- [ROADMAP.md](https://github.com/timescale/pg_textsearch/blob/main/ROADMAP.md)
- [CHANGELOG.md](https://github.com/timescale/pg_textsearch/blob/main/CHANGELOG.md)
- [CONTRIBUTING.md](https://github.com/timescale/pg_textsearch/blob/main/CONTRIBUTING.md)

---

## 总结

pg_textsearch 是一个生产就绪的 PostgreSQL 全文搜索扩展，基于经典的 BM25 算法提供相关性排名搜索。它与 PostgreSQL 深度集成，支持多种语言、并行构建和性能优化配置。对于需要在 PostgreSQL 中实现高效全文搜索的应用来说，这是一个值得考虑的选择。

**项目状态**: v1.0.0 - 生产就绪

**GitHub 地址**: https://github.com/timescale/pg_textsearch
