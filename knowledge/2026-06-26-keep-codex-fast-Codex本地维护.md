---
date: 2026-06-26
source_type: GitHub 仓库 / Skill
source_url: https://github.com/vibeforge1111/keep-codex-fast
tags:
- Codex
- Skill
- AI/Coding
- Tool/Maintenance
- 工具/本地维护
concepts:
- '[[concepts/Claude Code Skills]]'
- '[[concepts/Codex提示词工作流]]'
related:
- '[[concepts/Claude Code Skills]]'
- '[[concepts/Codex提示词工作流]]'
- '[[2026-06-10-Codex回顾提示词]]'
- '[[2026-06-27-Obsidian-AI-Agent-9个必备Skill]]'
title: keep-codex-fast —— Codex 安全本地维护 skill
created: 2026-06-26
updated: 2026-06-26
aliases:
- keep-codex-fast
- Codex 本地维护
- Codex 加速
source_skills_sh: https://www.skills.sh/vibeforge1111/keep-codex-fast/keep-codex-fast
source_install: npx skills add https://github.com/vibeforge1111/keep-codex-fast --skill keep-codex-fast
source_stars: 1400
source_installs: 241
source_first_seen: 2026-05-02
source_security_audits: Gen Agent Trust Hub / Socket / Snyk
source_language: Python
source_license: 未在 SKILL.md 顶部声明，需查仓库
extracted:
  by: Mavis
  from: Mavis 会话 mvs_d353643a746446ffadd2e97a12bee17a
  method: 阅读 skills.sh 简介 + GitHub SKILL.md → 结构化总结
  date: 2026-06-26
status: processed
---
# keep-codex-fast —— Codex 安全本地维护 skill

> [!info] 内容来源
> 本笔记由 ==Mavis== 阅读以下资料后整理：
> - **skills.sh 简介**：https://www.skills.sh/vibeforge1111/keep-codex-fast/keep-codex-fast
> - **SKILL.md**：https://raw.githubusercontent.com/vibeforge1111/keep-codex-fast/main/SKILL.md
> - **整理时间**：2026-06-26
> - **整理方式**：阅读 SKILL.md 全文 → 结构化总结

> [!quote] 一句话理解
> **keep-codex-fast = 给 Codex 跑的"安全本地维护"工具**——用 ==归档而非删除==、==先保连续性再改状态==的方式，让 local Codex state 不臃肿、不丢上下文、不自动 kill 进程。默认 ==report-only==，首次运行零风险。

---

## 核心原则

> [!important] 第一原则
> **先保连续性（preserve continuity），再改状态。**
> 对于活跃的 repo chat 用户可能还要继续的，==必须先==推荐 comprehensive handoff doc + reactivation prompt，再考虑归档任何东西。

这跟"AI 协作者不破坏上下文"的原则一致——技术清理不能以牺牲用户思路连续性为代价。

---

## 三种模式

| 模式 | 触发 | 干啥 | 风险 |
|---|---|---|---|
| **Inspect**（默认） | `python scripts/keep_codex_fast.py` | 只读 report：active/archived session 大小、最大活跃 session、thread metadata bloat、stale worktree、log 大小、Windows 扩展路径、config prune 候选、top Node 进程 | ==零风险== |
| **Maintain** | `--apply` | 备份、归档老 session、移走 stale worktree、轮转 logs、剪掉 dead config、规范化路径 | 有，但都是可逆的 |
| **Optional repair** | `--apply --repair-thread-metadata-bloat` | 缩短 SQLite 里过大的 `threads.title` 和 `threads.first_user_message`（rollout transcript 保留） | 显式 opt-in，backup 后才动 |

> [!note] 三个关键默认行为
> - 首次运行 ==必须 report-only==
> - Codex 在跑时 ==默认 report-only==，必须 Codex 关闭或 `--wait-for-codex-exit` 才 apply
> - 永远 ==归档而非删除==

---

## Thread Metadata Bloat（独特洞察）

