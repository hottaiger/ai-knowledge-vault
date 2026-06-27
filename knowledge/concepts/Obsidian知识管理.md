---
concept: Obsidian知识管理
aliases:
  - Obsidian PKM
  - Vault Workflow
---

# Obsidian知识管理

## 定义
以 Obsidian 作为本地优先的知识底座，用链接、属性和视图把静态 Markdown 组织成可导航的知识网络。

## 关键洞见
- （待当前Agent提炼）（来源：[[2026-06-27-Obsidian-AI-Agent-9个必备Skill]])
- 规模基线：1000+ 源码文件、11w 行业务代码（不含 node_modules 等三方依赖）。（来源：[[2026-06-18-商详页项目的复杂性]])
- （待当前Agent提炼）（来源：[[2026-06-12-GitNexus-代码知识图谱与AI编码工作流]])

## 代表条目
- [[2026-06-27-Obsidian-AI-Agent-9个必备Skill|为 Obsidian 配置 AI Agent——9 个必备 Skill 详解]]
- [[2026-06-18-商详页项目的复杂性|商详页项目的复杂性——规模数据]]
- [[2026-06-12-GitNexus-代码知识图谱与AI编码工作流|GitNexus - 代码知识图谱与 AI 编码工作流]]

## 相关概念
- [[concepts/第二大脑]]
- [[concepts/上下文工程]]
- [[concepts/Claude协作工作流]]

## 开放问题
- 除了 concepts/ 之外，哪些导航层最值得补成 Bases 或 Dataview 视图？

## Dataview
```dataview
TABLE date, source, tags, confidence
FROM "knowledge"
WHERE file.folder = "knowledge" AND contains(concepts, this.file.link)
SORT date DESC
```
