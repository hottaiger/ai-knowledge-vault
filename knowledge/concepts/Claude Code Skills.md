---
concept: Claude Code Skills
aliases:
  - Skills
  - Agent Skills
---

# Claude Code Skills

## 定义
把可复用工作流封装成目录化技能，连同脚本、素材和约束一起交给 Agent 按需执行。

## 关键洞见
- （待当前Agent提炼）（来源：[[2026-06-27-Obsidian-AI-Agent-9个必备Skill]])
- （待当前Agent提炼）（来源：[[2026-06-26-keep-codex-fast-Codex本地维护]])
- 确认设计方案时只看 3 个文件：(1) openspec/changes/ /.specdrive/handoff/brainstorm-summary.md 看最终结论；(2) docs/superpowers/specs/ - -design.md 看完整设计；(3) openspec/changes/ /specs/ .md 看需求约束做"规则级"复核…（来源：[[2026-06-17-SpecDrive用法与原理]])
- 证据优先：先用 Codex 会话记录 + Codex Memories + Chronicle 三层证据定位模式，不要凭空臆测重复模式。（来源：[[2026-06-10-Codex回顾提示词]])

## 代表条目
- [[2026-06-27-Obsidian-AI-Agent-9个必备Skill|为 Obsidian 配置 AI Agent——9 个必备 Skill 详解]]
- [[2026-06-26-keep-codex-fast-Codex本地维护|keep-codex-fast —— Codex 安全本地维护 skill]]
- [[2026-06-17-SpecDrive用法与原理|SpecDrive 怎么用——人工审批门控 + build_command 配置原理]]
- [[2026-06-10-Codex回顾提示词|Codex 回顾提示词——识别可封装的重复工作流]]
- [[2026-06-05-Effect-of-gitignore-on-gitnexus-skills|Effect of .gitignore on gitnexus skills]]
- [[2026-04-30-drawio-skill-从文本生成专业架构图|drawio-skill — 从文本生成专业架构图（含自检迭代）]]

## 相关概念
- [[concepts/Claude协作工作流]]
- [[concepts/上下文工程]]
- [[concepts/第二大脑]]

## 开放问题
- 哪些重复出现的知识整理动作应该继续沉淀成新 skill，而不是留在临时对话里？

## Dataview
```dataview
TABLE date, source, tags, confidence
FROM "knowledge"
WHERE file.folder = "knowledge" AND contains(concepts, this.file.link)
SORT date DESC
```
