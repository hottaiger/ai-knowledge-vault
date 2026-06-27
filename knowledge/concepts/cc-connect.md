---
concept: cc-connect
aliases:
  - cc-connect IM 桥
  - Cursor Agent 飞书桥接
---

# cc-connect

## 定义
cc-connect 作为本地中间层进程，桥接飞书/钉钉等 IM 与 Cursor Agent CLI，让 IM 里直接驱动 AI 编码 Agent。

## 关键洞见
- 飞书权限名照抄搜不到：INSTALL.md 把事件标识（im.message.receive_v1，带点）和权限标识（带冒号）混了。搜权限要去 docs/feishu.md 第四章拿 5 个正确列表；事件订阅单独去「事件与回调」菜单加。（来源：[[2026-06-22-cc-connect安装配置避坑指南]])
- 三层架构：cc-connect（中间层，本地进程）跟飞书 IM 保持 WebSocket 长连接；维护用户↔会话映射（~/.cc-connect/sessions/）；做权限检查（allow_from / admin_from）；把消息拼成完整 prompt，spawn 子进程喂给 Cursor Agent CLI；监听 JSON stream 输出实时回传…（来源：[[2026-06-20-cc-connect工作原理]])

## 代表条目
- [[2026-06-22-cc-connect安装配置避坑指南|cc-connect 安装配置避坑指南（实战 9 条）]]
- [[2026-06-20-cc-connect工作原理|cc-connect 工作原理]]

## 相关概念
- [[concepts/OpenClaw与Agent编排]]
- [[concepts/Claude协作工作流]]

## 开放问题
- cc-connect 之外是否还需要一个通用 IM Agent 网关，能跨多平台同时挂多 Agent？

## Dataview
```dataview
TABLE date, source, tags, confidence
FROM "knowledge"
WHERE file.folder = "knowledge" AND contains(concepts, this.file.link)
SORT date DESC
```
