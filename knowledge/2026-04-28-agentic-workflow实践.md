---
date: 2026-04-28
title: agentic workflow 实践
tags:
  - AI/Coding
  - 工作流/AI协作
  - 个人笔记/碎片
status: processed
---

参考了karpathy Claude指南中的## Goal-Driven Execution（目标驱动执行）
https://github.com/forrestchang/andrej-karpathy-skills/blob/main/skills/karpathy-guidelines/SKILL.md
参考openspace的AUTO-LEARN(自动学习)

 2. koala-skill 的本质是"查阅型知识"（组件映射、Props 列表），不是"行为指令"（像 karpathy-guidelines
  是改变行为方式的）。用 Read 读它和读 node_modules 文档是同一类操作，一致性更好

  如果 koala-skill 将来需要交互逻辑（比如自动扫描项目用了哪些组件、生成迁移报告），那时候做成 Skill
  才真正有意义。