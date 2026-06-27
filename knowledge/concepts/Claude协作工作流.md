---
concept: Claude协作工作流
aliases:
  - Claude Cowork
  - AI 协作
---

# Claude协作工作流

## 定义
把 Claude 从一次性问答工具变成长期协作者，关键在于先提供稳定上下文，再围绕任务持续迭代。

## 关键洞见
- （待当前Agent提炼）（来源：[[2026-06-27-大模型能力边界与AI项目落地]])
- （待当前Agent提炼）（来源：[[2026-06-24-Claude-Code-动态工作流]])
- 三层架构：cc-connect（中间层，本地进程）跟飞书 IM 保持 WebSocket 长连接；维护用户↔会话映射（~/.cc-connect/sessions/）；做权限检查（allow_from / admin_from）；把消息拼成完整 prompt，spawn 子进程喂给 Cursor Agent CLI；监听 JSON stream 输出实时回传…（来源：[[2026-06-20-cc-connect工作原理]])
- 角球 = 近封闭系统，AI 友好：开球位置固定（4 个角）、争顶区域限定（小禁区弧顶）、攻防人数固定、球路可枚举、有"充分时间"做事前规划——AI 强项。（来源：[[2026-06-20-AI角球视频引用]])

## 代表条目
- [[2026-06-27-大模型能力边界与AI项目落地|大模型能力边界与 AI 项目落地]]
- [[2026-06-24-Claude-Code-动态工作流|Claude Code 动态工作流（Dynamic Workflows）]]
- [[2026-06-20-cc-connect工作原理|cc-connect 工作原理]]
- [[2026-06-20-AI角球视频引用|"AI 角球" 视频引用——角球是 AI 友好的近封闭系统]]
- [[2026-06-20-AI角球与AI协作边界|AI 角球与 AI 协作边界]]
- [[2026-06-17-SpecDrive用法与原理|SpecDrive 怎么用——人工审批门控 + build_command 配置原理]]

## 相关概念
- [[concepts/上下文工程]]
- [[concepts/Claude Code Skills]]
- [[concepts/个人AI操作系统]]

## 开放问题
- 哪些启动提示和协作约束已经稳定，适合固化为团队通用模板？

## Dataview
```dataview
TABLE date, source, tags, confidence
FROM "knowledge"
WHERE file.folder = "knowledge" AND contains(concepts, this.file.link)
SORT date DESC
```
