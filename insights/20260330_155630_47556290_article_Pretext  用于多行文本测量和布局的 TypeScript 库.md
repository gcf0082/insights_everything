# Pretext 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://github.com/chenglou/pretext |
| **项目名称** | Pretext |
| **作者** | chenglou |
| **Stars** | 14.6k |
| **Forks** | 540 |
| **Watchers** | 52 |
| **许可证** | MIT |
| **主要语言** | TypeScript (89.4%), HTML (10.6%) |
| **提交数** | 264次 |

## 项目简介

Pretext 是一个纯 JavaScript/TypeScript 编写的多行文本测量与布局库。它快速、准确，支持所有你能想象到的语言，包括表情符号和混合双向文本（Bidi）。它允许渲染到 DOM、Canvas、SVG，并且即将支持服务端渲染。

## 核心特性

### 1. 避免 DOM 测量

Pretext 绕过了对 DOM 测量的需求（例如 `getBoundingClientRect`、`offsetHeight`），这些操作会触发布局回流（layout reflow），这是浏览器中最昂贵的操作之一。Pretext 实现了自己的文本测量逻辑，使用浏览器的字体引擎作为真实参考（ground truth），这是一种非常适合 AI 驱动迭代的方法。

### 2. 两种主要使用场景

#### 场景一：无需触碰 DOM 测量段落高度

```typescript
import { prepare, layout } from '@chenglou/pretext'

const prepared = prepare('AGI 春天到了. بدأت الرحلة 🚀', '16px Inter')
const { height, lineCount } = layout(prepared, textWidth, 20) // 纯算术，无 DOM 布局和回流！
```

- `prepare()` 执行一次性工作：规范化空白字符、分段文本、应用粘合规则、使用 Canvas 测量分段，返回一个不透明的处理句柄。
- `layout()` 是之后的轻量热路径：对缓存的宽度进行纯算术运算。
- 支持所有语言，包括表情符号和混合双向文本，并处理特定的浏览器 quirks。

#### 场景二：手动布局段落线条

- `layoutWithLines()`：在固定宽度下获取所有行
- `walkLineRanges()`：获取线条宽度和光标，而不构建文本字符串
- `layoutNextLine()`：允许在宽度变化时逐行路由文本

### 3. 性能数据

根据当前已检入的基准测试快照：
- `prepare()` 对 500 个文本批次约需 **19ms**
- `layout()` 对同一批次约需 **0.09ms**

### 4. 应用场景

- 虚拟化/遮挡：无需估算和缓存
-  fancy 用户布局：masonry、JS 驱动的类 flexbox 实现等
- 开发时验证：AI 辅助验证按钮等标签是否溢出到下一行，无需浏览器
- 防止布局偏移：当新文本加载并需要重新锚定滚动位置时

## API 概览

| API | 描述 |
|-----|------|
| `prepare(text, font, options?)` | 一次性文本分析和测量，返回传递给 layout() 的不透明值 |
| `layout(prepared, maxWidth, lineHeight)` | 给定最大宽度和行高，计算文本高度 |
| `prepareWithSegments(text, font, options?)` | 与 prepare() 类似，但返回更丰富的结构用于手动行布局 |
| `layoutWithLines(prepared, maxWidth, lineHeight)` | 手动布局的高级 API |
| `walkLineRanges(prepared, maxWidth, onLine)` | 手动布局的低级 API |
| `layoutNextLine(prepared, start, maxWidth)` | 迭代器式 API，每行宽度不同时使用 |
| `clearCache()` | 清除内部缓存 |
| `setLocale(locale?)` | 设置locale |

## 限制与注意事项

- 目前针对常见的文本设置：
  - `white-space: normal`
  - `word-break: normal`
  - `overflow-wrap: break-word`
  - `line-break: auto`
- `system-ui` 在 macOS 上用于 `layout()` 精度不安全，请使用命名字体
- 由于默认目标包含 `overflow-wrap: break-word`，非常窄的宽度仍可能在字素（grapheme）边界内断词

## 技术架构

Pretext 的设计受到 Sebastian Markbage 的 `text-layout` 启发：
- 使用 Canvas `measureText` 进行 shaping
- 使用 pdf.js 的 bidi 算法
- 流式行中断

## 总结

Pretext 是一个创新性的文本布局库，通过在 JavaScript 层实现文本测量逻辑，有效避免了浏览器布局回流的性能开销。它提供了高精度、快速的多行文本测量能力，支持各种语言和特殊字符，非常适合需要高性能文本布局的前端应用场景。
