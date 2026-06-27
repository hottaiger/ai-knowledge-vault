---
concept: OpenSpec规范驱动
aliases:
  - OpenSpec
  - Comet
  - SpecDrive
---

# OpenSpec规范驱动

## 定义
OpenSpec / Comet / SpecDrive 一套：把提案、设计、任务、归档和验证写成可追溯的规范文件，配合 human-in-the-loop 门控推进 change。

## 关键洞见
- 确认设计方案时只看 3 个文件：(1) openspec/changes/ /.specdrive/handoff/brainstorm-summary.md 看最终结论；(2) docs/superpowers/specs/ - -design.md 看完整设计；(3) openspec/changes/ /specs/ .md 看需求约束做"规则级"复核…（来源：[[2026-06-17-SpecDrive用法与原理]])
- （待当前Agent提炼）（来源：[[2026-06-03-Comet-使用问答与实现原理]])
- archive 里的 spec = 某次 PR 的附带文档，存档用：记录的是"这次改动期间写的 delta spec"，跟着 change 一起归档。（来源：[[2026-05-26-OpenSpec归档变更spec与主spec区别]])

## 代表条目
- [[2026-06-17-SpecDrive用法与原理|SpecDrive 怎么用——人工审批门控 + build_command 配置原理]]
- [[2026-06-03-Comet-使用问答与实现原理|Comet 使用问答与实现原理]]
- [[2026-05-26-OpenSpec归档变更spec与主spec区别|OpenSpec 归档——change 内的 delta spec 与项目级主 spec 的区别]]

## 相关概念
- [[concepts/Claude Code Skills]]
- [[concepts/Claude协作工作流]]

## 开放问题
- OpenSpec 与传统 ADR / RFC 的边界在哪？什么时候用 OpenSpec，什么时候写 ADR 就够了？

## Dataview
```dataview
TABLE date, source, tags, confidence
FROM "knowledge"
WHERE file.folder = "knowledge" AND contains(concepts, this.file.link)
SORT date DESC
```
