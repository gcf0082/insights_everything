# 洞察报告：cook — AI 代理编排 CLI 工具

**洞察链接：** https://rjcorwin.github.io/cook/  
**生成时间：** 2026-03-24  
**来源：** GitHub Pages

---

## 基本信息

**cook** 是一个简单的 CLI 工具，用于编排 Claude Code、Codex 和 OpenCode 等 AI 代理。它通过简洁的语法实现复杂的工作流程，包括循环、版本竞速和并行执行等功能。

---

## 核心功能

### 1. 工作单元（Work）
一个提示词等于一次代理调用，这是 cook 的核心单元。

### 2. 循环操作符
- **repeat (xN)**：顺序执行 N 次
- **review**：添加审查→门控循环，支持自定义审查提示和最大迭代次数
- **ralph**：任务列表进度管理包装器

### 3. 组合操作符
- **versions (vN / race N)**：并行运行 N 个相同的 cook，通过 pick 解析器选择最佳结果
- **vs**：并行运行两个不同的 cook，可使用 merge 合成结果或 compare 生成对比文档

---

## 安装方式

```bash
npm install -g @let-it-cook/cli
```

为 Claude Code 添加 cook 技能：
```bash
mkdir -p .claude/skills && \
cp -r $(npm root -g)/@let-it-cook/cli/skill .claude/skills/cook
```

---

## 使用示例

```bash
# 审查循环
cook "Implement dark mode" review

# 3 次执行
cook "Implement dark mode" x3

# 竞速 3 个版本，选择最佳
cook "Implement dark mode" v3 "least code"

# 两种方案选其一
cook "Auth with JWT" vs "Auth with sessions" pick "best security"
```

---

## 配置说明

运行 `cook init` 初始化项目配置，创建以下文件：
- `COOK.md` — 项目指令和代理提示模板
- `.cook/config.json` — 代理/模型/沙盒默认配置
- `.cook/Dockerfile` — Docker 沙盒模式依赖
- `.cook/logs/` — 会话日志

### 沙盒模式
- **Agent 模式（默认）**：代理使用自身 OS 级沙盒
- **Docker 模式**：代理在受限网络访问的 Docker 容器中运行

---

## 主要特性

1. **速率限制恢复**：自动等待并重试因配额或速率限制而失败的请求
2. **自定义代理**：支持为不同步骤指定不同代理或模型
3. **解析器**：pick（选择最佳）、merge（合成结果）、compare（生成对比文档）
4. **Git 工作树隔离**：并行执行在隔离的 git worktree 中运行

---

## 相关链接

- [GitHub](https://github.com/rjcorwin/cook)
- [npm](https://www.npmjs.com/package/@let-it-cook/cli)
