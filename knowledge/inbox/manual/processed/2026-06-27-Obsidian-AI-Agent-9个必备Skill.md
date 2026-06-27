---
title: 为 Obsidian 配置 AI Agent——9 个必备 Skill 详解
date: 2026-06-27
created: 2026-06-27
updated: 2026-06-27
tags:
  - Obsidian
  - Skill
  - AI Coding
  - Tool/Obsidian
  - 知识管理
aliases:
  - Obsidian 9 个 skill
  - Obsidian AI Agent 配置
  - obsidian-canvas-creator
  - mermaid-visualizer
  - excalidraw-diagram
  - kepano json-canvas
source_type: B站视频字幕
source_title: 如何为 Obsidian 配置 AI Agent？9 个必备 Skill 详解与安装指南
source_url: https://www.bilibili.com/video/BV14d9TBiE1S/
source_bvid: BV14d9TBiE1S
source_uploader: Jason (B站 ID，待核)
source_duration: ~18 分钟
source_subtitle_format: SRT
source_subtitle_path: /Users/zhangshuo12/Documents/b站视频字幕/如何为 Obsidian 配置 AI Agent？9 个必备 Skill 详解与安装指南_哔哩哔哩_bilibili_BV14d9TBiE1S_P字幕.srt
source_subtitle_lines: 2108
extracted:
  by: Mavis
  from: Mavis 会话 mvs_d353643a746446ffadd2e97a12bee17a
  method: 阅读 SRT 全文 → 结构化总结
  date: 2026-06-27
  skill_used: obsidian-markdown
status: processed
---

# 为 Obsidian 配置 AI Agent——9 个必备 Skill 详解

> [!info] 内容来源
> 本笔记由 ==Mavis== 阅读以下 SRT 字幕后整理：
> - **视频**：《如何为 Obsidian 配置 AI Agent？9 个必备 Skill 详解与安装指南》BV14d9TBiE1S
> - **字幕**：SRT（2108 行）
> - **整理时间**：2026-06-27
> - **整理方式**：全文阅读 → 结构化总结（非全文转录，原始 SRT 不动）
> - **整理使用的 skill**：`obsidian-markdown`（已 load）

> [!quote] 一句话理解
> Obsidian 用户配 AI Agent 的 skill 鱼龙混杂。这 9 个 skill 分 4 组：==**kepano 官方的 5 个（其中 1 个该淘汰、1 个该扩展）**==、==**Axton 的 3 个画图 skill（替代 kepano json-canvas）**==、==**2 个深度学习 skill（tutor 重点推荐）**==，外加 ==**2 个 Obsidian 智能体插件**==。OpenClaw 自带的 obsidian skill ==是坑==，不要用。

---

> [!tip] 视觉导览
> ![[2026-06-27-Obsidian-AI-Agent-9个必备Skill.canvas|800]]

## 全景速览

| 组               | Skill                     | 推荐度                 |
| --------------- | ------------------------- | ------------------- |
| **kepano 官方**   | `defuddle`                | ✅==核心必备==           |
|                 | `obsidian-cli`            | ✅==核心必备==           |
|                 | `obsidian-bases`          | ✅==独一无二，必须装==       |
|                 | `obsidian-markdown`       | ⚠️==建议扩展==          |
|                 | `json-canvas`（kepano）     | ❌ ==淘汰，用 Axton 替代== |
| **Axton 画图**    | `obsidian-canvas-creator` | ✅==节点布局好==          |
|                 | `mermaid-visualizer`      | ✅==结构清晰==           |
|                 | `excalidraw-diagram`      | ✅==头脑风暴首选==         |
| **深度学习**        | `tutor`                   | ==✅重点推荐==           |
|                 | `scholar`                 | ⚠️==慎用，token 杀手==   |
| **Obsidian 插件** | `Claudian`                | Claude Code 集成      |
|                 | `obsidian-agent-client`   | 更通用，多 agent 支持      |

---

## 第一组：kepano 官方的 5 个 skill

> [!tip] 仓库：https://github.com/kepano/obsidian-skills
> Obsidian CEO Steph（网名 kepano，Minimal 主题作者）发布。`defuddle` 同时是官方 Web Clipper 剪藏插件的底层。

### 1. `defuddle` —— ✅ 核心必备

> 网页/YouTube 剪藏工具，把任何 URL 转成干净 Markdown。

- Obsidian 官方 Web Clipper 插件**就基于 defuddle**
- **最近更新支持 YouTube 链接** —— 用 YouTube 官方 API，**比 yt-dlp 稳定、不用配 cookie**
- 安装：`npm install -g defuddle`
- 用法：发链接给 agent → 自动转 markdown 笔记；或 CLI 直接跑

### 2. `obsidian-cli` —— ✅ 核心必备

> Obsidian 官方命令行工具，全面拥抱 AI Agent。

