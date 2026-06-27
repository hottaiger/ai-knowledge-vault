---
source: https://github.com/Ceeon/openclaw-article-to-image
tags: AI工具, 内容创作, 小红书, 信息图, OpenClaw
concepts:
- '[[concepts/Claude Code Skills]]'
related:
- '[[concepts/Claude Code Skills]]'
- '[[2026-04-21-Vibe-Security-AI代码安全审计技能]]'
- '[[2026-04-23-Claude-多智能体协作5种模式]]'
title: Article-to-HTML — 文章转小红书信息图 Skill
created: 2026-04-21
---
# Article-to-HTML — 文章转小红书信息图 Skill

> OpenClaw Skill：将文章/推文转化为手机可读的 HTML 信息图，自动匹配视觉风格，截图即用。

## 效果

给 bot 发一个链接或一段文字，自动生成信息图：

```
文生图 https://x.com/xxx/status/xxx
```

## 核心特点

- **AI 自由设计** — 不套模板，根据内容调性自动选择视觉方向
- **手机适配** — 1080px 画布，字号 ×3 设计，手机上清晰可读
- **后处理兜底** — 自动修复 CSS 边距、溢出、字号等常见问题
- **9 种灵感方向** — 商务简报、暗色科技、杂志编辑、手账笔记、报纸新闻等

## 设计理念

**你是设计师，不是模板填充机。** 根据文章内容自由设计视觉风格，不要每次都用同一个模板。

- **内容决定形式** — 严肃分析用深色/衬线，轻松科普用手账/波普，技术教程用终端/蓝图
- **信息精炼** — 一张图讲清一个核心论点 + 3-4 个支撑模块，不是把文章全文塞进去
- **手机第一** — 图片在手机上缩放 3 倍，字号必须够大
- **每次不同** — 配色、字体、布局每次都应该有变化，避免千篇一律

## 工作流程

```
用户给文章
    ↓
① 内容分析 — 提取核心论点、关键数据、对比维度、避坑要点、金句
    ↓
② 自由设计 — 根据内容调性，自主决定视觉风格、配色、布局
    ↓
③ 生成 HTML — 自包含 HTML，必须遵守技术底线规则
    ↓
④ 后处理 — 必须执行 post-process.sh
    ↓
⑤ 截图交付 — Chrome DevTools 截图
```

**步骤④ 是强制的。** 无论生成的 HTML 多有信心，都必须跑后处理脚本。

## 技术底线（必须遵守）

| 规则 | 值 |
|------|-----|
| 容器宽度 | `width: 1080px`（不用 max-width） |
| 容器高度 | `min-height: 1440px`（不用 height） |
| H1 字号 | ≥ 72px |
| 正文字号 | ≥ 30px |
| 最大栏数 | 2 栏 |
| overflow | 禁止 hidden |

## 文件结构

```
SKILL.md                 # 入口：工作流 + 设计理念
rules/
  01-技术底线.md          # 字号、宽度、overflow 硬约束
  02-截图流程.md          # Chrome DevTools 截图步骤
  03-风格灵感.md          # 9 种视觉方向灵感参考
scripts/
  fix-html.js            # CSS 后处理修复
  post-process.sh        # 一键后处理
templates/               # 9 个 HTML 模板（灵感参考）
```

## 安装

```bash
cp -r . ~/.openclaw/skills/article-to-html
```

## 适用场景

关键词：文章转图、笔记转图、信息图、转小红书图、做张图、可视化这篇文章、文生图

## License

MIT