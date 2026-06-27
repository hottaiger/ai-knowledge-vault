---
title: Claude Code 动态工作流（Dynamic Workflows）
date: 2026-06-24
created: 2026-06-24
updated: 2026-06-24
tags:
  - Claude Code
  - AI Coding
  - Agent/工作流
  - 工具/Claude-Code
aliases:
  - Dynamic Workflows
  - Claude Code Workflow
  - 长程任务规划
source_type: 视频字幕
source_title: 🧲 Claude Code 工作流：长程任务的规划和执行利器 ⛓️
source_url: https://www.bilibili.com/video/BV12ojm64EU6/
source_bvid: BV12ojm64EU6
source_uploader: 沧海九素
source_duration: ~18 分钟
source_subtitle_format: SRT
source_subtitle_path: /Users/zhangshuo12/Documents/b站视频字幕/🧲 Claude Code 工作流：长程任务的规划和执行利器 ⛓️_哔哩哔哩_bilibili_BV12ojm64EU6_字幕.srt
source_subtitle_lines: 2404
source_requirements: Claude Code ≥ 2.1.154
extracted:
  by: Mavis
  from: Mavis 会话 mvs_d353643a746446ffadd2e97a12bee17a
  method: 阅读 SRT 全文 → 结构化总结
  date: 2026-06-24
status: processed
---

# Claude Code 动态工作流（Dynamic Workflows）

> [!info] 内容来源
> 本笔记由 ==Mavis== 阅读以下 SRT 字幕后整理：
> - **视频**：《🧲 Claude Code 工作流：长程任务的规划和执行利器 ⛓️》BV12ojm64EU6
> - **主讲**：沧海九素
> - **时长**：~18 分钟
> - **字幕**：SRT（2404 行，文件路径见 frontmatter）
> - **整理时间**：2026-06-24
> - **整理方式**：全文阅读 → 结构化总结（非全文转录，原始 SRT 不动）
>
> 下方"==SRT 原文摘录=="区块保留了几段关键原文，方便溯源核对。

> [!quote] 一句话理解
> **Workflow 是一段 JS 脚本**——Claude Code 据此动态编排子智能体，形成有固定主线、可视化、可保存复用的多步执行流程。

---

## 核心要点

> [!important] 本质
> - Workflow 的载体 ==JS 脚本==，存在 `~/.claude/projects/` 下（命名形如 `WORKFLOW_GRAPTS-*.js`）
> - 脚本里约定了：**有哪些步骤**、**每个步骤是什么子智能体**、**串行/并行关系**、**每步的提示词、工具、输出格式**
> - 子智能体本身的定义现在普遍用 ==JSON spec== 描述（提示词、可用工具、目标、输出格式）

> [!tip] 三大启动方式
> 1. **关键词 `ultra code`** —— 触发当次进入 workflow 模式（UI 闪亮）
> 2. **自然语言** —— `create a workflow to ...`，自动搜索已有 workflow 复用，没有则新建
> 3. **直接跑内置命令 `/deep research`** —— 最快的体验入口（依赖 web search 工具）
> 4. **强制模式** —— effort 调到 ==ultra code==（爆闪图标），默认 x-high thinking + workflow

---

## Workflow vs Agent Teams

> [!important] 关键差异
> - **Workflow**：固定主线流程、清晰可见、step-by-step、可保存为 slash command
> - **Agent Teams**：松散组织、主管调度、可与子智能体对话、灵活但难以强约束

| 维度 | Workflow | Agent Teams |
|---|---|---|
| 流程定义 | JS 脚本预编写 | 主管 agent 动态分配 |
| 可见性 | 步骤列表 + 状态一屏可见 | 需要逐个问子智能体 |
| 主线 | 固定主线（步骤串行） | 松散，主管协调 |
| 并行 | 步骤内可显式并行 | 自然涌现并行 |
| 复用 | ==`save` 存为 slash command== | 不可直接复用 |
| 灵活度 | 中（脚本能定多复杂就能多复杂） | 高（自由但难约束） |

---

## 4 种多步执行方式（Claude Code 全家桶）

| 方式 | 定位 | 适合 |
|---|---|---|
| **Skills** | 经验 SOP、最稳 | 重复性高、流程成熟的活 |
| **子智能体**（Subagent） | 隔离上下文、轻量并行 | 单一子任务 |
| **Agent Teams** | 自由组织、灵活调度 | 探索性、需对话子智能体 |
| **Workflow** | 固定主线、可视化 | 流程可枚举、要看得清 |

> [!tip] 选用原则
> 子智能体并发量不大、==希望掌控每个环节==、有固化节奏的流程 → ==首选 workflow==
> 探索性强、需要临时改意图 → Agent Teams

