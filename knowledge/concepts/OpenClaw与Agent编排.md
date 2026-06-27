---
concept: OpenClaw与Agent编排
aliases:
  - OpenClaw
  - Agent Orchestration
---

# OpenClaw与Agent编排

## 定义
围绕多 Agent 的连接、调度和执行，把上下文、工具和外部系统编排成可持续运转的自动化流程。

## 关键洞见
- 飞书权限名照抄搜不到：INSTALL.md 把事件标识（im.message.receive_v1，带点）和权限标识（带冒号）混了。搜权限要去 docs/feishu.md 第四章拿 5 个正确列表；事件订阅单独去「事件与回调」菜单加。（来源：[[2026-06-22-cc-connect安装配置避坑指南]])
- 三层架构：cc-connect（中间层，本地进程）跟飞书 IM 保持 WebSocket 长连接；维护用户↔会话映射（~/.cc-connect/sessions/）；做权限检查（allow_from / admin_from）；把消息拼成完整 prompt，spawn 子进程喂给 Cursor Agent CLI；监听 JSON stream 输出实时回传…（来源：[[2026-06-20-cc-connect工作原理]])
- 确认设计方案时只看 3 个文件：(1) openspec/changes/ /.specdrive/handoff/brainstorm-summary.md 看最终结论；(2) docs/superpowers/specs/ - -design.md 看完整设计；(3) openspec/changes/ /specs/ .md 看需求约束做"规则级"复核…（来源：[[2026-06-17-SpecDrive用法与原理]])
- 核心能力：根据自然语言生成 .drawio XML；用 draw.io 桌面版 CLI 导出 PNG/SVG/PDF/JPG；6 种图表类型预设（ERD、UML 类图、序列图、架构图、ML/DL、流程图）；动画连接线（数据流）；ML 张量形状标注 (B, C, H, W)；网格对齐（10px 倍数）；浏览器降级方案（CLI 不可用时生成 diagrams.n…（来源：[[2026-04-30-drawio-skill-从文本生成专业架构图]])

## 代表条目
- [[2026-06-22-cc-connect安装配置避坑指南|cc-connect 安装配置避坑指南（实战 9 条）]]
- [[2026-06-20-cc-connect工作原理|cc-connect 工作原理]]
- [[2026-06-17-SpecDrive用法与原理|SpecDrive 怎么用——人工审批门控 + build_command 配置原理]]
- [[2026-04-30-drawio-skill-从文本生成专业架构图|drawio-skill — 从文本生成专业架构图（含自检迭代）]]
- [[2026-04-26-多agent方案-Cline-Kanban|Multiple coding agents. One UI to orchestrate them.]]
- [[2026-03-10-Claude-Code实战指南|Claude Code 实战指南]]

## 相关概念
- [[concepts/Agent记忆架构]]
- [[concepts/个人AI操作系统]]
- [[concepts/Claude Code Skills]]

## 开放问题
- 哪些 OpenClaw 经验已经上升为通用编排原则，值得抽象到概念层？

## Dataview
```dataview
TABLE date, source, tags, confidence
FROM "knowledge"
WHERE file.folder = "knowledge" AND contains(concepts, this.file.link)
SORT date DESC
```
