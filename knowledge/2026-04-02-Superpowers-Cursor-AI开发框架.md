---
date: 2026-04-02
source: https://cursor.com/cn/marketplace/superpowers
tags:
- AI/Coding
- Cursor
- 开发工作流
- Superpowers
- TDD
- 子Agent
concepts:
- '[[concepts/Claude协作工作流]]'
related:
- '[[concepts/Claude协作工作流]]'
- '[[2026-06-12-GitNexus-代码知识图谱与AI编码工作流]]'
- '[[2026-06-24-Claude-Code-动态工作流]]'
title: Superpowers - Cursor AI 开发框架
status: processed
category: AI/AI/Coding
---
# Superpowers - Cursor AI 开发框架

## 是什么

Superpowers 是一个完整的 AI 软件开发工作流框架，基于一套可组合的"技能"（Skills），让 AI 编程 Agent 遵循规范的开发流程，而不是一上来就写代码。

核心理念：**Spec → Plan → Execute**，每个阶段都有自动化技能触发。

## 工作流三步曲

```
1. Spec（需求定义）  ← Brainstorming
   Agent 不直接写代码，先问清楚"你真正想做什么"
   ↓ 确认设计
2. Plan（实施方案）  ← Writing Plans
   拆解为可执行的小任务（2-5 分钟/任务）
   ↓ "go"
3. Execute（执行开发）  ← Subagent-Driven Development
   子 Agent 并行工作 → 审核 → 继续
```

## 核心技能

| 技能 | 触发时机 | 功能 |
|------|----------|------|
| brainstorming | 任何创意工作前 | 深入理解需求，探索替代方案，分块展示设计供确认 |
| dispatching-parallel-agents | 遇到 2+ 个独立任务 | 并行调度子 Agent，无共享状态依赖时使用 |
| executing-plans | 有已批准的实施方案 | 启动子 Agent 驱动的开发流程，含检查点审核 |
| finishing-a-development-branch | 实现完成、测试通过后 | 指导代码合并/PR/清理的决策 |
| receiving-code-review | 收到代码审查反馈时 | 在实现建议前验证技术准确性，不盲目接受 |

## TDD 强制流程

RED-GREEN-REFACTOR：
1. 写失败测试 → 看它失败
2. 写最小代码 → 看它通过
3. 重构优化

## 七步强制开发流程

1. **需求梳理**：提问细化需求，列出不同实现方式，整理成设计文档，必须用户确认后才继续
2. **创建环境**：建立独立分支，跑基线测试
3. **任务拆分**：拆成多个小任务，每个有明确目标
4-6. **TDD 开发**：先写测试用例 → 写代码 → 代码审查
7. **统一测试 + 清理**：最终测试与代码清理

**核心理念**：每步都有明确标准，不通过不能往下走，相当于给 AI 配了个"严格主管"。

## 与 OpenSpec 对比

| 维度 | Superpowers | OpenSpec |
|------|-------------|----------|
| 核心理念 | 规范开发流程（Spec-Plan-Execute） | 规格化驱动 |
| 子 Agent | 支持并行子 Agent 调度 | 强调规格约束 |
| TDD | 强制 RED-GREEN-REFACTOR | 鼓励测试 |
| 适用场景 | 复杂多任务、需要审核的开发 | 需求明确的规格化实现 |

## 安装

### Cursor
```
/add-plugin superpowers
```

### Claude Code
```
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

## 关键资源

- GitHub：https://github.com/obra/superpowers
- B站视频：https://www.bilibili.com/video/BV1wxAHzdEj3/
- 对比文章：https://mp.weixin.qq.com/s/NeBSi-Q8zUWlWb0mL5BPOA

## 核心观点

Superpowers 的核心价值：**没直接写代码，而是先问清楚需求**。这样做的好处是 AI 生成的代码第一次运行就完全通过，而不是看似有理实则一堆 bug。

---

*整理自微信公众号笔记 | 2026-04-19 小v整理*
