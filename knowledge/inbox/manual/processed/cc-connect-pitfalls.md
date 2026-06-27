# cc-connect 安装配置 · 避坑指南

> 实战环境：macOS 15 + Node 20 + cc-connect v1.3.4 + Cursor Agent CLI + 飞书开放平台
>
> 配合官方教程使用：https://github.com/chenhg5/cc-connect/blob/main/INSTALL.md
>
> 本指南**只记录本次实际踩过的坑**。INSTALL.md 没覆盖但通用的建议没列进来。

---

## 1. 飞书权限名照抄搜不到

**症状**：按 INSTALL.md 在「权限管理 → 权限配置」里搜 `im:message.receive_v1`，找不到。

**原因**：INSTALL.md 把**事件标识**（`im.message.receive_v1`，带点）当成权限标识列出。飞书开放平台把"权限"和"事件订阅"拆成两个独立菜单，搜的是权限（`im:message.receive_v1`，带冒号），但这条根本不是权限名。

**解决**：权限去 `docs/feishu.md` 第四章拿正确的列表，复制完整字符串去搜：

| 权限标识 | 作用 |
|---------|------|
| `contact:user.base:readonly` | 获取用户基本信息 |
| `im:message.p2p_msg:readonly` | 接收私聊消息 |
| `im:message.group_at_msg:readonly` | 接收群内 @机器人 消息 |
| `im:message.group_msg` | 读取群消息内容 |
| `im:message:send_as_bot` | 以机器人身份发送消息 |

事件订阅 `im.message.receive_v1` 单独去「事件与回调 → 事件配置 → 使用长连接接收事件 → 添加事件」里加。

---

## 2. Cursor Agent CLI 安装完看似可用，实则未认证

**症状**：`agent --version` 输出正常，`agent --print "ping"` 也回消息，但 cc-connect 一调它就报：

```
Error: Authentication required. Please run 'agent login' first,
or set CURSOR_API_KEY environment variable.
```

**原因**：INSTALL.md 第二步只说 `Verify your selected agent works: agent --version`，但 `--version` 不触发实际 API 调用。Cursor Agent CLI 的 OAuth 凭证（`~/.cursor/.cli-data/`）和 Cursor IDE 桌面凭证是分开的，IDE 登录过不代表 CLI 登录过。

**解决**：单独走一遍 CLI OAuth 登录：

```bash
NO_OPEN_BROWSER=1 agent login &
# 终端会打印一个 https://cursor.com/loginDeepControl?... 链接
# 浏览器打开 → 自动跳到 Cursor 授权页（已登录直接同意）→ 完成
```

`agent login` 在非交互 shell 里是阻塞的，必须 `&` 后台跑再去浏览器授权。

---

## 3. ⭐ Cursor mode=default 卡在 Workspace Trust

**症状**：飞书给机器人发消息，cc-connect 日志停在 `session spawned` 之后再无输出；直接命令行跑 `agent --print "你好"` 正常。最后报错：

```
⚠ Workspace Trust Required
Cursor Agent can execute code and access files in this directory.
Do you trust the contents of this directory?
/Users/zhangshuo12/Documents/guazi/detail-taro3
```

**原因**：Cursor Agent CLI 启动时检查 work_dir 是否在 trust 列表。第一次访问未 trust 目录，CLI 进入交互式确认等待 stdin。cc-connect 子进程没有 stdin 喂它，就死锁。cc-connect 在 mode=`"default"` 时不会自动传 `--force` / `--yolo` / `--trust`（看 `config.example.toml` Cursor 章节注释确认）。

**解决**：把 mode 改成 `"force"`：

```toml
[projects.agent.options]
mode = "force"  # 等同 --force/--yolo：跳过 trust 检查 + 自动放行所有工具
```

**关键**：INSTALL.md 完全没提这个坑，纯靠 debug 日志反推。

---

## 4. daemon install 之后 Status: Stopped

**症状**：`cc-connect daemon install` 显示成功，`cc-connect daemon start` 也说 started，但 `daemon status` 一直显示 Stopped，日志为空。

**原因**：之前用 `cc-connect > /tmp/cc-connect.log 2>&1 &` 前台启动后 kill 掉的，进程虽然退了，但 `~/.cc-connect/.config.toml.lock` 文件没清。新 daemon 拿不到 lock 就退出，launchd 看 PID 已退就报 Stopped。

**解决**：

```bash
rm -f ~/.cc-connect/.config.toml.lock
cc-connect daemon start
cc-connect daemon status   # 应看到 Status: Running + PID
```

---

## 5. 飞书 user open_id 从哪拿

**症状**：想配 `allow_from` 限制只有自己能跟机器人交互，但不知道怎么拿自己的飞书 user open_id。INSTALL.md 没明说。飞书官方 API（contact:user.get）需要先有 user_access_token。

**最快的办法**：给机器人发一条消息，然后看 daemon 日志：

```bash
cc-connect daemon logs -n 30 | grep "message received"
# 输出里 user=ou_xxxxxxxxxxxxxxxxxxxxxxxx 就是你的 user open_id
```

日志里还有 `bot_open_id`（机器人自己的，**不是你的**），别混了。

本次抓到的 open_id 格式：`ou_df9963c609c7d357ca50c090fdc22cef`。

---

## 6. macOS 没有 `timeout` 命令

**症状**：想"前台跑 cc-connect 几秒钟看启动日志然后自动退出"，用 `timeout 10 cc-connect` 报错 `command not found: timeout`。

**原因**：macOS 用 BSD userland，没装 GNU coreutils。

**绕开**：

