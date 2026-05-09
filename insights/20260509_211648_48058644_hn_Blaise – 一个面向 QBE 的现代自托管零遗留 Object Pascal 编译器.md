# 洞察报告：Blaise – 现代自托管 Object Pascal 编译器

## 基本信息

- **链接**: https://news.ycombinator.com/item?id=48058644
- **发布日期**: 2026-05-08
- **项目**: Blaise Pascal Compiler
- **GitHub**: https://github.com/graemeg/blaise
- **作者**: Graeme Geldenhuys (fpGUI 库的作者)
- **许可证**: Apache License 2.0

## 项目概述

Blaise 是一个为 2020 年代打造的现代自托管 Object Pascal 编译器，零遗留负担（zero-legacy），完整支持自动引用计数（ARC），统一 UTF-8 编码。

## 核心特性

### 设计目标
- 消除数十年积累的遗留负担
- 优先考虑开发者生产力、内存安全和高性能执行
- 针对原生代码，通过 QBE 后端（未来支持 LLVM）

### 主要特点
1. **单一语言模式**: 无 `{$mode}` 切换，无传统方言支持
2. **单一字符串类型**: UTF-8 引用计数字符串，二进制数据使用 `RawBytes`
3. **统一内存模型**: 字符串、类和接口统一采用自动引用计数
4. **无 GUID 接口**: 接口调度通过编译时 vtable 映射实现
5. **具体化泛型**: 编译时单态化，无类型擦除
6. **现代构建系统**: 使用 PasBuild 的 `project.xml`，无需 Makefile
7. **一流调试器支持**: OPDF 作为默认调试格式

## 项目状态

| 阶段 | 目标 | 状态 |
|------|------|------|
| 1 | Bootstrap 流水线 – Hello World on Linux x86_64 | ✅ 完成 |
| 2 | 类型系统 – 类、记录、ARC、异常 | ✅ 完成 |
| 3 | 泛型 + 无 GUID 接口 | ✅ 完成 |
| 4 | OPDF 调试信息生成 | ✅ 完成 |
| 5 | 自托管 + LLVM + Windows + macOS ARM64 | 进行中 |
| 6 | LSP + VS Code 扩展 | 计划中 |
| 7 | FPC/Delphi 代码迁移分析器 | 计划中 |

### 当前指标
- **自托管**: 是的，Blaise 已经实现自托管，仅用 7 天达成
- **测试**: 1200+ 测试且持续增长（从第一天起采用 TDD）
- **后端**: 当前使用 QBE 后端，LLVM 后端正在积极开发中
- **星标**: 122+ stars

## 自托管亮点

Blaise 在仅 7 天内实现自托管，意味着它能够编译自身，且字节级完全匹配引导版本：
- Stage 1: 初始 FPC 编译的二进制文件
- Stage 2: Stage 1 编译编译器代码生成的二进制文件
- Stage 3: Stage 2 再次编译编译器代码
- 自托管: Stage 2 和 Stage 3 字节级完全相同

## 社区反馈

Hacker News 社区对项目表示积极态度：
- 作者 Graeme Geldenhuys 是 fpGUI 库（自 2010 年存在）的作者，在 FreePascal 和 Pascal 语言方面拥有丰富经验
- 项目基础扎实
- 核心架构仍在最终确定中，暂不接受代码贡献
- 欢迎通过 GitHub Discussions 反馈语言设计、语法选择和未来方向

## 被移除的传统 Pascal 特性

| 特性 | 移除原因 |
|------|----------|
| ShortString, AnsiString, WideString, UnicodeString | 替换为单一的 UTF-8 引用计数 string 类型 |
| with 语句 | 导致难以诊断的符号解析 bug；破坏静态分析 |
| 旧式 object 类型 | 使用 record（栈/值）或 class（堆/引用）替代 |
| COM 风格接口 GUID | 接口调度通过编译时 vtable；GUID 是不必要的复杂性 |
| 多种语言模式 | 一种方言，精心维护，胜过五种方言维护不善 |
| assign, reset, rewrite, blockread | 替换为基于流的 I/O RTL |
| TObject vs TInterfacedObject 分离 | 统一类模型下自动引用计数；[Weak] 破坏循环 |

## 构建要求

- Free Pascal Compiler 3.2.2 或更高版本
- PasBuild
- C 编译器（gcc 或 clang）
- GNU ld 或 lld（Linux）；ld（macOS）

## 总结

Blaise 代表了 Object Pascal 编译器的现代化尝试，通过消除 decades 的遗留负担，提供一个更简洁、更安全的现代编程环境。该项目由经验丰富的 Pascal 社区成员开发，采用自底向上的方法重新设计语言和编译器，目前已实现自托管并拥有丰富的测试套件。