> [!important] 一个具体可诊断的"为什么 Codex 变慢"
> Codex Desktop 可能在 SQLite 的 `threads.title` 或 `threads.first_user_message` 字段里存了完整 prompt 或 history 规模的内容，而不是只存 display title / preview。
>
> 这会影响 thread list/navigation 的渲染路径——UI 在加载任何东西之前就先慢下来。

**Report 出来会看到的指标：**
- active thread count
- title/preview 字符总数
- 最大 title/preview 长度
- 超过 title limit 的数量
- 超过 preview limit 和 10k 字符的数量

**默认 apply 不修**，需要显式 `--repair-thread-metadata-bloat`。修的时候：
- title 默认 120 字符
- preview 默认 240 字符
- 修复后的值 append 到 `session_index.jsonl`（跟 upstream name-update 存储一致）
- 修复 manifest 存 `thread-metadata-repairs.jsonl` + `restore-thread-metadata.py`，==完全可逆==

**注意**：这是**元数据**修复，不会动 rollout transcript（存在 JSONL 里）。

---

## Handoff Doc + Reactivation Prompt（独特实践）

> [!tip] 这跟用户的 ai-knowledge-vault 思路天然契合
> keep-codex-fast 推荐的 handoff doc 模式 ==把 AI 协作产出沉淀成可复用的项目知识==，跟你 [[2026-04-23-Claude-多智能体协作5种模式]] 笔记里的"知识沉淀"和 [[2026-06-24-Claude-Code-动态工作流]] 笔记里的"save 成 slash command"一脉相承。

**何时用**：归档活跃 repo chat 之前，先做 handoff doc。

**Handoff doc 应包含**：
- repo/path 和 branch
- current goal
- 已完成
- touched files
- 运行过的 commands/tests
- 已知错误/警告
- 未决决策
- 约束、用户偏好、do-not-touch 区域
- 下 3-7 个具体步骤

**Reactivation prompt（贴在 handoff 顶部或底部）**：

```text
We are continuing from this handoff. Read this document first, inspect the current repo state, verify what still applies, and continue from the next steps without assuming the old chat context is available.
```

存到 `docs/codex-handoffs/YYYY-MM-DD-topic.md` 这样的 repo-local 位置，或用户指定的位置。

---

## Apply 默认会做什么

`python scripts/keep_codex_fast.py --apply --archive-older-than-days 10 --worktree-older-than-days 7`：

| 动作 | 路径 |
|---|---|
| 备份重要元数据 | `~/Documents/Codex/codex-backups/keep-codex-fast-*` |
| 归档老非 pinned session | `~/.codex/archived_sessions/` |
| 规范化 Windows `\\?\C:\...` 扩展路径（SQLite 文本字段） | — |
| 剪掉 missing/temp project 块 | `config.toml`，写 UTF-8 **无 BOM** |
| 移走 stale worktree | `~/.codex/archived_worktrees/` |
| 轮转 `logs_2.sqlite*`（仅超过阈值） | `~/.codex/archived_logs/` |
| Report heavy Node 进程 | ==不 kill== |
| Report thread title/preview bloat | 默认 ==不修==，需 opt-in |

**`--backup-only`**：只备份、不动 state。
**`--details`**：打印 raw thread ID / 标题 / 路径（默认不打，保护隐私）。

---

## Safety Rules 关键点（10 条核心）

> [!warning] 别踩的雷
> 1. **首次运行必须 report-only**，不允许写文件、备份、move folder
> 2. **Codex 在跑时默认 report-only**
> 3. **永远归档而非永久删除**（session/log/worktree/memory/skill/plugin/automation）
> 4. 每次 move session/worktree 都写 manifest + restore script
> 5. 不碰凭证文件，除非用户显式要求
> 6. 备份文件夹是私密本地 artifacts（含 Codex 元数据），==不要让用户公开分享==
> 7. 默认不打印 raw thread ID、chat title、本地路径（用 `--details` 才打）
> 8. **归档活跃 repo chat 前必须 handoff**
> 9. 重要 thread 用户没确认 handoff 存在或不需要之前，==不归档==
> 10. 不要宣传"通用提速"——改进是"本地维护的结果"

