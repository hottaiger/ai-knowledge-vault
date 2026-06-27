## openspec/changes/archive/2026-05-26-hide-footer-bar-on-installment-tab和openspec/specs/footer-bar-visibility的区别

简单说：

**`openspec/changes/archive/.../specs/footer-bar-visibility/spec.md`** — 这是**变更过程的快照**，属于这次 change 的归档记录。记录的是"这次改动期间写的 delta spec"，跟着 change 一起归档，只是历史留档。

**`openspec/specs/footer-bar-visibility/spec.md`** — 这是**项目级主 spec**，是"当前有效的规格文档"。归档时 comet-archive 脚本把 delta spec 同步过来覆盖了这里。这个目录下的内容代表项目当前对 `footer-bar-visibility` 这个能力的权威描述，后续新的 change 如果涉及这个能力，会在这里继续更新。

---

## 类比一下：

- `archive/...` 里的 spec = 某次 PR 的附带文档，存档用
- `openspec/specs/` 里的 spec = 项目的 wiki/规格库，持续维护

两个文件内容现在是一样的（刚同步过来），但随着后续迭代，`openspec/specs/` 里的会继续演进，`archive/` 里的永远是那次 change 时的状态。


.md文件的提交策略

必须提交：openspec/specs/footer-bar-visibility/ + 删除记录（D openspec/changes/.../tasks.md）
建议提交：openspec/changes/archive/...（完整归档，方便溯源）
跳过：docs/superpowers/ 下三个文件（内容已包含在归档目录里，重复）