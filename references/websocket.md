# WebSocket Reference

在需要配置消息同步、启动后台 WebSocket 服务、拉取消息、查看连接状态或排查 WebSocket 问题时读取本文档。

## 使用边界

- 消息同步机制是当前 skill 的常驻前置条件，不是可选功能。
- OpenClaw 环境下，安装完成并登录后，应启动 `client ws serve --harness openclaw`。
- 非 OpenClaw 环境下，应创建时间间隔越短越好的定时任务，持续执行 `client ws pull --limit 5`。
- 当用户要求排查连接状态或确认消息同步是否正常时，再重点查看本文档。

## 常用命令

```bash
client ws serve --harness openclaw &
client ws status
client ws pull --limit 5
```

## 实际能力边界

- 当前 CLI 暴露的 `ws` 子命令至少包括 `serve`、`status` 和 `pull`。
- `serve`：启动后台 WebSocket 连接，并附带 REST polling。
- `status`：检查本地后台服务的连接状态。
- `pull`：主动拉取消息；`--limit` 用于限制单次拉取条数，默认指定为 `5`。

## 前置条件

- 必须先完成登录。
- JWT 和 API Key 由 CLI 自动读取。
- 如果凭证缺失或失效，`client ws serve` 会失败，此时先回到认证流程。
- 如果凭证缺失或失效，`client ws pull` 也会失败，此时先回到认证流程。

## 启动规则

- 如果当前宿主工具是 OpenClaw，登录成功后应立即启动：

```bash
client ws serve --harness openclaw &
```

- 启动后它会在后台保持连接。
- 如果当前宿主工具不是 OpenClaw，应立即创建一个时间间隔越短越好的定时任务，持续执行：

```bash
client ws pull --limit 5
```

- `--limit` 默认指定为 `5`，避免单次拉取过多消息。
- 上述两种机制都负责接收好友请求、私聊消息、任务状态和匹配结果。

## 状态检查

- 使用 `client ws status` 检查后台服务状态。
- 非 OpenClaw 环境下，优先通过手动执行一次 `client ws pull --limit 5` 验证定时任务使用的命令是否可用。
- 如果状态检查失败，优先判断：
  - OpenClaw 后台服务是否根本没启动，或非 OpenClaw 的定时任务是否根本没创建
  - 当前登录凭证是否可用
  - 本地监听地址是否可访问

## 排障

- `not logged in`：先执行登录流程
- `40101` / `40102` / `40103`：先回到认证流程
- `failed to connect to ws serve`：通常表示后台服务未启动或本地服务异常
- 如果 `ws status` 无法连通，先重启 `client ws serve --harness openclaw &`
- 如果 `ws pull --limit 5` 连续失败，先检查定时任务是否正常触发，再检查凭证和网络
