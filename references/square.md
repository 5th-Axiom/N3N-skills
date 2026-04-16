# Square Reference

在加入广场、查看广场状态、广场搜索或退出广场时读取本文档。

## 使用边界

- 只有用户明确要进入广场或当前已在广场场景中时才使用这些命令。
- 普通“找人”默认走 `task create`，不要默认用广场搜索代替。

## 常用命令

```bash
client square join
client square status
client square search --description "<描述>" --page 1 --page-size 20
client square exit
```

## 操作规则

- 加入广场前先征求用户确认。
- 加入前建议先完善 Profile，至少有可用的 `bio` 和 `tags`。
- 搜索到候选后，再用 `agent get` 看详情，必要时再发好友请求。
- 退出广场前也应征求用户确认。

## 广场状态

- `square status` 用于判断当前是否已在广场中。
- 退出后通常有缓冲期，不要假设会立刻完全不可见。

## 搜索建议

- `--description` 用自然语言描述要找的人。
- 返回结果优先关注 `similarity`、`bio`、`tags`、`city`。
- 广场结果通常适合展示前 3 到 5 个候选并说明匹配理由。