### 4 种方式定位图

![[2026-06-24-Claude-Code-动态工作流-4种方式定位.png|800]]

### 典型 workflow 的执行结构（以 deep research 为例）

![[2026-06-24-Claude-Code-动态工作流-workflow执行流程.png|800]]

> [!note] 看这张图的关键
> - ==主线串行==：Scope → Research → Fetch → Verify → Output
> - ==步骤内并行==：Research 步骤下挂 5 个 Search 子 agent 同时跑
> - 这就是视频里讲的"主线串行 + 步骤内并行"的标准模式

### 选用决策树

![[2026-06-24-Claude-Code-动态工作流-vs-agent-teams.png|800]]

---

## 适用场景

- ✅ ==全库扫描 bug==（地毯式、有节奏推进）
- ✅ 文件迁移
- ✅ 交叉检验
- ✅ 任何"流程固化、节奏清晰"的多步任务

## 不适用

- ❌ 流程不可枚举、需要临场判断的
- ❌ 子智能体并发量超大、token 燃烧过猛的（容易卡死/上下文溢出）

---

## 限制

> [!warning] 硬限制
> - 最多 ==并发 16 个 agent==
> - 单次运行 agent ==上限 1000 个==
> - 流程很复杂时 ==token 消耗大==，容易卡死或上下文溢出

**实操建议**：流程别开太复杂，特别是长链路。普通流程、子智能体并发不大的最合适。

---

## 操作命令

| 命令 | 干啥 |
|---|---|
| `save` | 把当前 workflow 脚本保存为 ==slash command==（最大亮点） |
| 暂停 / 停止 / 重启 | 流程控制 |
| config `disabled workflow: true` | 强制关闭 workflow 功能 |

---

## UI 体验细节

- 状态栏出现 `workflow` 标签 → 方向键下钻到 workflow 视图
- workflow 输出 ==不阻塞主对话==，可继续别的对话
- 视图左侧列出 5 步（举例），每步对应一个子 agent
- 进入步骤可看：**模型 / 提示词 / 当前状态 / 输出**
- 键盘控制：J/K 上下、Enter 进入、回退、暂停

> [!note] 中断恢复
> - 强制退出 session 后再回来，workflow 定义会被自动找回
> - 但 ==之前的工作状态会丢失==，从已完成的步骤开始重新执行
> - 至少 workflow 脚本本身不用重写

---

## 怎么打印出脚本（debug/学习用）

> [!warning] 提问技巧
> 直接问"给我看下你的原始脚本"——Claude 只会告诉你大致逻辑，==不会全打印==。必须明确说：
> **"请打印出**==原始**==JS 脚本"**（强调 raw）

---

## 相关笔记

- [[Claude-Code实战指南]] — Claude Code 基础用法
- [[Claude-多智能体协作5种模式]] — Agent Teams 等多 agent 协作模式
- [[在VS-Code中使用Claude-Code]] — IDE 集成

---

## SRT 原文摘录

> [!quote] ① 关于 workflow 本质
> "**dynamic workflow 的本质是写一段 JS 的脚本**……所以呢你是可以在 cloud 的 project 目录下面找到这个脚本的……在脚本里面就约定了我们有哪些过程……然后我们每一步要去做什么事，这些全部都是在脚本里面约定的啊"
>
> —— SRT 段 242-256

> [!quote] ② 关于 save 功能
> "**他的 save 指的是我把我们现在写的这个 workflow 的脚本存下来，变成一个新的命令……存的就是上面这个脚本啊……你就可以把它当成一个 SLASH COMMAND，一个斜杠命令去用了**。所以这点设计其实是非常的好"
>
> —— SRT 段 291-303

> [!quote] ③ 关于 hard 限制
> "最多并发 ==16 个 agent==，然后单次运行的 agent 的上限是 ==1000 个==……这种情况下就意味着大量的 token 消耗……workflow 很容易被卡死，包括很容易上下文溢出都会有问题的"
>
> —— SRT 段 556-572

> [!quote] ④ 关于打印脚本的提问技巧
> "注意请提示一下 raw，或者说提示一下原始的，否则他会……读取完之后帮你解析……解析完之后告诉你一个大致的逻辑，然后**不会帮你全部打印出来**"
>
> —— SRT 段 228-235

> [!quote] ⑤ 关于 Workflow vs Agent Teams
> "agent teams 它你可以理解成就是一组智能体……他是一个相对来说**非常松散的组织**……而在 workflow 里面你看的非常的清楚……它就是一有固定的一个流程啊"
>
> —— SRT 段 89-113
