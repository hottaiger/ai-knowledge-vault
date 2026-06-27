---
concept: MCP工具链
aliases:
  - Model Context Protocol
---

# MCP工具链

## 定义
Model Context Protocol 标准化 AI 调用外部工具/数据源。GitNexus、Playwright、Sequential Thinking、Figma MCP 等都基于 MCP 暴露能力。

## 关键洞见
- 返回结构 = 代码 + 提示：get_design_context 返回 React 组件代码（含 data-node-id 注入），同时附带 4 条提示原文。代码不能直接用，提示信息很关键。（来源：[[2026-04-26-Figma-MCP缓存设计稿原理]])
- 工具封装：解决"知识获取"（来源：[[2026-03-23-Google-5种Agent设计模式]])
- 工具使用系统（来源：[[2026-03-10-Claude-Code实战指南]])
- （待当前Agent提炼）（来源：[[2026-06-12-GitNexus-代码知识图谱与AI编码工作流]])

## 代表条目
- [[2026-04-26-Figma-MCP缓存设计稿原理|Figma MCP 缓存设计稿——get_design_context 返回结构解析]]
- [[2026-03-23-Google-5种Agent设计模式|Google 5种Agent设计模式]]
- [[2026-03-10-Claude-Code实战指南|Claude Code 实战指南]]
- [[2026-06-12-GitNexus-代码知识图谱与AI编码工作流|GitNexus - 代码知识图谱与 AI 编码工作流]]

## 相关概念
- [[concepts/OpenClaw与Agent编排]]
- [[concepts/代码知识图谱]]

## 开放问题
- MCP 工具数量增多后，Agent 的工具选择策略如何训练？安全白名单机制如何补强？

## Dataview
```dataview
TABLE date, source, tags, confidence
FROM "knowledge"
WHERE file.folder = "knowledge" AND contains(concepts, this.file.link)
SORT date DESC
```