```bash
cc-connect > /tmp/cc.log 2>&1 &
PID=$!
sleep 8
kill $PID
wait $PID 2>/dev/null
cat /tmp/cc.log
```

---

## 7. macOS 不展开 `~` 在部分命令里

**症状**：`mavis-trash '~/.cc-connect/.config.toml.lock'` 报 `cannot resolve path`。

**原因**：zsh 在某些子命令调用里不会自动把 `~` 展开成 `$HOME`。

**绕开**：直接用绝对路径：

```bash
mavis-trash /Users/zhangshuo12/.cc-connect/.config.toml.lock
```

---

## 8. 飞书卡片回调没配 = 按钮点了没反应

**症状**：飞书那边收到的是交互卡片（带"🤔 思考中..."、"🔧 Read..."、"✅ 已加好"等），但点"允许/拒绝"按钮没反应，飞书客户端一直转圈或超时。飞书后台会主动弹窗："该应用尚未配置卡片回调，一键完成配置后即可使用"。

**原因**：cc-connect 默认用飞书交互卡片显示进度和权限确认。用户点按钮时，飞书要把按钮事件推回来给 cc-connect 处理，靠的是 `card.action.trigger` 回调事件。没订阅这个回调，按钮事件就丢失了。

INSTALL.md 和 docs/feishu.md 第五步提到了，但容易被当成可选步骤跳过。

**解决**：

1. 点飞书后台弹窗里的「立即配置」一键加（飞书会自动跳到「事件与回调 → 回调配置」并勾选 `card.action.trigger`）
2. 如果没弹窗，手动去「事件与回调 → 回调配置 → 使用长连接接收事件 → 添加回调 → 卡片回调 (card.action.trigger)」勾上
3. ⚠️ **关键**：配完必须**重新创建一个应用版本并发布**，否则新配置不生效

```bash
# 操作步骤（飞书后台）：
# 1. 「版本管理与发布」→ 「创建版本」→ 填版本号和说明 → 保存
# 2. 「申请发布」→ 个人账号自动通过，企业账号需管理员审批
# 3. 等几分钟生效后，飞书发消息测试按钮
```

**临时绕开**（如果暂时没法发布版本）：在 `config.toml` 关闭卡片功能，回退纯文本模式：

```toml
[projects.platforms.options]
enable_feishu_card = false
```

关闭后所有交互走纯文本，权限确认靠直接回复"允许/拒绝"，不影响基本功能。

---

## 9. 团队成员用不了机器人（allow_from 限制）

**症状**：你在群里 @ 机器人能正常用，团队其他成员（同事、PM）@ 机器人回复"角色未授权，请联系管理员添加权限"。

**原因**：`config.toml` 的 `allow_from` 默认只写了你自己的 `open_id`，其他飞书用户都被白名单拦了。cc-connect 拒绝时**不写日志**（消息在到达业务逻辑前就被拦），所以 `daemon logs` 里看不到被拒用户的 open_id，没法从日志反推。

**解决**：加团队成员的 open_id 到 `allow_from` 列表。

**第一步：拿到新成员的 open_id**

三种方式（按推荐度排序）：

1. **让新成员在飞书给机器人发 `/whoami`**（最快）—— cc-connect 自带命令，机器人会回他自己的 user ID
2. **临时把 `allow_from = "*"`**，让对方发条消息，再看 `cc-connect daemon logs | grep "message received"` 抓 open_id，最后改回白名单
3. **让对方从飞书个人主页复制**（不一定有这个 UI，方法不一定稳定）

**第二步：改 config.toml**

```toml
[[projects.platforms]]
type = "feishu"

[projects.platforms.options]
app_id = "cli_xxx"
app_secret = "xxx"
# 多个 open_id 用英文逗号分隔
allow_from = "ou_你的open_id,ou_同事1,ou_同事2"
```

**第三步：重启 daemon**

```bash
cc-connect daemon restart
```

**⚠️ 安全提醒**：

- `allow_from` 是**白名单**，不是黑名单——加进来的人**都能用机器人**
- 配合 `mode = "force"` 时，加进来的人**都能改你项目代码**
- 团队场景下推荐分层：
  - **管理员**（你 + 1-2 个信任的人）→ 写 `admin_from`，能用 `/shell` `/restart` 等特权命令
  - **普通成员** → 只写 `allow_from`，能用基本功能但不能跑 shell
  - **临时外协** → 临时加进 `allow_from`，结束后删掉
- 多人项目建议**每人一个 cc-connect 实例 + 不同 work_dir**，不要共用一个项目共享 allow_from（容易误改）

---

## 实战总结 checklist

按 INSTALL.md 顺序走的时候，建议额外确认：

- [ ] 飞书权限是 `docs/feishu.md` 里的 5 个，不是 INSTALL.md 里那个事件名
- [ ] `agent login` 单独走一遍（不仅看 `--version`）
- [ ] `mode = "force"`（如果不想每次 IDE 手动 trust）
- [ ] daemon 起不来看一眼 `.config.toml.lock` 是不是残留
- [ ] 飞书 open_id 从 `daemon logs` 里 grep `message received` 拿
- [ ] 飞书后台订阅了 `card.action.trigger` 回调，且**重新发布了应用版本**
- [ ] 团队成员用 `/whoami` 拿 open_id 加到 `allow_from`（多人场景）

---

**参考：**
- INSTALL.md: https://github.com/chenhg5/cc-connect/blob/main/INSTALL.md
- docs/feishu.md: https://github.com/chenhg5/cc-connect/blob/main/docs/feishu.md
- config.example.toml: https://github.com/chenhg5/cc-connect/blob/main/config.example.toml