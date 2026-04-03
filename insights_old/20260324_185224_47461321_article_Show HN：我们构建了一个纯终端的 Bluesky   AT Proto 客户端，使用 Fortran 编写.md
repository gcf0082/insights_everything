# 洞察报告：FormerLab/fortransky

## 基本信息

| 项目 | 内容 |
|------|------|
| **仓库名称** | FormerLab/fortransky |
| **仓库链接** | https://github.com/FormerLab/fortransky |
| **洞察时间** | 2026-03-24 18:52:24 |
| **主要语言** | Fortran (70.5%), Rust (17.3%), Python (7.8%), C (2.8%), CMake (1.1%), Shell (0.5%) |
| **创建时间** | 2026-03-19 |
| **贡献者数量** | 1 |
| **主题标签** | atprotocol, bluesky-client, c, cli, fortran, raw, rust, tui, vintage |

## 项目概述

Fortransky 是一款纯终端（Terminal-only）Bluesky / AT Protocol 客户端，完全使用 Fortran 编写。该项目包含一个 Rust 原生 firehose 解码器，用于处理 `relay-raw` 流路径的数据。这是一个非常有趣的项目，因为它使用了现代少见的 Fortran 编程语言来实现一个社交媒体客户端。

## 技术架构

### 核心组件

1. **Fortran TUI** (`src/`) - 终端用户界面
2. **C libcurl 桥接** (`cshim/`) - C 语言调用 libcurl
3. **Fortran iso_c_binding 模块** (`src/atproto/firehose_bridge.f90`) - Fortran 与 C 的互操作
4. **Rust staticlib** (`bridge/firehose-bridge/`) - Rust 静态库
5. **Python 辅助脚本** (`relay_raw_tail.py`) - 作为子进程启动

### 数据流

```
envelope → CAR → DAG-CBOR → NormalizedEvent → JSONL + firehose_bridge_cli binary
```

## 构建依赖

### 系统包 (Ubuntu/Debian)
- gfortran
- cmake
- pkg-config
- libcurl4-openssl-dev

### Rust 工具链
- 需要 rustc >= 1.70

### Python 依赖
- cbor2
- websockets

## 功能特性

### TUI 命令

| 命令 | 功能 |
|------|------|
| `l` | 登录并获取时间线 |
| `x` | 登出并清除保存的会话 |
| `a ` | 作者动态 |
| `s ` | 搜索帖子 |
| `p ` | 查看个人资料 |
| `n` | 通知 |
| `c` | 发布帖子 |

### 连接模式

1. **默认模式** - 使用合成测试数据（fixture）
2. **relay-raw 模式** - 连接到原始 AT Protocol relay (`com.atproto.sync.subscribeRepos`)，处理二进制 CBOR over WebSocket
3. **实时模式** - 设置 `FORTRANSKY_RELAY_FIXTURE=0` 启用

## 版本历史

- **v1.1** - 集成了原生 Rust firehose 解码器，`relay_raw_tail.py` 优先使用 `firehose_bridge_cli`，JWT 字段长度增加到 1024 以适应完整的 AT Protocol 令牌

## 已知问题与限制

1. JSON 解析器是手写的轻量级实现，非完整的模式驱动解析器
2. `relay-raw` 仅显示 `app.bsky.feed.post` 创建操作，其他集合在规范化阶段被过滤
3. 流视图显示原始 DID，未实现 DID → handle 解析
4. TUI 是基于行的（输入命令后按 Enter），不是原始按键交互

## 会话管理

会话状态保存在 `~/.fortransky/session.json`，重启后自动恢复。登录时需要使用应用专用密码（app password），而非主密码。

## 总结

Fortransky 是一个极具创意的项目，展示了 Fortran 这种老牌编程语言在现代网络应用中的潜力。该项目成功地将 Fortran 与 Rust、Python 结合，构建了一个功能完整的 Bluesky 终端客户端。对于对复古技术和跨语言集成感兴趣的开发者来说，这是一个值得关注的项目。