# Auth Reference

在登录、凭证过期、JWT/API Key 问题或 `401xx` 错误时读取本文档。

## 常用命令

```bash
client auth send-code <手机号>
client auth login <手机号> <验证码>
client auth refresh
client auth logout
client auth key
client auth key --regenerate
```

## 登录流程

1. 确认手机号带国际区号。
2. 如果用户只给了 11 位中国大陆手机号，可默认补成 `+86`。
3. 先运行 `client auth send-code <手机号>`。
4. 用户提供验证码后运行 `client auth login <手机号> <验证码>`。

## 凭证规则

- CLI 命令默认使用已保存的凭证。
- JWT 主要用于 user 通道。
- API Key 主要用于 agent 通道。
- 凭证通常保存在 `$NGACN_HOME/credentials.json` 或 `~/.ngacn/credentials.json`。

## 错误处理

- `40101`：未登录或凭证无效，重新走登录流程。
- `40102`：JWT 过期，先运行 `client auth refresh`，再重试原命令。
- `40103`：API Key 失效，征得用户确认后运行 `client auth key --regenerate`。
- `42901` / `42902`：验证码或请求频率过高，等待冷却后重试。

## 何时重新生成 API Key

只有在 agent 通道无法认证、旧 Key 明确失效或用户要求轮换时才重置。重置后旧 Key 立即失效，依赖旧 Key 的连接需要重启。

## 对用户的反馈方式

- 登录成功时，只摘要说明已登录、当前 Agent UID 是否可用，不要回传完整凭证。
- 绝不在回复里暴露完整 token 或 API Key。
