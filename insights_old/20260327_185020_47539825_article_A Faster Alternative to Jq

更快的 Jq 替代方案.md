# 洞察报告

## 基本信息

- **链接**: https://micahkepe.com/blog/jsongrep/
- **标题**: jsongrep is faster than {jq, jmespath, jsonpath-rust, jql}
- **作者**: Micah Kepe
- **发布日期**: 2026-02-28
- **阅读时间**: 17分钟

---

## 核心发现

### jsongrep 是什么

jsongrep 是一个基于 DFA（确定性有限自动机）的 JSON 查询工具，使用 Rust 编写。其核心特点是**将查询编译为 DFA**，在搜索时只需单次遍历 JSON 树，每个转换都是 O(1) 的表查找。

### 性能优势

在大规模数据集（约 190 MB）上，jsongrep 的端到端搜索性能远超其他 JSON 查询工具，包括 jq、jmespath、jsonpath-rust 和 jql。

---

## 技术原理

### DFA 查询引擎

jsongrep 的搜索管道包含五个阶段：

1. **JSON 解析**：使用 serde_json_borrow 实现零拷贝解析
2. **查询解析**：将查询字符串解析为 AST
3. **NFA 构造**：使用 Glushkov 算法构建无 ε 转移的 NFA
4. **DFA 确定化**：通过子集构造将 NFA 转换为 DFA
5. **搜索**：使用 DFS 遍历 JSON 树，每个边执行 DFA 转换

### 关键优势

- **正则语言**：查询语言是keys和indices字母表上的正则语言
- **单次遍历**：JSON 树每个节点最多访问一次
- **无回溯**：无需解释执行，无递归栈
- **剪枝优化**：对于不匹配的分支，直接跳过整个子树

---

## 查询语法

| 功能 | 语法 | 示例 |
|------|------|------|
| 点路径 | `.` | `roommates[0].name` |
| 通配符 | `*` / `[*]` | `favorite_drinks[*]` |
| 或运算 | `\|` | `name \| roommates` |
| 递归下降 | `(* \| [*])*` | `(* \| [*])*.name` |
| 可选 | `?` | `favorite_food?` |

---

## 基准测试结果

### 测试数据集

- **small**: 106 B（手工艺最小文档）
- **medium**: ~992 KB（Kubernetes API 模式定义）
- **large**: ~7.6 MB（Kestra OpenAPI 规范）
- **xlarge**: ~190 MB（San Francisco GeoJSON 地块）

### 性能对比（xlarge 数据集）

1. **文档解析时间**：serde_json_borrow 最快
2. **查询编译时间**：jsongrep 较慢（这是为快速搜索付出的代价）
3. **搜索时间**：jsongrep 远超其他工具
4. **端到端搜索**：jsongrep 遥遥领先

---

## 局限性

- 不如 jq 普及
- 查询语言表达能力有限（仅支持搜索，不支持转换）
- 是新工具，未经过生产环境验证

---

## 链接资源

- GitHub: https://github.com/micahkepe/jsongrep
- Crates.io: https://crates.io/crates/jsongrep
- 基准测试结果: https://micahkepe.com/jsongrep/report/index.html