- **命令行对 agent 最高效且省 token**
- 跟 [[2026-06-26-headroom-LLM-context压缩]] 是天然搭档 —— CLI + context 压缩 = token 极致省钱
- 需要 Obsidian ≥ 1.12.4

### 3. `obsidian-bases` —— ✅ 独一无二，必须装

> 教 agent 编写/修改 `.base` YAML 数据库文件。

- 简单 base 手动创建即可，**复杂 base 必须靠这个 skill**
- ==配合 `obsidian-cli`==：让 agent 理解你的整个知识库架构 → 生成复杂数据看板
- 对特定笔记的追踪依赖它

### 4. `obsidian-markdown` —— ⚠️ 建议扩展

> 给 agent 写 Obsidian 笔记的 SOP（标准作业程序）。

- **价值：防止 AI "向通用标准坍缩"**——即使训练数据里有 Obsidian 语法，agent 仍倾向输出标准 Markdown，写专属语法时还可能幻觉
- ==是所有 Obsidian 笔记撰写的**唯一入口 skill**==
- ==建议扩展成你自己的格式偏好==
- 优势：内容全部是 markdown，**没编程基础也能扩展**
- 视频作者举例："禁止 H1 标题（小红书/公众号排版不好看）" + "禁止嵌套列表"

### 5. `json-canvas`（kepano 版）—— ❌ 淘汰

> 教 agent 画 canvas 白板，但**布局不理想**（节点大小、文字溢出问题）。

- ==换 Axton 版替代==（下面）

---

## 第二组：Axton 的 3 个画图 skill（替代 kepano json-canvas）

> [!note] Axton 是著名 AI 博主，自媒体 ID "回到Axton"。Axton 的 canvas skill **针对节点坐标和布局做了非常好的优化**。

### Axton 版 `obsidian-canvas-creator` —— ✅ 替代 kepano

- 同样的提示词生成 3 张 canvas 对比：
  - Gemini 网页版直接出：节点大小计算不对，需手动调
  - kepano skill 在 OpenCode 用免费模型：干净整洁但仍有瑕疵
  - **Axton skill：所有节点大小合适，图表非常美观**

### Axton 版 `mermaid-visualizer` —— ✅ 结构清晰

- 代码生成的 mermaid **结构清晰 + 样式美观 + 成功率几乎 100%**，几乎不需要手动调试

### Axton 版 `excalidraw-diagram` —— ✅ 头脑风暴首选

- **Obsidian 插件市场下载量第一的插件**
- ==免费模型也能生成美观的图==
- 视频作者用 Axton 这个 skill 生成了视频里的所有结构图

---

## 第三组：深度知识学习 skill

### `tutor` —— ✅✅ 重点推荐

> ==能把任何文档资料转成完整的 Obsidian 知识库==（含双链、MOC、标签、考试训练）。

**输入**：把学习资料（PDF 等）放到 `resources/` 文件夹
**输出**：agent 在 `StudyVault/` 创建一个完整知识库
**两条命令**：
- `/tutor-setup` —— 建库
- `/tutor` —— 生成测试题

**支持场景**：
- 知识文档 / 资料学习
- **GitHub 代码项目分析**（视频作者说"这是我最喜欢的功能"）
- 考试 / 面试准备（自动生成专项训练笔记）
- 学习仪表板：可视化你对每个领域的掌握程度

### `scholar` —— ⚠️ 慎用

> 学术科研专用，重度工作流编排系统。

- **深度依赖 OpenClaw（大龙虾）**
- 三级阅读体系；L3 深度阅读是**长生命周期异步任务，耗费几小时**
- 整个科研模拟循环
- ==**非常消耗 token**==
- 依赖 `obsidian-direct` skill（本质是 I/O 文件操作，也耗 token）
- ==日常优先用 tutor，需要学术研究再用 scholar==

---

## 第四组：Obsidian 智能体插件

### `Claudian`

> Obsidian 集成 Claude Code 的插件。

- 安装：BRAT 插件加 `https://github.com/YishenTu/claudian` 或手动下载 3 个核心文件
- 个性化设置里填**称呼**
- 被 Anthropic 限制的地区需填**环境变量**转接兼容模型

### `obsidian-agent-client` —— 更通用

> 支持主流 agent 的 Obsidian 客户端。

- **支持**：Claude Code / Codex / Gemini CLI / OpenCode / 通义千问 code
- 安装：BRAT 加 `https://github.com/RAIT-09/obsidian-agent-client`（待核）
- 配置：custom agent 添加 → 设 agent ID / display name / **path**（用 `where opencode` 查）/ arguments
- arguments 第 3 行是**你的 Obsidian 知识库路径**

> [!tip] 选哪个
> 视频原话"两个插件都是机器人图标，根据需求选一个"。Claudian 跟 Claude Code 集成深；agent-client 更通用。

