# RaTeX 洞察报告

**洞察链接**: https://ratex.lites.dev/

**基本信息和简介**

- **项目名称**: RaTeX (Rust TeX)
- **项目性质**: 开源数学排版引擎
- **核心功能**: 使用 Rust 编写的 LaTeX 数学解析和排版引擎，可输出用于 CoreGraphics、Skia、Canvas 2D 等矢量后端的扁平显示列表
- **技术特点**: 通过原生 FFI 和 WebAssembly 提供相同输出，支持跨平台部署（移动端到 Web）
- **许可证**: MIT
- **GitHub**: https://github.com/erweixin/RaTeX

---

## 核心特性

### 1. 与 KaTeX 对齐
- CI 运行大规模金标测试套件，通过像素差异与参考图像比对
- 在该测试集上输出与 KaTeX 广泛可比
- 提供支持表可与 KaTeX 并排对比

### 2. 跨平台部署
- **Web (WASM)**: 通过 npm 安装 ratex-wasm
- **iOS (Swift)**: 通过 SPM 安装
- **Android (Kotlin)**: 通过 Maven (io.github.erweixin:ratex-android)
- **Flutter (Dart FFI)**: 通过 pub.dev 安装 ratex_flutter
- **React Native**: 通过 npm 安装 ratex-react-native
- **Server / CLI**: 支持服务器端 PNG 生成和命令行工具

### 3. 化学和单位支持
- 内置 `\ce` (mhchem 化学公式)
- 内置 `\pu` (物理单位/ siunitx 风格)
- 支持反应箭头和物理单位，与普通数学在同一管道中处理

---

## 技术架构

### Rust 核心
- 单一排版引擎，热路径无 GC
- 可预测的计时性能，适合移动端 UI、服务器和 CI 栅格化测试

### 内存安全
- 使用 Rust 的内存安全特性
- 显示列表模型确保可预测的内存使用

### 多种后端支持
- C ABI 用于 Swift、Kotlin、Dart 等语言绑定
- WASM 用于 Web
- tiny-skia 或自定义栅格化器

---

## 性能对比

### Web 栈对比

| 特性 | RaTeX | KaTeX (web) | MathJax |
|------|-------|-------------|---------|
| 运行时 | Pure Rust | JavaScript + DOM | JavaScript + DOM |
| 移动端 | Native / WASM | WebView | WebView |
| 离线 | 是 | 取决于配置 | 取决于配置 |
| JS bundle 大小 | ~0 kB (核心是 WASM) | ~280 kB | ~500 kB |
| 内存模型 | 可预测 | GC / heap | GC / heap |

### 与原生数学 SDK 对比

| 特性 | RaTeX | swiftMath | flutter_math | iosMath |
|------|-------|----------|--------------|--------|
| mhchem \ce (化学) | ✓ | ✗ | ✗ | ✗ |
| \pu / siunitx 风格单位 | ✓ | ✗ | ✗ | ✗ |
| 同一引擎: 原生 FFI + WASM | ✓ | ✗ | ✗ | ✗ |
| 移动端 + 桌面端来自单一 Rust 核心 | ✓ | ✗ | ✗ | ✗ |
| Rust 排版核心 (可预测热路径) | ✓ | ✗ | ✗ | ✗ |

---

## 使用场景

### 何时选择 RaTeX

1. **原生或服务器部署**: 在 iOS、Android、Flutter 或 Rust 服务上使用相同排版（PNG/SVG 风格栅格化），无需捆绑浏览器

2. **WASM 在宿主中运行**: 在 WebAssembly 中运行核心并使用 Canvas 绘制，可与 KaTeX 在实时演示中对比输出

3. **化学和单位**: 使用 `\ce` / `\pu` 在 mhchem 风格路径上处理，反应箭头和物理单位与普通数学在同一管道中

### 为什么不是 WebView 栈

在浏览器中，KaTeX 和 MathJax 通常作为 JavaScript 对 DOM 操作。对于通过 WebView 嵌入数学的应用壳，仍需加载浏览器栈。RaTeX 将排版和栅格化放在 Rust 中，为希望避免该路径的宿主提供解决方案。

---

## 演示和文档

- **在线演示**: https://ratex.lites.dev/demo.html
- **数学画廊**: https://ratex.lites.dev/math.html
- **化学画廊**: https://ratex.lites.dev/chemistry.html
- **物理画廊**: https://ratex.lites.dev/physics.html
- **支持表**: https://ratex.lites.dev/demo/support-table.html
- **中文文档**: https://ratex.lites.dev/zh.html

---

## 总结

RaTeX 是一个 Rust 编写的 TeX 质量数学排版引擎，旨在为原生应用、服务器和无 WebView 的嵌入场景提供与 KaTeX 相当但更轻量的解决方案。其核心优势包括：

1. **跨平台一致性**: 单一 Rust 核心支持 Web (WASM)、iOS、Android、Flutter、React Native
2. **化学支持**: 内置 mhchem 风格的 `\ce` 和 `\pu` 宏
3. **性能优势**: 无 GC、热路径可预测、极小的 bundle 大小
4. **与 KaTeX 对齐**: 通过金标测试确保输出质量

对于需要在非浏览器环境中渲染 LaTeX 数学的开发者和团队，RaTeX 是一个值得考虑的现代替代方案。