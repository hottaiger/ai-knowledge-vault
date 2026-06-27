---
concept: Codex提示词工作流
aliases:
  - Codex Workflow
  - Codex 自动化
---

# Codex提示词工作流

## 定义
围绕 Codex 会话记录与 Chronicle，回顾重复工作流并封装成 Skill / Subagent / Automation 的提示词模式与方法论。

## 关键洞见
- （待当前Agent提炼）（来源：[[2026-06-26-keep-codex-fast-Codex本地维护]])
- Codex 工作流分析提示词：让 Codex 拉取最近 7 天提交记录，分析"都做了什么"——是回顾工作内容、识别重复工作流的基础。（来源：[[2026-06-14-用codex分析工作记录]])
- 证据优先：先用 Codex 会话记录 + Codex Memories + Chronicle 三层证据定位模式，不要凭空臆测重复模式。（来源：[[2026-06-10-Codex回顾提示词]])
- （待当前Agent提炼）（来源：[[2026-05-29-Codex学习手册-CodexGuide]])

## 代表条目
- [[2026-06-26-keep-codex-fast-Codex本地维护|keep-codex-fast —— Codex 安全本地维护 skill]]
- [[2026-06-14-用codex分析工作记录|2026-06-14-用codex分析工作记录]]
- [[2026-06-10-Codex回顾提示词|Codex 回顾提示词——识别可封装的重复工作流]]
- [[2026-05-29-Codex学习手册-CodexGuide|Codex 学习手册 — CodexGuide 核心要点]]

## 相关概念
- [[concepts/Claude Code Skills]]
- [[concepts/OpenSpec规范驱动]]

## 开放问题
- Codex Memories 之外，哪些跨会话模式值得结构化沉淀，哪些继续靠 Chronicle 即可？

## Dataview
```dataview
TABLE date, source, tags, confidence
FROM "knowledge"
WHERE file.folder = "knowledge" AND contains(concepts, this.file.link)
SORT date DESC
```
