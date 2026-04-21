# 洞察报告

**链接：** https://news.ycombinator.com/item?id=47823460
**来源：** Hacker News
**标题：** Show HN: Prompt-to-Excalidraw demo with Gemma 4 E2B in the browser (3.1GB)
**发布时间：** 2026-04-19 (~1天前)
**得分：** 137 points
**评论数：** 50 comments

---

## 项目概述

这是一个在浏览器中运行的 Prompt-to-Excalidraw 演示，使用 Gemma 4 E2B（4E2B是Gemma 4的Edge/浏览器优化版本）在WebGPU上运行，模型大小为3.1GB。用户可以通过输入文字提示生成Excalidraw图表。

## 关键技术特点

1. **LLM输出优化**：LLM输出约50个token的紧凑代码，而非约5000个token的原始Excalidraw JSON
2. **WebGPU运行**：利用浏览器WebGPU直接在本地运行模型
3. **Drawmode集成**：使用另一个名为Drawmode的项目来解释和执行绘图指令

## 讨论热点

### 1. 图表生成的现状与挑战

- **Mermaid vs SVG**：有用户指出Mermaid图表渲染较为粗糙，SVG格式可获得更好的视觉效果并支持动画
- **技术图表用TikZ**：有人倾向于使用TikZ生成技术/架构图，效果优于Mermaid但仍需多次迭代修复箭头和布局问题
- **"Pelican测试"**：讨论中提及LLM生成图像的困难，如"画一只骑自行车的鹈鹕"对LLM来说非常困难，而生成图表相对容易

### 2. 浏览器端AI模型的进展

- **本地模型表现**：有用户报告在Pixel 10 Pro上使用E2B进行本地研究，获得的结果与Gemini或ChatGPT相当
- **移动端优化**：浏览器端小模型与服务器端优化目标不同，浏览器限制为batch size 1，内核启动开销和内存带宽成为瓶颈
- **Gemma有望成为Chrome内置AI**：有建议认为Gemma应取代Gemini Nano成为Chrome内置AI

### 3. 浏览器_cache和CDN问题

- **重复下载**：用户抱怨多个浏览器WebAssembly演示需要重复下载相同模型
- **浏览器缓存隔离**：现代浏览器按origin分区缓存，不同域名下载相同模型会被重复存储
- **安全考量**：这是出于安全考虑的安全特性，可防止恶意网站通过缓存检查用户访问历史

### 4. 技术兼容性问题

- **Firefox支持**：目前不支持Firefox，因为Firefox尚未支持subgroupShuffleXor扩展，所有matmul/softmax内核都依赖此功能
- **GPU要求**：有用户报告在桌面Chrome 147和GTX 1060 (6GB RAM)上遇到"Unsupported browser/GPU"错误

## 相关资源

- 演示地址：https://teamchong.github.io/turboquant-wasm/draw.html
- Drawmode项目：https://github.com/teamchong/drawmode
- 视频演示：https://github.com/user-attachments/assets/71ae6e5c-a5ec-4d09-9de5-cf67ff42edfb