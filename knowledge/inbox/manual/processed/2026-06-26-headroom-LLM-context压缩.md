---
title: Headroom —— LLM Context 压缩层
date: 2026-06-26
created: 2026-06-26
updated: 2026-06-26
tags:
  - AI Coding
  - Claude Code
  - Token Optimization
  - Tool/Compression
  - Agent/Infrastructure
aliases:
  - Headroom
  - headroom-ai
  - LLM Context 压缩
source_type: GitHub 仓库
source_url: https://github.com/headroomlabs-ai/headroom
source_upstream: https://github.com/chopratejas/headroom
source_stars: 51324
source_forks: 3639
source_language: Python
source_license: Apache-2.0
source_pypi: headroom-ai
source_npm: headroom-ai
source_docker: ghcr.io/chopratejas/headroom:latest
source_homepage: https://headroomlabs-ai.github.io/headroom/
extracted:
  by: Mavis
  from: Mavis 会话 mvs_d353643a746446ffadd2e97a12bee17a
  method: 阅读 README + GitHub API 元信息 → 结构化总结
  date: 2026-06-26
status: processed
---

# Headroom —— LLM Context 压缩层

> [!info] 内容来源
> 本笔记由 ==Mavis== 阅读以下资料后整理：
> - **GitHub 仓库**：https://github.com/headroomlabs-ai/headroom
> - **README**：https://raw.githubusercontent.com/headroomlabs-ai/headroom/main/README.md
> - **元信息**：GitHub REST API
> - **整理时间**：2026-06-26
> - **整理方式**：阅读 README + API 元信息 → 结构化总结
>
> ⚠️ **仓库 URL 注意**：用户给的是 `headroomlabs-ai/headroom`（组织仓库），README 内部链接用的是 `chopratejas/headroom`（个人仓库）。描述、topics、commit 历史一致，应为同一项目的组织迁移版本。引用时建议用 `chopratejas/headroom` 这个最权威的入口。

> [!quote] 一句话理解
> **Headroom = 在 LLM 之前插入的"上下文压缩层"**——压缩工具输出 / 日志 / RAG chunks / 文件 / 对话历史，让 ==60-95% token 减少==、答案质量不变。可作为 library、proxy、MCP server、agent wrapper 接入。==可逆（CCR）==——原始内容本地缓存，LLM 必要时可拉回。

---

## 核心数据

> [!tip] 实测工作负载节省

| Workload | Before | After | Savings |
|---|---:|---:|---:|
| Code search (100 results) | 17,765 | 1,408 | ==**92%**== |
| SRE incident debugging | 65,694 | 5,118 | ==**92%**== |
| GitHub issue triage | 54,174 | 14,761 | **73%** |
| Codebase exploration | 78,502 | 41,254 | **47%** |

> [!tip] 准确度不降反升

| Benchmark | Baseline | Headroom | Delta |
|---|---:|---:|---|
| GSM8K（数学） | 0.870 | 0.870 | ±0.000 |
| TruthfulQA（事实） | 0.530 | 0.560 | ==+0.030== |
| SQuAD v2 | — | 97% | 19% 压缩 |
| BFCL（工具调用） | — | 97% | 32% 压缩 |

---

## 三种使用模式

| 模式 | 命令 | 适用 |
|---|---|---|
| **Library** | `from headroom import compress` / `await compress(messages)` | 集成到自有 Python / TS 应用 |
| **Proxy** | `headroom proxy --port 8787` | ==零代码改动==，任何 OpenAI-compatible 客户端 |
| **Agent wrap** | `headroom wrap claude\|codex\|aider\|copilot\|opencode` | 一行命令包裹主流 coding agent |
| **MCP server** | `headroom mcp install` | 任何 MCP 客户端（暴露 `headroom_compress` / `headroom_retrieve` / `headroom_stats`） |

> [!note] 特殊场景
> **Cursor** 不是 CLI 工具，`headroom wrap cursor` 会启动 proxy 并打印 base URL 让你粘到 Cursor settings 里。

