---
date: 2026-05-04
source: 碎片速记
tags:
- AI/Coding
- 工作流/AI协作
- 提示词
- 个人笔记/碎片
concepts:
- '[[concepts/上下文工程]]'
- '[[concepts/Agent记忆架构]]'
- '[[concepts/Claude协作工作流]]'
related:
- '[[concepts/上下文工程]]'
- '[[concepts/Agent记忆架构]]'
- '[[concepts/Claude协作工作流]]'
- '[[2026-03-10-Claude-Code实战指南]]'
- '[[2026-06-20-cc-connect工作原理]]'
title: AI Workflow 在项目中的应用——提示词优化与项目记忆
created: 2026-05-04
updated: 2026-06-27
aliases:
- 提示词优化
- 项目记忆
status: processed
---
# AI Workflow 在项目中的应用——提示词优化与项目记忆

> [!quote] 一句话理解
> 上下文越详细、准确，对模型推理能力要求越低；模糊的上下文反而**消耗更多 token**。Cursor 的 DevMemory 就是这种"项目级上下文持续注入"能力的代表。

## 核心观点

- **提示词优化方向**：尽量详尽地描述功能；上下文越详细准确，对模型推理能力要求越低；模糊的上下文会消耗更多 token。
- **项目记忆**：Cursor 有类似"DevMemory"的能力，能跨会话保留项目级上下文，避免每次重复说明同一项目背景。

## 原始内容

```
提示词优化
尽量详尽的功能，描述
上下文越详细、准确，对模型推理能力要求的越低。
模糊的上下文会使用更多的 token。

项目记忆
cursor 有类似记忆的功能：DevMemory
```