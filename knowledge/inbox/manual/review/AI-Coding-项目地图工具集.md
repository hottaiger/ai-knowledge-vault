# AI Coding 项目地图工具集

> 来源：微信公众号汇总 | 2026-05-11
> 状态：已并入 `gitnexus使用.md`（2026-06-12），本文件仅作历史保留

---

## 工具列表

### 1. Graphify
- **定位**：AI Coding 助手的"项目地图"
- **功能**：把代码库、文档、论文、图片一起编译成知识图谱
- **特点**：跨模态知识编译器，支持代码/文档/论文/图像/视频统一图谱
- **触发方式**：Skill 触发
- **输出**：Git 友好的静态产物（可团队共享）
- **相关链接**：https://mp.weixin.qq.com/s/apdD-t8cErWsdhBChyoNXA

### 2. Understand-Anything (code-review-graph)
- **定位**：Claude Code 神器，用知识图谱彻底搞懂代码库
- **功能**：给 Claude Code 装上代码地图，token 直降 6.8 倍
- **相关链接**：https://mp.weixin.qq.com/s/D850oxBUZzk-IcJ_VFpY1g
- **补充**：https://mp.weixin.qq.com/s/HJi8n8t5RXFCevn0ezmLNg

### 3. GitNexus
- **定位**：代码库的"神经系统"，让 AI Agent 不再盲目编辑代码
- **核心理念**：把代码仓库索引为知识图谱，通过 MCP 协议喂给 Codex/Claude Code/Cursor
- **官网**：https://www.aivi.fyi/llms/gitnexus

#### 核心特性
- **7个内置 MCP 工具**：
  - `impact` - 爆炸半径分析（改函数会波及哪些代码）
  - `context` - 360度符号视图（完整上下游关系）
  - `query` - 进程感知混合搜索（BM25 + 语义向量）
  - `detect_changes` - Git diff 风险评估
  - `rename` - 跨文件协调重命名
  - `cypher` - 原始图查询
  - `list_repos` - 全局仓库注册表
- **零 Token 索引**：索引/解析/聚类/图构建完全本地化，嵌入向量用本地 transformers.js 跑 Hugging Face 模型
- **多语言支持**：TypeScript/JavaScript/Python/Java/Kotlin/C#/Go/Rust/PHP

#### GitNexus vs Graphify 怎么选
| 维度 | GitNexus | Graphify |
|------|----------|----------|
| 定位 | AI Agent 的代码地图 | 跨模态知识编译器 |
| 关注点 | 调用链、爆炸半径、类型解析 | 代码/文档/论文/图像/视频统一图谱 |
| 索引 LLM 开销 | 零 Token | AST 零开销，语义通道有消耗 |
| 触发方式 | MCP 协议 | Skill 触发 |
| 输出 | 实时查询的结构化上下文 | Git 友好的静态产物 |

**选择建议**：
- 精准代码问题 → GitNexus（例："修改这个函数会影响哪些模块？"）
- 语义知识问题 → Graphify（例："这段实现和论文哪个部分对应？"）
- 复杂混合问题 → 两者叠加使用

### 4. zread.ai
- **定位**：AI/LLM 趋势资讯站
- **链接**：https://zread.ai/trending

5、实践
gitnexus，可以语义化查询，查看依赖关系。
用国内镜像安装：HF_ENDPOINT=https://hf-mirror.com gitnexus analyze --embeddings

## 标签
#AI-Coding #工具 #知识图谱 #MCP #Claude-Code #Graphify #GitNexus
