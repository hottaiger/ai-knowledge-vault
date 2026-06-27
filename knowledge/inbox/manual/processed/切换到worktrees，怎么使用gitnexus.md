# Effect of .gitignore on gitnexus skills
_Exported on 2026/6/5 at GMT+8 16:59:50 from Cursor (3.6.31)_

---

**User**

如果我把.gitnexus加到.gitignore中，切换到worktrees目录，gitnexus相关skill还能生效吗

---

**Cursor**

我来查看相关的 skill 文件和项目结构来回答这个问题。

现在我有足够信息来回答了。以下是对两个问题的分析：

---

## 结论

### 1. 把 `.gitnexus` 加入 `.gitignore`

**不影响 GitNexus skill 的运行。**

`.gitignore` 只控制 git 是否追踪该目录，`.gitnexus/` 目录本身依然存在于磁盘上。GitNexus MCP 服务器读取的是本地文件系统，不依赖 git 是否追踪它。

加入 `.gitignore` 是推荐做法（索引文件属于本地缓存，不需要提交）。

---

### 2. 切换到 git worktrees 目录后，skill 还能生效吗

**部分生效，但有一个关键问题：**

| 能力 | worktree 中是否有效 | 原因 |
|------|-----|------|
| `gitnexus_query` / `gitnexus_impact` / `gitnexus_context` | ✅ 有效 | MCP 服务器全局注册在 `~/.gitnexus/registry.json`，按 `repoPath` 索引，与当前工作目录无关 |
| `gitnexus_detect_changes` | ⚠️ 可能不准 | 该工具读取 git diff，worktree 和主目录的 diff 是分开的，但 index 对应的是主 worktree 的文件路径 |
| Skill 文件本身（SKILL.md）| ❌ 默认不可用 | worktree 是独立的工作目录，不包含 `.cursor/skills/`、`.claude/skills/` 等文件，除非你把这些文件也放进去 |
| `npx gitnexus analyze` | ⚠️ 需单独运行 | 在 worktree 目录运行会建立一个新索引，对应 worktree 路径；或者手动指向主仓库 |

---

### 推荐做法

如果你需要在 worktree 中使用 GitNexus skill：

```bash
# 在 worktree 目录中重新建索引（会以 worktree 路径注册）
npx gitnexus analyze
```

或者直接让 worktree 共享主仓库的索引（通过软链接）：

```bash
# 在 worktree 根目录
ln -s /Users/zhangshuo12/Documents/guazi/detail-taro3/.gitnexus .gitnexus
```

这样既不需要重新分析，也能复用已有的知识图谱。

---

本次回复未使用任何 skill。

---

**User**

.gitnexus 目录的作用是什么

---

**Cursor**

`.gitnexus` 目录是 GitNexus 在本地存储**代码知识图谱索引**的地方。

## 目录结构

从你的项目来看，它包含：

```
.gitnexus/
├── meta.json       # 索引元数据（仓库路径、最后提交、统计信息）
├── lbug            # 图数据库文件（LevelDB/本地图存储）
└── parse-cache/    # 文件解析缓存（加速增量重建）
```

## 存储的内容

根据 `meta.json`，当前索引了：

| 指标 | 数量 |
|------|------|
| 文件数 | 858 |
| 符号节点 | 8,647 |
| 关系边 | 12,633 |
| 功能聚类 | 211 |
| 执行流程 | 120 |

这些数据构成一张**代码关系图**，记录了函数调用、文件导入、类继承、接口实现等关系。

## 为什么存本地

- **MCP 服务器**（`user-gitnexus`）读取这里的数据来响应 `gitnexus_query`、`gitnexus_impact` 等工具调用
- 构建一次后，AI 可以直接查询，不需要每次重新分析源码
- `parse-cache` 使得增量更新（`npx gitnexus analyze`）只重新解析变更过的文件，速度更快

## 是否需要提交到 git