---

## Recommended Policy

> [!tip] 实践建议
> - 只保留最近 ==7-10 天==的非 pinned chat
> - 重要老 thread ==用 handoff doc==
> - 从 handoff doc 起新 thread，而不是反复 resume 大 chat
> - 每天用就每周跑一次维护
> - 首次成功 apply 后可设 weekly/biweekly ==report-only reminder==（==不要==持续 maintenance）
> - 不确定时，==留 chat active 或问用户==，永不擅自归档 pinned/current chat
> - title/preview repair 视为==纯元数据修复==——rollout transcript 不动

---

## Anti-patterns（明确不要做）

- ❌ 永久删除 session/logs/worktree/memory/plugin/skill
- ❌ Codex 在写 DB 时改
- ❌ 没 handoff 就 archive 重要 repo chat
- ❌ 把 active history size 当成"坏"的不看用户需求
- ❌ 把 preview metadata repair 当成删除 transcript
- ❌ 自动 kill Node/dev 进程
- ❌ 不备份、不 parse-check 就改 `config.toml`
- ❌ 写 UTF-8 with BOM 的 TOML（Windows 坑）
- ❌ 承诺"通用提速"——要诚实
- ❌ 让用户觉得"我用 Codex 多了是错的"

---

## 适用场景

> [!success] 适合
> - Codex 感觉**慢/臃肿**（thread metadata bloat、log 太大、stale worktree、dead config project）
> - 本地 session/log/worktree 长期累积想整理
> - 想安全维护 Codex Desktop / CLI 状态，==不冒失丢失上下文==
> - 想用 handoff doc 模式沉淀活跃 chat 为项目知识

> [!failure] 不适合
> - 想直接提速 Codex 模型推理——这个跟模型行为无关，是本地 state
> - 想让 skill 自动跑持续 maintenance——keep-codex-fast ==不推荐==自动跑 mutation，只推荐自动跑 report
> - 你想要"通用提速"承诺——skill 明确说改进是"本地维护结果"，不是通用事实

---

## 安装

```bash
npx skills add https://github.com/vibeforge1111/keep-codex-fast --skill keep-codex-fast
```

或手动：clone 仓库，scripts 里有 `keep_codex_fast.py`。

**首次使用流程**：
1. `python scripts/keep_codex_fast.py` — report-only，看数据
2. 跟用户确认哪些 active repo chat 还要继续
3. 给那些 chat 做 handoff doc
4. 关闭 Codex（或者用 `--wait-for-codex-exit`）
5. `python scripts/keep_codex_fast.py --apply --archive-older-than-days 10 --worktree-older-than-days 7`
6. 再跑一次 report 验证
7. 询问用户要不要 weekly/biweekly report-only reminder

---

## 跟 [[2026-06-26-headroom-LLM-context压缩]] 的协同

| 维度 | Headroom | keep-codex-fast |
|---|---|---|
| **作用层** | LLM 上下文（token） | 本地文件 state（DB、log、worktree） |
| **核心动作** | 压缩 prompt | 归档/规范化/清理 |
| **触发原因** | 上下文窗口、token 成本 | Codex 慢、本地臃肿 |
| **可逆性** | CCR（cache & retrieve） | manifest + restore script |
| **协同** | 跑 Codex 时省 token | 跑 Codex 前清理 state 让它跑得轻 |

两者是**互补**的：先用 keep-codex-fast 让本地 state 干净，再用 Headroom 让 token 消耗低。

---

## 相关笔记

- [[2026-06-26-headroom-LLM-context压缩]] — Token 层的压缩
- [[2026-05-29-Codex学习手册-CodexGuide]] — Codex 基础
- [[2026-04-23-Claude-多智能体协作5种模式]] — 多 agent 协作
- [[2026-06-24-Claude-Code-动态工作流]] — Claude Code Workflow，跟 Codex 是同类工具的不同实现
- [[2026-03-10-Claude-Code实战指南]] — Claude Code 基础，对照看 Codex 类似设计