---

## 架构

![[2026-06-26-headroom-LLM-context压缩-architecture.png|800]]

### 5 个核心组件

| 组件 | 干啥 |
|---|---|
| **CacheAligner** | 稳定前缀，让 Anthropic / OpenAI KV cache 真正命中（避免 prompt 一改就 cache miss） |
| **ContentRouter** | 检测内容类型（JSON / 代码 / 文本），路由到对应压缩器 |
| **SmartCrusher** | 通用 JSON 压缩（数组、嵌套、混合类型） |
| **CodeCompressor** | AST 感知代码压缩（Python / JS / Go / Rust / Java / C++） |
| **Kompress-base** | 自家 HuggingFace 模型（`chopratejas/kompress-v2-base`），训练在 agentic traces 上 |
| **CCR** | ==可逆压缩== —— 原内容本地缓存，LLM 必要时通过 `headroom_retrieve` 拉回 |

---

## Output Token Reduction —— 独特亮点

> [!important] 不只压输入，还压输出
> 大部分压缩工具只管"送给 LLM 的 token"。Headroom 还管 ==LLM 写回来的 token==（Opus 类模型输出成本是输入的 5×）。

三个机制：

1. **Verbosity steering** —— 在 system prompt 末尾追加"保持简洁、不要重述上下文"，仍命中 prompt cache
2. **Effort routing** —— 模型只是接续 tool result（读了文件、测试通过）这种场景，自动降低 thinking effort
3. **`headroom learn --verbosity`** —— ==读你的历史会话==，自动学习你偏好的简洁度

```bash
export HEADROOM_OUTPUT_SHAPER=1     # 默认 off
headroom proxy --port 8787

headroom output-savings
# Reduction: 31.7%  (95% CI 27.7% … 35.7%)   [estimated]
```

> [!warning] 实测 vs 估计
> Output token 节省是**反事实的**（你看不到模型"本来会写什么"），所以 Headroom 给的是带置信区间的**估计**。想要"实测"就留 10% 会话做对照组：`HEADROOM_OUTPUT_HOLDOUT=0.1`。

---

## `headroom learn` —— 从失败中自动沉淀

> [!tip] 这个跟 [[2026-06-24-Claude-Code-动态工作流]] 是天然搭档
> `headroom learn` 会**挖掘失败的会话**，把纠正写到 `CLAUDE.md` / `AGENTS.md` / `GEMINI.md`——你下次起新会话，agent 自动"记住"之前踩的坑。

```bash
headroom learn --verbosity            # dry run，看会挖出啥
headroom learn --verbosity --apply    # 应用
```

---

## Cross-agent Memory

跨 agent 共享记忆：
- 同一个 store 给 Claude / Codex / Gemini 共用
- 自动 dedup
- agent provenance（谁产生的）

解决"换 agent 就丢上下文"的痛点。

---

## Agent 兼容性矩阵

| Agent | `headroom wrap` | 备注 |
|---|:---:|---|
| **Claude Code** | ✅ | 支持 `--memory` · `--code-graph` · `--1m`（1M context） |
| **Codex** | ✅ | 与 Claude 共享 memory |
| **Cursor** | 手动 | 启动 proxy，打印 base URL 让你粘到 settings |
| **Aider** | ✅ | 启动 proxy + 启动 agent |
| **Copilot CLI** | ✅ | 启动 proxy + 启动 agent |
| **OpenClaw** | ✅ | 安装为 ContextEngine 插件 |
| **OpenCode** | ✅ | 注入配置 + 启动 proxy + 启动 agent |
| **Cortex Code** | ✅ | 60-65% 节省，library 模式 |

任何 OpenAI-compatible 客户端都能用 `headroom proxy`。MCP-native 用 `headroom mcp install`。

---

## 集成方式（覆盖主要框架）

