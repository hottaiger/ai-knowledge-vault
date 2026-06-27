---
concept: AI协作框架
aliases:
  - AI 协作边界
  - AI 协作心智模型
---

# AI协作框架

## 定义
判断 AI 介入任务程度的框架（🟢🟡🟠🔴 四档），以及 AI 是'失忆的实习生'等协作心智模型与实践规则（AI 使用日志、单测补审）。

## 关键洞见
- 角球 = 近封闭系统，AI 友好：开球位置固定（4 个角）、争顶区域限定（小禁区弧顶）、攻防人数固定、球路可枚举、有"充分时间"做事前规划——AI 强项。（来源：[[2026-06-20-AI角球视频引用]])
- "AI 角球"判断框架：约束越完整、契约越清晰、越接近"事前规划"而非"实时博弈"的任务，越适合交给 AI（文档/CRUD/单测/机械重构）。（来源：[[2026-06-20-AI角球与AI协作边界]])
- Vibe Coding ↔ AI Coding 的区别：前者靠感觉，后者靠人审。（来源：[[2026-05-16-关于AI-Coding我的一些总结]])

## 代表条目
- [[2026-06-20-AI角球视频引用|"AI 角球" 视频引用——角球是 AI 友好的近封闭系统]]
- [[2026-06-20-AI角球与AI协作边界|AI 角球与 AI 协作边界]]
- [[2026-05-16-关于AI-Coding我的一些总结|关于 AI Coding 我的一些总结]]

## 相关概念
- [[concepts/Claude协作工作流]]
- [[concepts/Codex提示词工作流]]

## 开放问题
- 四档分级之外，是否需要更细粒度的'AI 介入深度'维度（比如 0-10 分）？

## Dataview
```dataview
TABLE date, source, tags, confidence
FROM "knowledge"
WHERE file.folder = "knowledge" AND contains(concepts, this.file.link)
SORT date DESC
```
