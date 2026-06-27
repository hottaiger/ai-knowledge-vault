# 知识库索引

> 最后更新：2026-06-28 | 条目数：33 | 概念页数：14
> 检索入口：优先从概念页进入，再按需打开具体知识条目的 `原始内容`。

## 概念导航

### 第二大脑
- [[concepts/第二大脑|第二大脑]]（关联 1 条，别名：Second Brain, 知识操作系统）
- 聚焦主题：把知识、任务与产出沉淀为可被 AI 长期读取、更新和复用的文件系统，而不是一次性聊天记录。

### 上下文工程
- [[concepts/上下文工程|上下文工程]]（关联 7 条，别名：Context Engineering, 上下文管理）
- 聚焦主题：围绕选择、组织、压缩和治理上下文的系统化方法，目标是让模型在有限窗口内读到最对的材料。

### Agent记忆架构
- [[concepts/Agent记忆架构|Agent记忆架构]]（关联 2 条，别名：Agent Memory, Memory Architecture）
- 聚焦主题：把当前上下文、长期事实、近期脉络和历史案例拆成不同层级，分别用匹配的存储与读取策略管理。

### Claude Code Skills
- [[concepts/Claude Code Skills|Claude Code Skills]]（关联 9 条，别名：Skills, Agent Skills）
- 聚焦主题：把可复用工作流封装成目录化技能，连同脚本、素材和约束一起交给 Agent 按需执行。

### Obsidian知识管理
- [[concepts/Obsidian知识管理|Obsidian知识管理]]（关联 3 条，别名：Obsidian PKM, Vault Workflow）
- 聚焦主题：以 Obsidian 作为本地优先的知识底座，用链接、属性和视图把静态 Markdown 组织成可导航的知识网络。

### Claude协作工作流
- [[concepts/Claude协作工作流|Claude协作工作流]]（关联 21 条，别名：Claude Cowork, AI 协作）
- 聚焦主题：把 Claude 从一次性问答工具变成长期协作者，关键在于先提供稳定上下文，再围绕任务持续迭代。

### OpenClaw与Agent编排
- [[concepts/OpenClaw与Agent编排|OpenClaw与Agent编排]]（关联 6 条，别名：OpenClaw, Agent Orchestration）
- 聚焦主题：围绕多 Agent 的连接、调度和执行，把上下文、工具和外部系统编排成可持续运转的自动化流程。

### cc-connect
- [[concepts/cc-connect|cc-connect]]（关联 2 条，别名：cc-connect IM 桥, Cursor Agent 飞书桥接）
- 聚焦主题：cc-connect 作为本地中间层进程，桥接飞书/钉钉等 IM 与 Cursor Agent CLI，让 IM 里直接驱动 AI 编码 Agent。

### Codex提示词工作流
- [[concepts/Codex提示词工作流|Codex提示词工作流]]（关联 4 条，别名：Codex Workflow, Codex 自动化）
- 聚焦主题：围绕 Codex 会话记录与 Chronicle，回顾重复工作流并封装成 Skill / Subagent / Automation 的提示词模式与方法论。

### OpenSpec规范驱动
- [[concepts/OpenSpec规范驱动|OpenSpec规范驱动]]（关联 3 条，别名：OpenSpec, Comet）
- 聚焦主题：OpenSpec / Comet / SpecDrive 一套：把提案、设计、任务、归档和验证写成可追溯的规范文件，配合 human-in-the-loop 门控推进 change。

### AI协作框架
- [[concepts/AI协作框架|AI协作框架]]（关联 3 条，别名：AI 协作边界, AI 协作心智模型）
- 聚焦主题：判断 AI 介入任务程度的框架（🟢🟡🟠🔴 四档），以及 AI 是'失忆的实习生'等协作心智模型与实践规则（AI 使用日志、单测补审）。

### 可视化工具链
- [[concepts/可视化工具链|可视化工具链]]（关联 1 条，别名：Visualization Stack, 图表生成）
- 聚焦主题：围绕架构图、流程图、时序图、思维导图等可视化的工具选型与自动生成 skill 集合。

### MCP工具链
- [[concepts/MCP工具链|MCP工具链]]（关联 4 条，别名：Model Context Protocol）
- 聚焦主题：Model Context Protocol 标准化 AI 调用外部工具/数据源。GitNexus、Playwright、Sequential Thinking、Figma MCP 等都基于 MCP 暴露能力。

### 代码知识图谱
- [[concepts/代码知识图谱|代码知识图谱]]（关联 4 条，别名：Code Knowledge Graph, GitNexus）
- 聚焦主题：把代码仓库解析为图（节点=符号，边=调用/导入/继承），让 AI 编码 Agent 通过查询替代 grep。代表工具：GitNexus。