| 你的栈 | 怎么接 |
|---|---|
| Python / TS 应用 | `compress(messages, model=…)` |
| Anthropic / OpenAI SDK | `withHeadroom(new Anthropic())` |
| Vercel AI SDK | `wrapLanguageModel({ model, middleware: headroomMiddleware() })` |
| LiteLLM | `litellm.callbacks = [HeadroomCallback()]` |
| LangChain | `HeadroomChatModel(your_llm)` |
| Agno / Strands | `HeadroomAgnoModel(...)` |
| ASGI app | `app.add_middleware(CompressionMiddleware)` |
| MCP client | `headroom mcp install` |
| Multi-agent | `SharedContext().put / .get` |

---

## 竞品对比

| | 覆盖范围 | 部署 | 本地 | 可逆 |
|---|---|---|:---:|:---:|
| **Headroom** | ==全 context==：工具 / RAG / 日志 / 文件 / 历史 | Proxy / library / middleware / MCP | ✅ | ✅ |
| [RTK](https://github.com/rtk-ai/rtk) | CLI 命令输出 | CLI wrapper | ✅ | ❌ |
| [lean-ctx](https://github.com/yvgude/lean-ctx) | CLI / MCP / 编辑器 | CLI / MCP | ✅ | ❌ |
| Compresr / Token Co. | 文本 | Hosted API | ❌ | ❌ |
| OpenAI Compaction | 对话历史 | Provider-native | ❌ | ❌ |

> [!note] 一个有趣的事实
> Headroom 内置了 ==RTK 的二进制==做 shell-output 重写（`git show --short`、`ls` 范围限制、安装器摘要）。RTK 团队的工具是 Headroom 栈的一等公民。
>
> 也可以把 [lean-ctx](https://github.com/yvgude/lean-ctx) 当 CLI context tool 用：`HEADROOM_CONTEXT_TOOL=lean-ctx`

---

## 安装

```bash
pip install "headroom-ai[all]"          # Python，全特性
npm install headroom-ai                 # TS / Node
docker pull ghcr.io/chopratejas/headroom:latest
```

需要 **Python 3.10+**。

**Extras 可选**：`[proxy]` / `[mcp]` / `[ml]`（Kompress-base）/ `[code]` / `[memory]` / `[relevance]` / `[image]` / `[agno]` / `[langchain]` / `[evals]` / `[pytorch-mps]`（Apple GPU 嵌入器 offload）

**60 秒上手**：
```bash
pip install "headroom-ai[all]"
headroom wrap claude              # 包裹 Claude Code
headroom proxy --port 8787        # 或起 proxy，零代码改动
headroom perf                     # 看节省数据
headroom dashboard                # 实时面板（需 proxy 跑着）
```

> [!warning] 企业 SSL 检测环境常见坑
> - `CERTIFICATE_VERIFY_FAILED`：公司 MITM 代理拦截了 `cdn.pyke.io`（ONNX Runtime）或 `huggingface.co`（Kompress 模型）。需要先装 Rust 或 pre-download。
> - **Python 3.13+ strict mode 报错** `Basic Constraints of CA cert not marked critical`：公司 CA 没标 critical，被新版 strict 模式拒绝。设 `HEADROOM_TLS_STRICT=0` 解决（Headroom 控制的 TLS 上下文，chain 校验仍开着）。

---

## 适用 vs 不适用

> [!success] 适合
> - 每天跑 AI coding agent，想省 token 又不想改代码
> - 多 agent 协作，要共享 memory
> - 需要可逆压缩（CCR 期限内可 retrieve 原文）

> [!failure] 不适合
> - 只用单一 provider 的原生 compaction（如 OpenAI 自己的对话压缩），不需要跨 agent memory
> - 沙箱环境（本地进程跑不了）

---

## 相关笔记

- [[2026-06-24-Claude-Code-动态工作流]] — Claude Code Workflow，Headroom 跟它是天然搭档（workflow 消耗 token 多，Headroom 帮你省）
- [[AI工具与资源收藏]] — 同主题聚合
- [[Claude-多智能体协作5种模式]] — cross-agent memory 适用场景