**不需要**，建议加入 `.gitignore`：
- 它是本地构建产物，类似 `node_modules`
- 文件较大且频繁变更
- 每个开发者在本地运行 `npx gitnexus analyze` 即可重建

---

本次回复未使用任何 skill。

---

**User**

那么在worktree执行 npx gitnexus analyze 会重新生成.gitnexus ?

---

**Cursor**

是的，但行为上有几个关键细节需要了解：

## 会发生什么

在 worktree 目录执行 `npx gitnexus analyze` 时：

1. **在 worktree 根目录生成新的 `.gitnexus/`**，与主仓库的 `.gitnexus/` 完全独立
2. **在全局注册表中新增一条记录**，`~/.gitnexus/registry.json` 会同时存在两个条目：
   ```json
   // 主仓库
   { "repoPath": "/Users/.../detail-taro3", "name": "detail-taro3" }
   
   // worktree（路径不同，可能同名）
   { "repoPath": "/Users/.../worktrees/feature-xxx", "name": "detail-taro3" }
   ```

## 潜在问题

- **名称冲突**：两个仓库可能被注册为同一个 `name`（`detail-taro3`），MCP 工具按名称查找时可能取到错误的那个
- **重新分析耗时**：858 个文件、8647 个符号，首次分析需要几分钟
- **索引不共享**：主仓库的修改不会自动同步到 worktree 的索引，反之亦然

## 更推荐的做法

如果 worktree 的代码和主仓库差异不大（只是特性分支），用软链接复用现有索引更划算：

```bash
# 在 worktree 根目录
ln -s /Users/zhangshuo12/Documents/guazi/detail-taro3/.gitnexus .gitnexus
```

这样 MCP 工具直接读主仓库的索引，无需重新分析，也不会产生注册冲突。只有当 worktree 分支改动较大、需要独立的精确索引时，才值得单独 analyze。

---

本次回复未使用任何 skill。

---

**User**

# 在 worktree 根目录
ln -s /Users/zhangshuo12/Documents/guazi/detail-taro3/.gitnexus .gitnexus解释这个命令，执行了之后会什么弊端，怎么恢复？

---

**Cursor**

## 命令解释

```bash
ln -s /Users/zhangshuo12/Documents/guazi/detail-taro3/.gitnexus .gitnexus
```

| 部分 | 含义 |
|------|------|
| `ln -s` | 创建**符号链接**（软链接），类似 Windows 快捷方式 |
| `/Users/.../detail-taro3/.gitnexus` | 链接指向的**真实目录**（主仓库） |
| `.gitnexus` | 在**当前目录**（worktree）创建的链接名 |

执行后，worktree 中的 `.gitnexus` 只是一个指针，实际读写都发生在主仓库的 `.gitnexus/` 上。

---

## 弊端

### 1. 写操作相互污染
如果在 worktree 中运行 `npx gitnexus analyze`，**会覆盖主仓库的索引**，因为两者共享同一份数据。主仓库打开后读到的是 worktree 分支的索引。

### 2. 索引内容不匹配
worktree 可能是一个功能分支，代码结构与主仓库不同。此时 `gitnexus_impact` / `gitnexus_query` 返回的结果基于旧索引，可能出现"找不到符号"或"调用链错误"的情况。

### 3. 主仓库删除 `.gitnexus` 后链接失效
如果主仓库执行 `npx gitnexus clean`，软链接变成**悬空链接**（dangling symlink），读取时报错。

---

## 如何恢复

只需删除软链接本身，**不会影响主仓库的真实数据**：

```bash
# 在 worktree 根目录执行
rm .gitnexus
```

> 注意：必须用 `rm`，不要用 `rm -rf .gitnexus/`（带斜杠会删除链接指向的真实目录内容）

删除后 worktree 回到无索引状态，可以根据需要选择：
- 重新软链接
- 或单独运行 `npx gitnexus analyze` 生成独立索引

---

本次回复未使用任何 skill。

