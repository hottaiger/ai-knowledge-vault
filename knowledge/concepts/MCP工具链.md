---
concept: MCP工具链
aliases:
  - Model Context Protocol Tools
  - MCP Tools
---

# MCP 工具链

## 定义

**Model Context Protocol (MCP)** 是 Anthropic 提出的开放协议，让 AI 模型/Agent 通过标准化 JSON-RPC 接口调用外部工具/数据源。GitNexus、Playwright、Sequential Thinking 等能力型工具都基于 MCP 暴露给 Claude Code / Cursor / Codex 等客户端。

核心思想：**工具能力 = 一个标准接口 + 一组 Resources/Tools/Prompts**。客户端无需为每个工具写适配。

## 关键洞见

- **MCP 工具和 Skill 是两套机制**——MCP 工具是协议级能力（实时调用），Skill 是文档级工作流（指导 Agent 何时调用）
- **GitNexus 的 6 个 Skill 路由表**（gitnexus-guide / -exploring / -impact-analysis / -debugging / -refactoring / -cli）就是典型的"协议 + 文档"组合
- **零 Token 索引 + MCP 实时查询**是 GitNexus 模式的核心优势——AI 不用把整个图谱塞进 context window
- **CLAUDE.md / AGENTS.md 把"先 impact 后改"写成硬约束**——工具 + 项目规则双管齐下，合规性靠人而非工具
- **MCP 服务器全局注册在 `~/.gitnexus/registry.json`**——按 `repoPath` 索引，与工作目录无关，worktree 也能用

## 代表条目

- [[2026-06-12-GitNexus-代码知识图谱与AI编码工作流]] — GitNexus 暴露的 7 个 MCP 工具详解

## 7 个 GitNexus MCP 工具速查

| 工具 | 作用 |
|------|------|
| `query` | 进程感知混合搜索（BM25 + 语义向量） |
| `context` | 360° 符号视图（上下游调用关系） |
| `impact` | 爆炸半径分析 |
| `detect_changes` | Git diff 风险评估 |
| `rename` | 跨文件协调重命名 |
| `cypher` | 原始图查询（Neo4j 风格） |
| `list_repos` | 全局仓库注册表 |

## 相关概念

- [[concepts/代码知识图谱]] — MCP 工具消费的核心数据源
- [[第二大脑]] — MCP 协议的"工具 = 数据 + 协议"哲学和知识管理的"条目 + 链接"哲学同源
- [[2026-04-21-Article-to-HTML-文章转信息图-Skill]] — 同样是给 Agent 暴露能力的 Skill 模式

## 开放问题

- MCP 工具和原生 function calling 的边界在哪里？什么时候该走 MCP？
- 多个 MCP 服务器（GitNexus + Playwright + Sequential Thinking）同时挂载时，Agent 的工具选择策略如何训练？
- MCP 工具的"安全白名单"机制目前还很原始，企业内如何管控敏感工具？
