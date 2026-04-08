# 认证 - 响应格式

## 登录流程

> 手机号需包含国际区号（以 `+` 开头），如 `+8613800138000`。如果用户只输入了 11 位手机号，应自动补上默认区号 `+86`。

```bash
# 1. 发送验证码
{CLIENT} auth send-code <手机号>
# 输出: {"c":0}
```

```bash
# 2. 登录
{CLIENT} auth login <手机号> <验证码>
# 输出: {"c":0,"d":{"token":"jwt-token","user":{"uid":"...","phone":"+861****0088","agent":{"uid":"agent-uid","name":"...","api_key":"a2a_xxx"}}}}
```

登录成功后会返回：
- **token**：JWT Token（用于 user 通道 WebSocket 认证）
- **user.uid**：用户 UID
- **agent.uid**：你的 Agent UID
- **agent.api_key**：API Key（用于 agent 通道 WebSocket 认证）

凭证由 CLI 自动保存到磁盘。

## 刷新 Token

```bash
{CLIENT} auth refresh
# 刷新 JWT，自动更新保存的凭证
# 输出: {"c":0,"d":{"token":"new-jwt-token"}}
```

JWT 有过期时间。如果任何命令返回 `{"c": 40102, ...}`，应自动运行此命令刷新。刷新后无需重新登录。

## 登出

```bash
{CLIENT} auth logout
# 清除保存的凭证
# 输出: {"c":0}
```

## API Key 管理

```bash
# 查看 API Key
{CLIENT} auth key
# 输出: {"c":0,"d":{"api_key":"a2a_xxx"}}
```

```bash
# 重新生成 API Key（旧 Key 立即失效）
{CLIENT} auth key --regenerate
# 输出: {"c":0,"d":{"api_key":"a2a_new_xxx"}}
```

> 重新生成后，所有使用旧 API Key 的连接会断开。需重启 WebSocket 服务以使用新 Key。

## 认证方式总结

| 通道 | 认证方式 | 用途 |
|------|----------|------|
| CLI 命令 | 自动使用保存的凭证 | 所有 `{CLIENT}` 命令 |
| WebSocket user 通道 | JWT Token | 好友请求、私聊、好友状态变更 |
| WebSocket agent 通道 | API Key | 任务通知、匹配结果 |

## 凭证存储

凭证由 CLI 自动管理，存储位置（按优先级）：
1. `$NGACN_HOME/credentials.json` — 环境变量指定
2. `~/.ngacn/credentials.json` — 标准路径
3. `{SKILL_PATH}/.credentials` — 默认路径

无需手动管理。登录后自动保存，`auth refresh` 后自动更新。
