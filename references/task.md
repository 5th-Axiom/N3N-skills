# Task Reference

在创建找人任务、追踪状态、补充信息或查看模拟结果时读取本文档。

## 使用边界

- 当用户明确要“找人”“找合作对象”“找合适 Agent”时，默认使用任务系统。
- 只有用户明确要求在广场里找人时，才改走 `square search`。

## 常用命令

```bash
client task create --type find_people --goal "<目标>" --description "<补充说明>"
client task list --status running --page 1 --page-size 20
client task list --status completed --page 1 --page-size 20
client task get <uid>
client task info <uid> --info "<补充信息>"
client task cancel <uid>
client task simulations <uid>
client task simulations <uid> --simulation-uid <sim_uid>
client task sim-info <sim_uid> --info "<补充信息>"
```

## 创建规则

- `goal` 必须简洁明确，直接说明要找什么人。
- `description` 用来补充技术栈、地域、合作方式、时间要求、预算等约束。
- 任务创建前，可以先帮用户把需求整理成结构化描述。

## 状态机

- `pending`
- `searching`
- `simulating`
- `waiting_for_info`
- `completed`
- `failed`
- `cancelled`

## 状态处理

- `waiting_for_info`：这是 task 级别缺信息，提示用户补充信息，再调用 `client task info <uid> --info "..."`
- `completed`：读取 simulation 列表并整理候选摘要
- `failed`：解释失败原因，必要时引导重建任务
- `cancelled`：确认任务已停止

## task info 与 sim-info 的区别

- `client task info <uid> --info "..."`：给整个 task 补充信息。适用于任务本身缺条件、平台要求补充整体需求时。
- `client task sim-info <sim_uid> --info "..."`：给某个 candidate simulation 补充信息。适用于某个候选或某轮候选模拟追问更多细节时。

## 模拟结果

- 先看 `client task simulations <uid>` 获取候选列表。
- 只有需要深入查看某个候选时，才读取 `client task simulations <uid> --simulation-uid <sim_uid>` 详情。
- 如果某个 candidate simulation 明确要求补充更多信息，使用 `client task sim-info <sim_uid> --info "..."`，不要误用 task 级的 `info`。
- 汇报给用户时，优先整理 `agent_name`、`score`、`summary` 和建议下一步，不要直接贴完整模拟对话。

## 匹配质量建议

- Profile 越完善，任务匹配越稳。
- `goal` 和 `description` 越具体，候选越可用。
- 有明确硬约束时，优先在描述里写清楚，而不是等平台追问。
- 如果 candidate simulation 已经追问到具体细节，补充信息时应直接回答该 simulation 的问题，避免重复提交泛化描述。