## 最近收录

| 日期 | 标题 | 来源 | 关联概念 | 摘要 |
|------|------|------|----------|------|
| 2026-06-27 | 大模型能力边界与 AI 项目落地 | 未知来源 | Claude协作工作流 | （待当前Agent提炼） |
| 2026-06-27 | 为 Obsidian 配置 AI Agent——9 个必备 Skill 详解 | 未知来源 | Claude Code Skills, Obsidian知识管理 | （待当前Agent提炼） |
| 2026-06-26 | keep-codex-fast —— Codex 安全本地维护 skill | 未知来源 | Claude Code Skills, Codex提示词工作流 | （待当前Agent提炼） |
| 2026-06-26 | Headroom —— LLM Context 压缩层 | 未知来源 | 上下文工程 | （待当前Agent提炼） |
| 2026-06-24 | Claude Code 动态工作流（Dynamic Workflows） | 未知来源 | Claude协作工作流 | （待当前Agent提炼） |
| 2026-06-22 | cc-connect 安装配置避坑指南（实战 9 条） | 实战联调笔记（macOS 15 + Node 20 + cc-connect v1.3.4 + Cursor Agent CLI + 飞书） | OpenClaw与Agent编排, cc-connect | 飞书权限名照抄搜不到：INSTALL.md 把事件标识（im.message.receive_v1，带点）和权限标识（带冒号）混了。搜权限要去 docs/feishu.md 第四章拿 5 个正确列表；事件订阅单独去「事件与回调」菜单加。 |
| 2026-06-20 | cc-connect 工作原理 | 实战联调对话整理 | 上下文工程, Claude协作工作流, OpenClaw与Agent编排, cc-connect | 三层架构：cc-connect（中间层，本地进程）跟飞书 IM 保持 WebSocket 长连接；维护用户↔会话映射（~/.cc-connect/sessions/）；做权限检查（allow_from / admin_from）；把消息拼成完整 prompt，spawn 子进程喂给 Cursor Agent CLI；监听 JSON stream 输出实时回传… |
| 2026-06-20 | "AI 角球" 视频引用——角球是 AI 友好的近封闭系统 | B 站视频引用 | Claude协作工作流, AI协作框架 | 角球 = 近封闭系统，AI 友好：开球位置固定（4 个角）、争顶区域限定（小禁区弧顶）、攻防人数固定、球路可枚举、有"充分时间"做事前规划——AI 强项。 |
| 2026-06-20 | AI 角球与 AI 协作边界 | Mavis 会话 mvs_d353643a746446ffadd2e97a12bee17a | 上下文工程, Claude协作工作流, AI协作框架 | "AI 角球"判断框架：约束越完整、契约越清晰、越接近"事前规划"而非"实时博弈"的任务，越适合交给 AI（文档/CRUD/单测/机械重构）。 |
| 2026-06-18 | 商详页项目的复杂性——规模数据 | 碎片速记 | 上下文工程, Obsidian知识管理, 代码知识图谱 | 规模基线：1000+ 源码文件、11w 行业务代码（不含 node_modules 等三方依赖）。 |
| 2026-06-17 | SpecDrive 怎么用——人工审批门控 + build_command 配置原理 | SpecDrive 使用问答整理 | Claude Code Skills, Claude协作工作流, OpenClaw与Agent编排, OpenSpec规范驱动 | 确认设计方案时只看 3 个文件：(1) openspec/changes/ /.specdrive/handoff/brainstorm-summary.md 看最终结论；(2) docs/superpowers/specs/ - -design.md 看完整设计；(3) openspec/changes/ /specs/ .md 看需求约束做"规则级"复核… |
| 2026-06-14 | 2026-06-14-用codex分析工作记录 | 未知来源 | Claude协作工作流, Codex提示词工作流 | Codex 工作流分析提示词：让 Codex 拉取最近 7 天提交记录，分析"都做了什么"——是回顾工作内容、识别重复工作流的基础。 |
| 2026-06-11 | 美国地址生成器 | 链接速记 | 待归类 | 用途：需要测试地址数据（电商/支付/物流/海外注册等场景）时的在线生成工具。 |
| 2026-06-10 | Codex 回顾提示词——识别可封装的重复工作流 | Codex 会话整理 | Claude Code Skills, Claude协作工作流, Codex提示词工作流 | 证据优先：先用 Codex 会话记录 + Codex Memories + Chronicle 三层证据定位模式，不要凭空臆测重复模式。 |
| 2026-06-05 | Effect of .gitignore on gitnexus skills | Cursor 会话导出 | Claude Code Skills, Claude协作工作流, 代码知识图谱 | （待当前Agent提炼） |
| 2026-06-03 | Comet 使用问答与实现原理 | detail-taro3 项目一次完整的 Comet 实战对话 | Claude协作工作流, OpenSpec规范驱动 | （待当前Agent提炼） |
| 2026-05-26 | OpenSpec 归档——change 内的 delta spec 与项目级主 spec 的区别 | Comet 使用问答整理 | Claude协作工作流, OpenSpec规范驱动 | archive 里的 spec = 某次 PR 的附带文档，存档用：记录的是"这次改动期间写的 delta spec"，跟着 change 一起归档。 |
| 2026-05-16 | 关于 AI Coding 我的一些总结 | 碎片速记 | AI协作框架 | Vibe Coding ↔ AI Coding 的区别：前者靠感觉，后者靠人审。 |
| 2026-05-04 | AI Workflow 在项目中的应用——提示词优化与项目记忆 | 碎片速记 | 上下文工程, Agent记忆架构, Claude协作工作流 | 提示词优化方向：尽量详尽地描述功能；上下文越详细准确，对模型推理能力要求越低；模糊的上下文会消耗更多 token。 |
| 2026-04-30 | drawio-skill — 从文本生成专业架构图（含自检迭代） | https://github.com/Agents365-ai/drawio-skill | Claude Code Skills, Claude协作工作流, OpenClaw与Agent编排, 可视化工具链 | 核心能力：根据自然语言生成 .drawio XML；用 draw.io 桌面版 CLI 导出 PNG/SVG/PDF/JPG；6 种图表类型预设（ERD、UML 类图、序列图、架构图、ML/DL、流程图）；动画连接线（数据流）；ML 张量形状标注 (B, C, H, W)；网格对齐（10px 倍数）；浏览器降级方案（CLI 不可用时生成 diagrams.n… |
| 2026-04-29 | 知识库构建参考链接——飞书 Wiki + 小红书视频 | 链接速记 | Claude协作工作流 | 全部是知识库/AI 工作流相关的视频和文章，待后续阅读整理——入库时保留为链接索引，避免直接丢弃原始 URL。 |
| 2026-04-28 | 2026-04-28-agentic-workflow实践 | 未知来源 | Claude协作工作流 | （待当前Agent提炼） |
| 2026-04-26 | Multiple coding agents. One UI to orchestrate them. | https://cline.bot/kanban | Claude协作工作流, OpenClaw与Agent编排 | （待当前Agent提炼） |
| 2026-04-26 | Figma MCP 缓存设计稿——get_design_context 返回结构解析 | Mavis 会话 + Figma MCP 实测 | 上下文工程, Claude协作工作流, MCP工具链 | 返回结构 = 代码 + 提示：get_design_context 返回 React 组件代码（含 data-node-id 注入），同时附带 4 条提示原文。代码不能直接用，提示信息很关键。 |
| 2026-04-02 | Superpowers - Cursor AI 开发框架 | https://cursor.com/cn/marketplace/superpowers | Claude协作工作流 |  |
| 2026-03-23 | Google 5种Agent设计模式 | https://x.com/GoogleCloudTech/status/2033953579824758855 | Claude Code Skills, MCP工具链 | 工具封装：解决"知识获取" |
| 2026-03-10 | Claude Code 实战指南 | Claude Code course notes | 第二大脑, 上下文工程, Agent记忆架构, Claude协作工作流, OpenClaw与Agent编排, MCP工具链, 代码知识图谱 | 工具使用系统 |
| 1970-01-01 | GitNexus - 代码知识图谱与 AI 编码工作流 | ['微信公众号汇总（2026-05-11）', '.claude/skills/gitnexus（6 个 SKILL.md）', 'Cursor 对话记录（2026-06-05）'] | Obsidian知识管理, Claude协作工作流, MCP工具链, 代码知识图谱 | （待当前Agent提炼） |
| 1970-01-01 | Codex 学习手册 — CodexGuide 核心要点 | 未知来源 | Codex提示词工作流 | （待当前Agent提炼） |
| 1970-01-01 | Claude 多智能体协作5种模式 | https://www.bilibili.com/video/BV19LdqBAErd | Claude协作工作流 | （待当前Agent提炼） |
| 1970-01-01 | 用AI的方式，重新打开飞书！【建议收藏】 | https://b23.tv/IW5guNY | 待归类 | （待当前Agent提炼） |
| 1970-01-01 | Vibe Security — AI 代码安全审计技能 | https://skills.sh/raroque/vibe-security-skill/vibe-security | Claude Code Skills | （待当前Agent提炼） |
| 1970-01-01 | Article-to-HTML — 文章转小红书信息图 Skill | https://github.com/Ceeon/openclaw-article-to-image | Claude Code Skills | （待当前Agent提炼） |