---

## 🔴 OpenClaw 的 obsidian skill —— 这是个坑

> [!warning] 视频明确警告
> **OpenClaw（大龙虾）官方 GitHub 仓库里的 obsidian skill 不推荐使用**——它不是用 Obsidian 官方 CLI，而是用 2023 年的一个开源项目（现已改名 **notesmd-cli**）。

**问题清单**：
- 描述说"用 Obsidian CLI"，实际**不是**
- 作者已经把项目改名 notesmd-cli 来跟 Obsidian 官方 CLI 区分
- 本质是 ==**直接操作 Markdown 文件（I/O）**==，==巨耗 token==
- ==**不触发 Obsidian 内部机制**==
- 唯一优势：不需要 Obsidian 客户端
- **但很难想象**：部署 OpenClaw 的机器有 vault 但没装 Obsidian

**正确做法**：
- OpenClaw 用户去 ==**ClawHub 官方市场**==找 ==**obsidian CLI 这个官方 skill**==
- 不要用 OpenClaw 自带的

---

## 开放性思考题：MOC vs Bases

> [!question] 视频留下的问题
> Agent 自动生成的 Obsidian vault 有精美的 MOC 内容地图。
>
> **MOC 里到底应该手动编写关联笔记并设置双链，还是应该用 base 数据库？**
> **问题核心**：用 base 数据库 ==**不会产生双链**==，那 MOC 的意义何在？

这是一个真问题。视频没给答案，我也没标准答案，但提供几个思考角度：

1. **MOC 的价值不只是双链**：MOC 本身是结构化目录、双向导航的载体
2. **Bases 的价值在动态查询**：filter / formula / summary 是 MOC 做不到的
3. **两者可能互补**：MOC 管静态知识结构，Bases 管动态数据视图

---

## 跟你当前 vault 状态的对照

> [!tip] 你（用户）已经装了哪些、还建议加哪些

### 已装 ✅

你前两天已把 kepano 5 个 skill 装到 `~/.agents/skills/`：
- ✅ `defuddle`
- ✅ `obsidian-cli`
- ✅ `obsidian-bases`
- ✅ `obsidian-markdown`
- ✅ `json-canvas`（kepano 版，但视频建议**淘汰**换 Axton 版）

### 建议加 🔧

- ❓ **`obsidian-canvas-creator`** —— 替代 kepano 版，节点布局更好
- ❓ **`mermaid-visualizer`** —— 结构清晰、几乎不需调试
- ❓ **`excalidraw-diagram`** —— 头脑风暴首选，Obsidian 插件市场下载量第一
- ❓ **`tutor`** —— ==重点推荐==，跟你的知识库工作流契合
- ❓ **Claudian 或 obsidian-agent-client** —— Obsidian 内直接调 agent
- ❓ **`obsidian-markdown` 扩展**：把你 ai-knowledge-vault 的格式偏好写进去（你已经在用了 obsidian-markdown 风格）

---

## 相关笔记

- [[2026-06-26-headroom-LLM-context压缩]] — obsidian-cli 跟 Headroom 是天然搭档
- [[2026-04-23-Claude-多智能体协作5种模式]] — agent 协作模式
- [[2026-06-24-Claude-Code-动态工作流]] — Claude Code Workflow
- [[2026-03-10-Claude-Code实战指南]] — Claude Code 基础

> **脚注**：关于 Axton 三个 skill 的具体仓库地址，原视频字幕里没明确给出（SRT 没有 URL 段）。如果你要装，建议先搜 "Axton json-canvas skill" 或在 ClawHub / skills.sh 上找。

[^axton]: Axton 是从视频中提到的著名 AI 博主，自媒体 ID "回到Axton"，但具体仓库 URL 没在 SRT 里。

---

## SRT 原文摘录（关键段）

> [!quote] ① 关于 obsidian CLI 的价值
> "**命令行对于智能体来说，是最高效且节省 token 的方式**"

> [!quote] ② 关于 OpenClaw 的坑
> "**这个 skill 不是一个好的选择**——它虽然描述中说它使用的是 obsidian CLI，那实际上它使用的是 ==一个 2023 年的开源项目==……==会耗费大量 token==，而且它也==不像 obsidian CLI 那样触发 obsidian 的内部机制=="

> [!quote] ③ 关于 obsidian-markdown 扩展性
> "我之所以在表格中给这个 skill 加了叹号，是我**更建议大家对这个 skill 进行扩展**……因为这个 skill 是撰写 Obsidian 笔记的==**唯一入口 skill**=="

> [!quote] ④ 关于 scholar 的 token 消耗
> "它深度依赖 OpenClaw 大龙虾……L3 级别的深度阅读是一个==**长生命周期的异步编排任务，会耗费几个小时**==……==**它非常的消耗 token**=="