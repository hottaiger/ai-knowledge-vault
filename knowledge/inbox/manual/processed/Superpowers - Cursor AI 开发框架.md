# Superpowers - Cursor AI 开发框架

> 📅 计划：2026-04-02 阅读 / 观看
> 🏷️ 标签：#AI/Coding #Cursor #开发工作流 #Superpowers

## 简介

Superpowers 是一个**完整的 AI 软件开发工作流框架**，基于一套可组合的"技能"（Skills）和初始指令，让 AI 编程 Agent 能够遵循规范的开发流程，而不是一上来就写代码。

核心思路：**Spec → Plan → Execute**，每个阶段都有自动化技能触发。

---

## 核心技能（Skills）

| 技能 | 触发时机 | 功能 |
|------|----------|------|
| `brainstorming` | 任何创意工作前 | 深入理解需求，探索替代方案，分块展示设计供确认 |
| `dispatching-parallel-agents` | 遇到 2+ 个独立任务 | 并行调度子 Agent，无共享状态依赖时使用 |
| `executing-plans` | 有已批准的实施方案 | 启动子 Agent 驱动的开发流程，含检查点审核 |
| `finishing-a-development-branch` | 实现完成、测试通过后 | 指导代码合并/PR/清理的决策 |
| `receiving-code-review` | 收到代码审查反馈时 | 在实现建议前验证技术准确性，不盲目接受 |

### 开发流程核心技能

- **using-git-worktrees** - 设计批准后，创建隔离分支工作区，验证干净测试基线
- **writing-plans** - 将工作拆解为 2-5 分钟的小任务，每个任务有精确文件路径和验证步骤
- **subagent-driven-development** - 每个任务派发一个子 Agent，两阶段审核（规范合规性 + 代码质量）
- **test-driven-development** - 强制执行 RED-GREEN-REFACTOR：写失败测试 → 看它失败 → 写最小代码 → 看它通过
- **requesting-code-review** - 任务间触发，按严重性报告问题

---

## 工作流三步曲

```
1. 🔍 Spec（需求定义）
   ↓ Brainstorming
   Agent 不直接写代码，先问清楚"你真正想做什么"
   ↓ 确认设计
2. 📋 Plan（实施方案）
   ↓ Writing Plans
   拆解为可执行的小任务（2-5 分钟/任务）
   ↓ "go"
3. ⚡ Execute（执行开发）
   ↓ Subagent-Driven Development
   子 Agent 并行工作 → 审核 → 继续
```

---

## 安装方式

### Cursor
```
/add-plugin superpowers
```
或在 Cursor 插件市场搜索 "superpowers"

### Claude Code
```
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

### 其他平台
- Codex、OpenCode、Gemini 均支持，详见 GitHub

---

## 相关资源

- **官网/插件页**：https://cursor.com/cn/marketplace/superpowers
- **GitHub**：https://github.com/obra/superpowers
- **B 站视频**：https://www.bilibili.com/video/BV1wxAHzdEj3/
- **对比文章（微信）**：https://mp.weixin.qq.com/s/NeBSi-Q8zUWlWb0mL5BPOA
  _《AI 编程三剑客：Spec-Kit、OpenSpec、Superpowers 深度对比与实战指南》_

---

## 我的笔记

> **为什么要了解这个？**
> 张硕正在学习 AI 应用开发，Superpowers 提供了一套经过验证的 AI 编程工作流规范，
> 对于想用 Cursor/Cursor 类工具提升开发效率的前端工程师很有参考价值。

> **学习目标（今日 2026-04-02）**
> - [x] 看 B 站视频完整了解 Superpowers 工作流
> - [ ] 在 Cursor 中安装 Superpowers 插件
> - [ ] 结合文章理解它与 OpenSpec、Spec-Kit 的区别
> - [ ] 尝试用它开发一个小功能，体验子 Agent 驱动的工作流

---

## 📺 B 站视频总结（AI大白话007）

> 视频：【AI大白话007】聊聊神级 Skills 开源项目 Superpowers 爆火 37K+ Star
> 字幕文件：`/Users/zhangshuo12/Documents/ai/B站字幕/【AI大白话007】—聊聊神级Skills开源项目Superpowers 爆火37K+Star—【2026-03-20】.txt`

### 核心问题
AI 生成的代码看似有理，一运行全是 bug，花在调试上的时间比手写还长。

### 🎯 七步强制开发流程

| 步骤 | 内容 |
|------|------|
| **1. 需求梳理** | 提问细化需求，列出不同实现方式，整理成设计文档，必须用户确认后才继续 |
| **2. 创建环境** | 建立独立分支，跑基线测试 |
| **3. 任务拆分** | 拆成多个小任务，每个有明确目标 |
| **4-6. TDD 开发** | 先写测试用例 → 写代码 → 代码审查 |
| **7. 统一测试 + 清理** | 最终测试与代码清理 |

**核心理念**：每步都有明确标准，不通过不能往下走，相当于给 AI 配了个"严格主管"。

### 🏄 作者实测体验
做一个"气话转语气"工具时：
1. Superpowers **没直接写代码**，而是先问清楚：网页还是命令行？要不要用户登录？自定义语气？
2. 确认后设计架构、页面布局、边界情况
3. 生成代码**第一次运行就完全通过**，无 bug
4. 生成的句子完全听不出是机器写的

### 安装升级
- **Cursor**：两步命令安装
- **其他 AI**：让 AI 拉取官方安装文档即可
- **升级**：运行一条命令 + 重启

---

## 对比：Superpowers vs OpenSpec

| 维度 | Superpowers | OpenSpec |
|------|-------------|----------|
| 核心理念 | 规范开发流程（Spec-Plan-Execute） | 规格化驱动 |
| 子 Agent | 支持并行子 Agent 调度 | 强调规格约束 |
| TDD | 强制 RED-GREEN-REFACTOR | 鼓励测试 |
| 适用场景 | 复杂多任务、需要审核的开发 | 需求明确的规格化实现 |

> 参考：《AI 编程三剑客：Spec-Kit、OpenSpec、Superpowers 深度对比与实战指南》
