# 洞察报告：honker

## 基本信息

- **洞察链接**：https://github.com/russellromney/honker
- **项目名称**：honker
- **项目描述**：SQLite extension + bindings for Postgres NOTIFY/LISTEN semantics with durable queues, streams, pub/sub, and scheduler
- ** stars**：300
- ** forks**：5
- ** License**：Apache-2.0
- ** 主要语言**：Python 70.9%, Rust 28.0%, Makefile 1.1%
- ** 贡献者**：russellromney, Claude

## 项目概述

honker 是一个 SQLite 扩展库， 为 SQLite 带来了 PostgreSQL 风格的 NOTIFY/LISTEN 语义。它内置了持久的发布/订阅、任务队列和事件流功能，无需客户端轮询或守护进程/代理。任何可以通过 `SELECT load_extension('honker')` 加载扩展的语言都能使用相同的功能。

honker 通过替换轮询间隔为 SQLite WAL 文件上的事件通知来实现推送语义，实现了跨进程的个位数毫秒级通知传递。

## 核心特性

### 1. 跨进程通知/监听
- 支持跨进程的通知和监听
- 使用 WAL 文件的 stat(2) 轮询实现亚毫秒级唤醒

### 2. 工作队列
- 持久化至少一次(at-least-once)工作队列
- 支持重试、优先级、延迟任务
- 失败任务自动移入死信表

### 3. 原子事务
- 任何入队操作都可以与业务写入原子化
- 事务一起提交或一起回滚

### 4. 流处理
- 持久化发布/订阅
- 每个消费者独立偏移量追踪
- 可配置刷新间隔

### 5. 临时通知
- 临时发布/订阅
- 不保留历史记录

### 6. 调度器
- Crontab 风格周期性任务
- Leader-elected 调度器

### 7. 其他特性
- 任务超时处理
- 指数退避重试
- 命名锁
- 速率限制
- 任务结果存储

## 支持的语言

- Python (`honker`)
- Node.js (`@russellthehippo/honker-node`)
- Bun (`@russellthehippo/honker-bun`)
- Ruby (`honker`)
- Go
- Elixir
- C++
- Rust (`honker-core`/`honker-extension`)

## 架构设计

### WAL 模式优先

honker 要求数据库启用 WAL 模式。WAL 模式允许读写并发，避免了 DELETE/TRUNCATE 模式下写作者获取排他锁导致的阻塞问题。

### 唤醒路径

- 每个数据库一个 stat(2) 线程，每毫秒轮询一次 `.db-wal` 文件
- (size, mtime) 变化时向每个订阅者的有界通道发送信号
- 每个订阅者运行 SELECT 查询获取新行

### 队列模式

- `_honker_live` 表存储待处理和处理中的任务
- 声明部分索引 `(queue, priority DESC, run_at, id) WHERE state IN ('pending','processing')`
- Claim = UPDATE ... RETURNING
- Ack = DELETE

### 事务耦合

- notify()、queue.enqueue()、stream.publish() 都在调用者的事务中执行
- 这是事务性发件箱模式的默认实现

## 性能

在现代笔记本电脑上能处理数千条消息每秒，跨进程唤醒延迟由 1ms stat 轮询周期限制（在 M 系列芯片上约 1-2ms 中位数）。

## 使用场景

1. 需要在 SQLite 中实现任务队列
2. 需要跨进程事件通知
3. 需要持久化事件流
4. 需要定时任务调度
5. 不想引入 Redis + Celery 的额外基础设施

## 代码结构

```
honker-core/              # Rust rlib，所有 bindings 共享
honker-extension/         # SQLite 可加载扩展
packages/
  honker/                 # Python 包
  honker-node/            # Node.js 绑定
  honker-rs/              # Rust 封装
  honker-go/              # Go 绑定
  honker-ruby/            # Ruby 绑定
  honker-bun/             # Bun 绑定
  honker-ex/              # Elixir 绑定
  honker-cpp/             # C++ 绑定
tests/                    # 集成测试
bench/                    # 性能测试
site/                     # 文档网站
```

## 快速开始示���

### Python 队列

```python
import honker
db = honker.open("app.db")
emails = db.queue("emails")

# 入队
emails.enqueue({"to": "alice@example.com"})

# 原子操作：业务写入 + 入队
with db.transaction() as tx:
    tx.execute("INSERT INTO orders (user_id) VALUES (?)", [42])
    emails.enqueue({"to": "alice@example.com"}, tx=tx)

# worker 认领任务
async for job in emails.claim("worker-1"):
    send(job.payload)
    job.ack()
```

### Python 流

```python
stream = db.stream("user-events")

# 发布事件
with db.transaction() as tx:
    tx.execute("UPDATE users SET name=? WHERE id=?", [name, uid])
    stream.publish({"user_id": uid, "change": "name"}, tx=tx)

# 订阅事件
async for event in stream.subscribe(consumer="dashboard"):
    await push_to_browser(event)
```

### SQLite 扩展

```sql
.load ./libhonker_ext
SELECT honker_bootstrap();
INSERT INTO _honker_live (queue, payload) VALUES ('emails', '{"to":"alice"}');
SELECT honker_claim_batch('emails', 'worker-1', 32, 300);
SELECT honker_ack_batch('[1,2,3]', 'worker-1');
```

## 局限性

1. 仅支持 WAL 模式（单主机、单写作者）
2. 不支持任务管道/链/组/和弦
3. 不支持多写作者复制
4. 不支持 DAG 工作流编排
5. 不支持跨 NFS 共享文件

## 竞品对比

- **pg_notify**：PostgreSQL 原生，快的触发器，无重试/可见性
- **Huey**：SQLite 后端 Python 任务队列
- **pg-boss**：PostgreSQL 任务队列
- **Oban**：Elixir/Phoenix 任务队列

## 总结

honker 是一个创新的 SQLite 扩展，为 SQLite带来了 PostgreSQL 风格的异步通知和任务队列功能。对于已经使用 SQLite 作为主要数据存储的应用，honker 提供了一个轻量级的解决方案，避免了引入 Redis + Celery 的额外基础设施开销。其核心优势在于：
1. 原子事务支持
2. 个位数毫秒级跨进程响应
3. 多语言绑定支持
4. 简单部署（单文件扩展）

实验性项目，API 可能会有变化。