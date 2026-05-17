# zerostack 1.0.0 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://www.ic.work/article/zerostack-1-0-0-rust-coding-agent |
| **包名称** | zerostack |
| **版本** | 1.0.0 |
| **发布平台** | crates.io |
| **发布时间** | 2026年5月17日 |
| **核心定位** | 纯 Rust、Unix 风格的轻量级 coding agent |

---

## 核心摘要

zerostack 1.0.0 已发布到 crates.io，可通过 `cargo install zerostack` 安装，定位是纯 Rust、Unix 风格的轻量级 coding agent。项目给出的性能数据：约 7k LoC、8.9MB 二进制、空会话约 8MB RAM、工作时约 12MB RAM。真正值得看的不是它能不能立刻替代 Claude Code、Cursor 或 opencode，而是它把资源占用、权限边界和终端工作流重新摆回了开发者面前。

---

## 一、产品定位

zerostack 的定位很清楚：纯 Rust 写的轻量级 Unix 风格 coding agent。

### 支持的模型提供商
- OpenRouter
- OpenAI
- Anthropic
- Gemini
- Ollama
- 自定义 provider

### 基础工具
- read
- write
- edit
- grep
- find_files
- list_dir

### 其他功能
- 终端 UI
- 会话恢复
- MCP
- Exa 搜索
- prompt 模式切换

---

## 二、关键指标对比

| 指标 | zerostack 官方数据 | 备注 |
|------|-------------------|------|
| 代码量 | 约 7k LoC | 项目自述 |
| 二进制大小 | 8.9MB | 项目自述 |
| 空会话内存 | 约 8MB RAM | 项目自述 |
| 工作时内存 | 约 12MB RAM | 项目自述 |
| 许可证 | GPL-3.0-only | - |

**注意**：以上数据来源于项目自述，不能作为独立评测结论。

---

## 三、权限系统——核心亮点

zerostack 最值得看的特性是权限系统，这是它与其他 AI 编程代理的主要差异化所在。

### 权限控制点

| 控制点 | 具体做法 | 影响对象 |
|--------|---------|---------|
| 权限模式 | restrictive、standard、accept-all、yolo 四档 | 个人开发者可按风险切换 |
| 工具权限 | 按工具和 glob 配置 | 团队可限制读写范围 |
| 会话控制 | session allowlist | 临时授权，不必永久放权 |
| Bash 执行 | permission gating | 跑命令前多一道刹车 |
| 命令隔离 | 可选 bubblewrap sandbox | 只针对 bash 命令隔离 |
| 循环防护 | doom-loop detection | 连续重复调用工具时提醒或拒绝 |

### doom-loop detection
连续相同工具调用 3 次以上，会触发警告或拒绝。这个设计承认了 agent 会犯傻，而且会机械地犯傻，比很多"全自动开发"的叙事诚实。

---

## 四、适用场景分析

### 适合人群
1. **个人开发者**：偏好 CLI、Rust、终端工作流、本地模型或多 provider 接入的人
2. **开源维护者**：仓库权限复杂，自动化冲动强，但维护成本有限
3. **小团队**：需要一个能限制读写、能分会话授权、能在循环调用时踩刹车的 agent

### 不适合场景
1. 替代 Claude Code、Cursor 或 opencode 的成熟替代品
2. 复杂项目的生产环境
3. 有闭源集成需求的企业（GPL-3.0-only 许可证限制）

---

## 五、实验性功能

以下功能明确标注为 experimental，不能按成熟功能卖：

- loop system
- git worktrees integration

---

## 六、优劣势总结

### 优势
- 轻量级二进制，资源占用低
- 多模型接入，避免单一入口锁定
- 权限系统完善，可控制权前置
- 纯 Rust 实现，无 JS 依赖
- Unix 风格，终端工作流

### 劣势
- 生态不够厚
- 团队维护不确定性
- IDE 集成有限
- 复杂项目体验未知
- GPL-3.0-only 对闭源集成有合规成本

---

## 七、结论与建议

### 结论
zerostack 代表了一种"反向减重"的趋势——AI 编程代理都在变重，zerostack 偏要变轻。这不是情怀，轻量是一种约束。约束越清楚，开发者才越敢把方向盘交出去一小段。

### 建议

**个人开发者**：可以试，成本不高，适合 CLI 用户。

**工程团队**：可以观望加小范围验证，先放非关键仓库，别急着迁移主流程。

**企业用户**：如果有闭源集成计划，先看 GPL-3.0-only 许可证合规问题，再谈采用。

### 后续关注点
1. 项目给出的低资源数据，在真实中大型仓库里是否还能站住
2. experimental 的 loop system 和 git worktrees integration 能否从演示能力变成稳定工作流
3. 权限系统能否在团队场景里持续好用

---

*报告生成时间：2026-05-17*
*来源：ic.work 洞察*