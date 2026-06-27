---
concept: Agent记忆架构
aliases:
  - Agent Memory
  - Memory Architecture
---

# Agent记忆架构

## 定义
把当前上下文、长期事实、近期脉络和历史案例拆成不同层级，分别用匹配的存储与读取策略管理。

## 关键洞见
- 提示词优化方向：尽量详尽地描述功能；上下文越详细准确，对模型推理能力要求越低；模糊的上下文会消耗更多 token。（来源：[[2026-05-04-AI-Workflow在项目中的应用]])
- 工具使用系统（来源：[[2026-03-10-Claude-Code实战指南]])

## 代表条目
- [[2026-05-04-AI-Workflow在项目中的应用|AI Workflow 在项目中的应用——提示词优化与项目记忆]]
- [[2026-03-10-Claude-Code实战指南|Claude Code 实战指南]]

## 相关概念
- [[concepts/上下文工程]]
- [[concepts/第二大脑]]
- [[concepts/OpenClaw与Agent编排]]

## 开放问题
- 哪些信息应继续结构化存储，哪些更适合摘要或语义检索？

## Dataview
```dataview
TABLE date, source, tags, confidence
FROM "knowledge"
WHERE file.folder = "knowledge" AND contains(concepts, this.file.link)
SORT date DESC
```
