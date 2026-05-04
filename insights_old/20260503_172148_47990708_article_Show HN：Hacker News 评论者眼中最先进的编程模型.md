# HN SOTA — 编码模型流行度洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://hnup.date/hn-sota |
| **主题** | Hacker News 评论中编码模型的流行度与用户情感分析 |
| **数据来源** | Hacker News API + Gemini LLM 分析 |
| **更新频率** | 每日更新 |
| **聚合周期** | 10天滚动聚合 (2026/4/23 至 2026/5/1) |
| **生成日期** | 2026-05-03 |

---

## 概述

HN SOTA（Hacker News State of the Art）是一个追踪 AI 辅助编程领域最新发展的项目，通过捕捉 Hacker News 评论中编码模型的流行度和用户情感来保持对最新动态的了解。该项目每日运行，从 Hacker News 的热门帖子中提取关于 LLM 和编程相关讨论的用户观点。

> "The space of AI-assisted coding is evolving rapidly. This is an attempt at staying up to date with the latest developments, by capturing popularity and user sentiment of coding models from Hacker News comments."

---

## 数据采集方法

### 每日 Pipeline

该项目每天执行以下步骤：

1. **获取热门帖子**: 从 Hacker News API 获取过去 24 小时内最热门的 200 篇帖子
2. **帖子筛选**: 使用 LLM 筛选出标题与 LLM 或编程相关的帖子（最多 50 篇），因为这些帖子更可能包含相关讨论
3. **模型识别与情感分析**: 对每篇帖子的标题和评论发送到 Gemini 模型，识别 OpenRouter 模型列表中的模型，并评估每条评论对每个模型的情感倾向

### 数据可审计性

项目作者希望能够审计过程和结果，因此所有结果都记录到 Google Sheets 中，用户可以看到评论 ID、提到的具体模型以及模型对该评论和模型判定的情感。这为调试和对模型输出进行偶尔的 sanity check 提供了能力。

---

## 数据展示

### Top 10 模型流行度

该页面展示了过去 10 天（2026/4/23 至 2026/5/1）的：
- **总提及次数**: 模型被提及的总次数
- **用户情感**: 用户对每个模型的情感倾向（正面/中性/负面）

显示方式采用百分比条形图，刻度至 100%。

### 链接资源

- **Google Sheets 链接**: [详细结果表格](https://docs.google.com/spreadsheets/d/1MH68pNTKq_HJN79d81DdNtC9ZCMjt6E4BnhcHaQ-QiU/edit?usp=sharing) - 包含粒度化的分析结果，可查看具体评论 ID 提到的模型和判定的情感

---

## 核心洞察

1. **社区驱动的模型评价**: 该项目通过聚合 Hacker News 开发者社区的真实讨论，提供了不同于官方排行榜的模型受欢迎程度视角

2. **情感分析的价值**: 除了统计提及次数，还分析用户情感倾向，帮助了解特定模型在开发者社区中的口碑

3. **数据透明**: 所有原始数据（评论 ID、模型、情感判定）都公开可查，提高了结果的可信度

4. **每日更新**: 持续追踪趋势变化，而非一次性快照

---

## 使用说明

### 查看具体评论

用户可以通过在 URL `https://news.ycombinator.com/item?id=` 后面追加评论 ID 来打开原始评论。

### 数据校验

由于使用 LLM 进行模型识别和情感分析，可能存在误差。用户可以通过 Google Sheets 中的评论 ID 自行验证分析结果的准确性。

---

## 技术栈

| 组件 | 用途 |
|------|------|
| Hacker News API | 获取热门帖子数据 |
| LLM (Gemini) | 帖子筛选、模型识别、情感分析 |
| Google Sheets | 数据存储与展示 |

---

## 结论

HN SOTA 项目提供了一种独特的视角来了解 AI 编码工具在开发者社区中的实际使用情况和受欢迎程度。通过每日更新和情感分析，开发者可以追踪这一快速变化领域的最新趋势，同时通过公开的原始数据保证了一定的透明度。

该项目的价值在于：
- 聚合真实开发者的使用体验和观点
- 提供情感倾向分析，不仅看数量也看质量
- 数据可追溯可验证
- 持续跟踪而非一次性分析

---

*洞察报告由自动化工具生成*
*数据来源: https://hnup.date/hn-sota*