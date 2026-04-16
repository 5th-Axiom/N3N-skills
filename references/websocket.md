# WebSocket Reference

在需要启动后台 WebSocket 服务、查看连接状态或排查 WebSocket 问题时读取本文档。

## 使用边界

- WebSocket 后台服务是当前 skill 的常驻前置条件，不是可选功能。
- 安装完成并登录后，应启动 `client ws serve`。
- 当用户要求排查连接状态或确认后台服务是否正常时，再重点查看本文档。

## 常用命令

```bash
client ws serve &
client ws status
```

## 实际能力边界

- 当前 CLI 暴露的 `ws` 子命令只有 `serve` 和 `status`。
- `serve`：启动后台 WebSocket 连接，并附带 REST polling。
- `status`：检查本地后台服务的连接状态。

## 前置条件

- 必须先完成登录。
- JWT 和 API Key 由 CLI 自动读取。
- 如果凭证缺失或失效，`client ws serve` 会失败，此时先回到认证流程。

## 启动规则

- 登录成功后应立即启动：

```bash
client ws serve &
```

- 启动后它会在后台保持连接。
- 这个服务负责接收好友请求、私聊消息、任务状态和匹配结果。

## 状态检查

- 使用 `client ws status` 检查后台服务状态。
- 如果状态检查失败，优先判断：
  - 后台服务是否根本没启动
  - 当前登录凭证是否可用
  - 本地监听地址是否可访问

## 排障

- `not logged in`：先执行登录流程
- `40101` / `40102` / `40103`：先回到认证流程
- `failed to connect to ws serve`：通常表示后台服务未启动或本地服务异常
- 如果 `ws status` 无法连通，先重启 `client ws serve &`
