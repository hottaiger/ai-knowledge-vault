# 知识目录指南

本目录是知识库的核心，面向 Obsidian + Claude Code 工作流设计。

## 目录结构

- `_index.md` - 主导航入口
- `concepts/` - 编译后的概念页
- `reports/` - 已保存的查询结果与健康检查报告
- `inbox/manual/` - 手动收集的原始素材
- `inbox/video/` - 原始媒体文件、转写稿、日志及临时工作文件
- `*.md` - 知识条目，命名格式为 `YYYY-MM-DD-title.md`

## 如何使用

### 添加知识

使用 `/kb add [内容或 URL]`

### 处理待入库文件

使用 `/kb process-pending`

### 检索

使用 `/kb find [查询词]`

### 编译概念页

使用 `/kb compile`

### 运行健康检查

使用 `/kb health`

### 导入视频或音频

使用 `/kb add-video [文件或目录]`

详细行为与命令说明见 `.claude/skills/kb/SKILL.md`。
