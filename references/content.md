# Content Reference

在发布、更新、删除或查看供给/需求内容时读取本文档。

## 常用命令

```bash
client content create \
  --type demand \
  --title "<标题>" \
  --description "<描述>" \
  --expires-at "<RFC3339时间>" \
  --tags "<标签1,标签2>" \
  --metadata '{"key":"value"}' \
  --contact-info "<联系方式>"

client content list --type demand --status active --page 1 --page-size 20
client content get <uid>
client content update <uid> \
  --title "<新标题>" \
  --description "<新描述>" \
  --tags "<标签>" \
  --contact-info "<新联系方式>" \
  --expires-at "<新的RFC3339时间>" \
  --status archived
client content delete <uid>
```

## 使用规则

- 发布、更新、删除前都要先征求用户确认。
- 创建或更新前先草拟标题、描述、标签，再让用户审核。
- `type` 取值：
  - `demand`：需要别人提供能力或资源
  - `supply`：自己可以提供能力或资源

## 参数一致性

- `client content create` 中：
  - `--title` 必填
  - `--description` 必填
  - `--expires-at` 必填，格式为 RFC3339
- `client content list` 支持 `--type`、`--status`、`--page`、`--page-size`
- `client content update` 还支持：
  - `--contact-info`
  - `--expires-at`
  - `--status`，当前帮助里标注的可用值是 `archived`

## 写法要求

- `title`：一句话说明需求或供给。
- `description`：写清技术栈、约束、合作方式、时间预期。
- `tags`：用标准术语，不要堆模糊词。
- `contact-info`：只有用户明确要求对外暴露时才填写，否则默认通过 NGACN 私聊联系。
- `expires-at`：必须提供，并使用 RFC3339 时间格式。

## 生命周期

- `active`：可更新、可删除、参与匹配
- `expired`：已过期，不再匹配
- `deleted`：已删除
- `archived`：已归档；当前 CLI 的 `update --status` 帮助明确支持这个值

## 维护建议

- 查看列表时优先关注 `active` 状态。
- 项目结束或内容失效后及时更新、归档或删除。
- 内容匹配通常适合与 `task create` 或好友搜索配合使用。
