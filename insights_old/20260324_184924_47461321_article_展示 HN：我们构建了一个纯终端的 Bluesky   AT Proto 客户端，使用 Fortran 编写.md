# 洞察报告：Fortransky

## 基本信息

- **洞察链接**: https://github.com/FormerLab/fortransky
- **项目名称**: Fortransky
- **项目描述**: 一个纯终端的 Bluesky / AT Protocol 客户端，使用 Fortran 编写
- **星标数**: 43
- **主要语言**: Fortran (68.5%), Rust (14.5%), Python (11.7%), C (3.6%)
- **更新日期**: 2026-03-24

---

## 项目概述

Fortransky 是一个极具创新性的开源项目，它证明了 Fortran 这种古老的编程语言在现代网络应用开发中仍然具有强大的生命力。该项目是一个纯终端（Terminal-only）的 Bluesky / AT Protocol 客户端，提供了完整的功能支持，包括时间线浏览、发帖、互动（点赞、转发、引用）、图像发布以及实时流处理等。

### 核心特性

1. **纯 Fortran 实现**: 核心 TUI（终端用户界面）完全使用 Fortran 编写，通过 iso_c_binding 模块与 C/Rust 进行互操作
2. **原生 Firehose 解码器**: 集成了 Rust 和 x86-64 汇编编写的原生 firehose 解码器，支持 `relay-raw` 流的处理
3. **Floyd-Steinberg 抖动算法**: 实现了经典的 Bill Atkinson 抖动算法（1984年 MacPaint 使用的算法），用于图像处理和发布
4. **双流模式**: 支持 Jetstream（JSON原生）和 Relay-raw（CBOR二进制）两种流模式

---

## 技术架构

### 架构层次

```
Fortran TUI (src/)
  └─ C libcurl bridge (cshim/)
  └─ Fortran iso_c_binding 模块 (src/atproto/firehose_bridge.f90)
       └─ Rust staticlib (bridge/firehose-bridge/)
            envelope → CAR → DAG-CBOR → NormalizedEvent → JSONL
```

### 关键技术组件

1. **Fortran TUI**: 基于文本的用户界面，支持行命令输入
2. **C Shim**: 使用 C 编写的 libcurl 桥接层，用于 HTTP 请求
3. **Rust Firehose Bridge**: Rust 实现的静态库，处理 firehose 数据解码
4. **Assemblersky**: 可选的 x86-64 汇编实现的 AT Protocol 解码器，性能更优
5. **Python 依赖**: cbor2（CBOR解码）、websockets（WebSocket连接）、Pillow（图像处理）

### 数据流路径

- **relay-raw 流路径**: relay_raw_tail.py → assemblersky_cli / firehose_bridge_cli → Python cbor2
- **图像发布路径**: dither_prep.py → dither.f90 → pixels_to_png.py → uploadBlob → createRecord

---

## 功能特性

### TUI 命令

**首页视图:**
- `l` - 登录并获取时间线
- `x` - 登出并清除会话
- `a <handle>` - 作者动态
- `s <query>` - 搜索帖子
- `p <handle>` - 查看个人资料
- `n` - 通知
- `c` - 撰写帖子
- `d <imagepath>` - 抖动图像并发布
- `t <uri/url>` - 打开主题
- `j` - 流尾随
- `m` - 切换流模式
- `q` - 退出

**帖子列表视图:**
- `j/k` - 移动选择
- `n/p` - 下一页/上一页
- `o` - 打开选中主题
- `r` - 回复
- `l` - 点赞
- `R` - 转发
- `q` - 引用帖子
- `P` - 打开作者资料
- `b` - 返回首页

### 图像发布

`d` 命令可以将任何图像转换为灰度、调整为 576×720（MacPaint 原始画布尺寸）、进行 1-bit 抖动处理，然后发布到 Bluesky。

---

## 版本历史

| 版本 | 主要更新 |
|------|----------|
| v1.3 | Floyd-Steinberg 抖动 + 图像发布功能 |
| v1.2 | Assemblersky 集成，relay-raw 原生解码器支持 |
| v1.1 | Rust 原生 firehose 解码器集成 |
| v1.0 | 点赞、转发、引用帖子功能 |
| v0.9 | 类型解码层，更丰富的帖子语义 |
| v0.7 | C libcurl 桥接，会话保存，流模式切换 |

---

## 构建依赖

### 系统包（Ubuntu/Debian）
```
sudo apt install -y gfortran cmake pkg-config libcurl4-openssl-dev
```

### Rust 工具链
需要 rustc >= 1.70，通过 rustup 安装。

### Python 依赖
```
pip install cbor2 websockets Pillow
```

### 可选：Assemblersky
x86-64 汇编 AT Protocol firehose 解码器，可从 https://github.com/FormerLab/assemblersky 构建。

---

## 会话管理

- 会话保存在 `~/.fortransky/session.json`
- 使用应用密码登录（非主密码）
- 重新启动时自动恢复会话

---

## 已知问题与限制

1. JSON 解析器为手写轻量级实现，非完整模式驱动解析器
2. relay-raw 模式仅显示 `app.bsky.feed.post` 创建操作
3. 流视图显示原始 DID，可在可用时进行句柄解析
4. TUI 基于行命令输入（非原始按键）

---

## 项目亮点

1. **技术复古主义**: 使用 Fortran 和汇编等复古技术栈构建现代社交网络客户端
2. **创新架构**: 成功将 Fortran 与 Rust、C、Python 多语言集成
3. **性能优化**: 提供原生汇编解码器选项
4. **完整功能**: 尽管是终端应用，功能覆盖全面
5. **图像处理**: 实现了经典的 MacPaint 抖动算法

---

## 总结

Fortransky 是一个充满创意的技术项目，展示了 Fortran 在现代应用中的潜力。项目通过多语言互操作实现了完整的 Bluesky 客户端功能，包括实时流处理、图像发布等高级特性。其技术架构体现了深厚的系统编程功底，是技术怀旧与现代功能结合的典范。