# Blaise Pascal 编译器洞察报告

**洞察链接**: https://github.com/graemeg/blaise

**基本信息**:
- **项目名称**: Blaise Pascal Compiler
- **作者**: Graeme Geldenhuys (graemeg)
- **星标数**: 122
- **语言**: Pascal (94.0%), Python (2.7%), C (2.3%)
- **许可证**: Apache License v2.0 with Runtime Library Exception
- **提交数**: 205 commits
- **分支/标签**: master 分支, 5 个发布标签

---

## 项目概述

Blaise 是一个现代化的 Object Pascal 编译器，从零开始构建，旨在消除传统 Pascal 语言数十年的历史包袱。它优先考虑开发者生产力、内存安全和高效执行。

## 核心设计理念

### 统一与简洁
- **单一语言模式**: 无需 `{$mode}` 切换，无传统方言支持
- **单一字符串类型**: UTF-8 引用计数字符串，二进制数据用 `RawBytes`
- **统一内存模型**: 自动引用计数(ARC)统一应用于字符串、类和接口

### 现代化特性
- **无 GUID 接口**: 接口调度通过编译时 vtable 映射实现
- **具体化泛型**: 编译时单态化，无类型擦除
- **现代构建系统**: 使用 PasBuild 和 `project.xml`，无需 makefile
- **一流调试器**: 默认使用 OPDF 调试格式

## 技术状态

| 阶段 | 目标 | 状态 |
|------|------|------|
| 1 | Bootstrap 流水线 (Linux x86_64 Hello World) | ✅ 完成 |
| 2 | 类型系统 (类、记录、ARC、异常) | ✅ 完成 |
| 3 | 泛型 + 无 GUID 接口 | ✅ 完成 |
| 4 | OPDF 调试信息输出 | ✅ 完成 |
| 5 | 自托管 + LLVM + Windows + macOS ARM64 | 🔄 进行中 |
| 6 | LSP + VS Code 扩展 | 📋 计划中 |
| 7 | FPC/Delphi 代码迁移分析器 | 📋 计划中 |

## 项目结构

```
project.xml                       根聚合器
├── compiler/                     编译器 (application)
│   └── src/
│       ├── main/pascal/          uLexer, uParser, uAST, uCodeGenQBE
│       └── test/pascal/          FPTest 测试套件
├── rtl/                          运行时库 (library)
│   └── src/
│       ├── main/pascal/          System.pas, SysUtils.pas, Classes.pas
│       └── test/pascal/
├── tools/
│   └── migration-analyser/       FPC/Delphi 迁移工具
├── vendor/qbe/                   QBE 后端源码
└── docs/                         设计文档
```

## 移除的传统特性

| 特性 | 替代方案 |
|------|---------|
| ShortString, AnsiString, WideString, UnicodeString | 单一 UTF-8 引用计数 string |
| with 语句 | 移除（导致符号解析问题） |
| 旧式 object 类型 | 使用 record 或 class |
| COM 风格接口 GUID | 编译时 vtable 接口调度 |
| 多语言模式 | 单一方言 |
| assign, reset, rewrite, blockread | 流式 I/O RTL |
| TObject vs TInterfacedObject 分裂 | 统一 ARC 类模型 |

## 构建与测试

### 前提条件
- Free Pascal Compiler 3.2.2+
- PasBuild
- C 编译器 (gcc/clang)
- GNU ld 或 lld

### 构建命令
```bash
# 构建所有模块
pasbuild compile

# 调试/发布配置
pasbuild compile -p debug
pasbuild compile -p release

# 运行测试
pasbuild test

# 单独构建编译器模块
pasbuild compile -m blaise-compiler
```

## 关键亮点

1. **自托管能力**: Blaise 能够编译自身，字节级精确匹配
2. **测试驱动开发**: 1200+ 测试用例
3. **QBE 后端**: 当前使用 QBE 编译器作为后端
4. **LLVM 后端**: 正在开发中
5. **跨平台**: 支持 Linux、计划支持 Windows 和 macOS ARM64

## 社区与贡献

项目核心架构仍在最终确定中，暂不接受代码贡献。欢迎通过 GitHub Discussions 提供关于语言设计、语法选择和未来方向的反馈。

---

*报告生成时间: 2026-05-09*