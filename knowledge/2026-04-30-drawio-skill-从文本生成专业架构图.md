---
title: drawio-skill — 从文本生成专业架构图（含自检迭代）
date: 2026-04-30
created: 2026-04-30
updated: 2026-06-27
tags:
  - AI/Coding/工具
  - Skill
  - 架构图
  - drawio
  - 工作流/可视化
aliases:
  - drawio-skill
  - 从文本生成架构图
  - drawio 桌面版 CLI
source: https://github.com/Agents365-ai/drawio-skill
source_url: https://agents365-ai.github.io/drawio-skill/
status: processed
---

# drawio-skill — 从文本生成专业架构图（含自检迭代）

> [!quote] 一句话理解
> Agents365-ai/drawio-skill 是**唯一**纯 SKILL.md 方案中能"自动读取输出 PNG → 修复问题 → 支持 5 轮迭代"的架构图生成 skill，预设 6 种图表类型（ERD/UML/序列图/架构图/ML/流程图），支持张量形状标注和网格对齐布局。

## 核心观点

- **核心能力**：根据自然语言生成 `.drawio` XML；用 draw.io 桌面版 CLI 导出 PNG/SVG/PDF/JPG；6 种图表类型预设（ERD、UML 类图、序列图、架构图、ML/DL、流程图）；动画连接线（数据流）；ML 张量形状标注 `(B, C, H, W)`；网格对齐（10px 倍数）；浏览器降级方案（CLI 不可用时生成 diagrams.net URL）；**自检 + 迭代循环**（读取输出 PNG → 自动修复 → 5 轮优化）。
- **6 大优势**：(1) 唯一带自检 + 5 轮迭代的纯 SKILL.md 方案；(2) 6 种图表类型预设；(3) ML/DL 模型图张量标注；(4) 多智能体、零配置、跨 6 个平台；(5) 网格对齐、分级间距、路由走廊、hub 居中；(6) 专业级布局。
- **前置依赖**：必须装 draw.io 桌面版（`brew install --cask drawio`）。
- **安装**：OpenClaw/ClawHub `clawhub install drawio-pro-skill`；Claude Code `git clone https://github.com/Agents365-ai/drawio-skill.git ~/.claude/skills/drawio-skill`。
- **样式预设**：`default`（蓝/绿/黄）/ `corporate`（低饱和度专业配色）/ `handdrawn`（手绘描边白板风）。还能从现有 `.drawio` 或图片学习样式。
- **图表类型覆盖**：架构图（微服务/云 AWS/GCP/Azure/网络拓扑/部署）、ML/DL（Transformer/CNN/LSTM/GRU）、流程图（业务/工作流/决策树/状态机）、UML（类图/序列图）、数据图（ER/DFD）、其他（组织架构/思维导导/线框图）。

## 对比原生智能体的优势

| 功能 | drawio-skill | 原生智能体 |
|------|-------------|-----------|
| 导出后自检 | ✅ 自动修复 6 类问题 | ❌ |
| 迭代反馈循环 | ✅ 5 轮循环 | ❌ |
| 图表类型预设 | ✅ 6 种 | ❌ |
| 网格对齐 | ✅ 10px 倍数 | ❌ |
| 动画连线 | ✅ 数据流可视化 | ❌ |
| ML/DL 模型图 | ✅ 张量标注 | ❌ |

## 原始内容

# drawio-skill — 从文本生成专业架构图

> 来源：https://github.com/Agents365-ai/drawio-skill
> 官方文档：https://agents365-ai.github.io/drawio-skill/

## 核心功能

- 根据自然语言描述生成 `.drawio` XML 文件
- 使用 draw.io 桌面版 CLI 导出为 PNG、SVG、PDF、JPG
- **6 种图表类型预设**：ERD、UML 类图、序列图、架构图、ML/深度学习、流程图
- 动画连接线 (`flowAnimation=1`) 适用于数据流和管道图
- ML 模型图支持张量形状标注 `(B, C, H, W)`
- 网格对齐布局（所有坐标对齐到 10px 倍数）
- 浏览器降级方案：CLI 不可用时生成 diagrams.net URL
- 自检 + 迭代循环：读取输出 PNG，自动修复问题，支持 5 轮优化

## 核心优势

1. **自检 + 迭代循环** — 唯一纯 SKILL.md 方案中能自动读取输出、修复问题、支持多轮优化的
2. **6 种图表类型预设** — 每种都有预设形状、样式和布局规范
3. **ML/DL 模型图** — 张量形状标注、层类型配色，专为学术论文打造
4. **多智能体、零配置** — 跨 6 个平台运行，仅需 SKILL.md + draw.io 桌面版
5. **专业级布局** — 网格对齐、分级间距、路由走廊、hub 居中策略

## 前置依赖

需要安装 draw.io 桌面版：

```bash
# macOS
brew install --cask drawio

# 验证
drawio --version
```

## 安装方式

### OpenClaw / ClawHub

```bash
clawhub install drawio-pro-skill

# 或手动安装
git clone https://github.com/Agents365-ai/drawio-skill.git ~/.openclaw/skills/drawio-skill
```

### Claude Code

```bash
git clone https://github.com/Agents365-ai/drawio-skill.git ~/.claude/skills/drawio-skill
```

## 使用示例

直接描述图表：

```
画一个微服务电商架构图，包含 Mobile/Web/Admin 客户端，API Gateway，
Auth/User/Order/Product/Payment 微服务，Kafka 消息队列，Notification 服务，
以及各自独立的数据库
```

智能体自动生成 `.drawio` 文件并导出为 PNG。

## 样式预设

| 名称 | 说明 |
|------|------|
| `default` | 干净的蓝/绿/黄配色 |
| `corporate` | 低饱和度专业配色，适合商务 |
| `handdrawn` | 手绘描边风格，适合白板式图表 |

可以从现有 `.drawio` 文件或图片学习样式：

```
从 ~/diagrams/brand.drawio 学习我的样式，保存为 "mybrand"
```

## 支持的图表类型

- **架构图**：微服务、云架构（AWS/GCP/Azure）、网络拓扑、部署架构
- **ML / 深度学习**：Transformer、CNN、LSTM、GRU
- **流程图**：业务流程、工作流、决策树、状态机
- **UML**：类图、序列图
- **数据图**：ER 图、数据流图（DFD）
- **其他**：组织架构图、思维导图、线框图

## 对比优势

| 功能 | drawio-skill | 原生智能体 |
|------|-------------|-----------|
| 导出后自检 | ✅ 自动修复 6 类问题 | ❌ |
| 迭代反馈循环 | ✅ 5 轮循环 | ❌ |
| 图表类型预设 | ✅ 6 种 | ❌ |
| 网格对齐 | ✅ 10px 倍数 | ❌ |
| 动画连线 | ✅ 数据流可视化 | ❌ |
| ML/DL 模型图 | ✅ 张量标注 | ❌ |