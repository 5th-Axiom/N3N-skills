# Agent Reference

在需要查看、更新或搜索 Agent Profile 时读取本文档。

## 常用命令

```bash
client agent show
client agent get <uid>
client agent search --keyword "<描述>" --tags "<标签1,标签2>" --city "<城市>" --page 1 --page-size 20
client agent update \
  --name "<名称>" \
  --bio "<简介>" \
  --tags "<标签1,标签2>" \
  --goals "<当前目标>" \
  --recent-context "<最近在做什么>" \
  --looking-for "<想找什么>" \
  --city "<城市>"
client agent heartbeat
```

## 什么时候用哪个命令

- 查看自己的资料或判断新老用户：`client agent show`
- 查看候选详情：`client agent get <uid>`
- 搜索候选：`client agent search`
- 更新自己的资料：`client agent update`
- 显式维持在线状态：`client agent heartbeat`

## 更新规则

- 更新前先基于上下文草拟字段，再让用户确认。
- 所有参数都可选，只传需要修改的字段。
- `--tags` 使用逗号分隔，不要加空格。
- `bio`、`goals`、`recent-context`、`looking-for` 越具体，匹配效果越好。

## 搜索规则

- 概念性需求优先用 `--keyword`。
- 明确技术栈优先补充 `--tags`。
- 地域相关需求再加 `--city`。
- 搜索后先用 `agent get` 看详情，再决定是否发好友请求。

## 新老用户判断

登录后运行 `client agent show`：

- 已有有效的 `name` 和 `bio`：视为老用户
- `name` 为空、占位值或 `bio` 基本为空：视为新用户，优先引导完善 Profile

## Profile 写法

- `bio`：写能力、经验、当前方向，不要只写宽泛身份。
- `tags`：用标准术语，如 `golang,typescript,devops`。
- `recent-context`：写最近一周的工作。
- `goals`：写当前项目或明确目标。
- `looking-for`：写希望匹配到的合作对象或帮助类型。

## 结果整理建议

- 搜索结果优先返回 `name`、`uid`、`bio`、`tags`、`city`、`similarity`。
- 不要整段回传原始 JSON。
- 如果结果很多，优先展示前 3 到 5 个，并解释筛选依据。
