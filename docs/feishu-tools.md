# 飞书插件工具使用手册

> OpenClaw 飞书插件工具完整指南

## 概述

OpenClaw 飞书插件提供了一套完整的工具，用于在飞书中进行文档管理、日历日程、任务管理、多维表格等操作。

大部分工具需要用户 **OAuth 授权**才能使用完整功能。

---

## 📄 文档工具

### feishu_create_doc

创建新的飞书云文档。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| title | string | 是 | 文档标题 |
| markdown | string | 是 | Markdown 内容 |
| folder_token | string | 否 | 父文件夹 token |
| wiki_node | string | 否 | 知识库节点 token |
| wiki_space | string | 否 | 知识空间 ID |

**示例**：
```markdown
# 创建文档
feishu_create_doc --title "测试文档" --markdown "# Hello World"
```

### feishu_fetch_doc

读取飞书云文档内容。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| doc_id | string | 是 | 文档 ID 或 URL |
| offset | number | 否 | 字符偏移量（分页用）|
| limit | number | 否 | 返回最大字符数 |

**返回**：文档标题和 Markdown 格式内容

### feishu_update_doc

更新云文档内容。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| doc_id | string | 是 | 文档 ID 或 URL |
| mode | string | 是 | 更新模式 |
| markdown | string | 是 | Markdown 内容 |

**mode 选项**：
- `overwrite` - 覆盖整个文档
- `append` - 追加到文档末尾
- `replace_range` - 替换指定范围
- `replace_all` - 替换所有匹配内容
- `insert_before` - 插入到指定位置之前
- `insert_after` - 插入到指定位置之后
- `delete_range` - 删除指定范围

### feishu_doc_comments

管理云文档评论。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | 操作类型：list/create/patch |
| file_token | string | 是 | 文档 token |
| file_type | string | 是 | 文档类型：doc/docx/sheet/wiki |
| elements | array | 否 | 评论内容元素（create 时必填）|

**elements 支持类型**：
- `text` - 纯文本
- `mention` - @用户
- `link` - 超链接

### feishu_doc_media

管理文档媒体（图片/文件）。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | 操作：insert/download |
| doc_id | string | 是 | 文档 ID |
| file_path | string | 是 | 本地文件路径 |
| type | string | 是 | 类型：image/file |
| align | string | 否 | 对齐方式：left/center/right |
| caption | string | 否 | 图片描述 |

⚠️ **注意**：file_path 必须在 `/tmp` 目录下

---

## 📅 日历工具

### feishu_calendar_calendar

列出日历。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | list/get/primary |
| calendar_id | string | 否 | 日历 ID |
| page_size | number | 否 | 分页大小 |

### feishu_calendar_event

管理日程（创建/查询/修改/删除）。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | create/list/get/patch/delete |
| summary | string | 否 | 日程标题 |
| start_time | string | 是 | 开始时间 |
| end_time | string | 是 | 结束时间 |
| user_open_id | string | 是 | 用户 open_id |
| description | string | 否 | 日程描述 |
| attendees | array | 否 | 参会人列表 |
| location | object | 否 | 地点信息 |
| reminders | array | 否 | 提醒列表 |

**时间格式**：ISO 8601 / RFC 3339
```
2026-03-12T11:00:00+08:00
```

### feishu_calendar_freebusy

查询忙闲状态。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| time_min | string | 是 | 查询起始时间 |
| time_max | string | 是 | 查询结束时间 |
| user_ids | array | 是 | 用户 open_id 列表（1-10个）|

---

## ✅ 任务工具

### feishu_task_task

管理任务。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | create/list/get/patch |
| summary | string | 是 | 任务标题 |
| description | string | 否 | 任务描述 |
| current_user_id | string | 是 | 当前用户 open_id |
| due | object | 否 | 截止时间 |
| start | object | 否 | 开始时间 |
| members | array | 否 | 任务成员 |

**时间格式**：ISO 8601
```json
{
  "timestamp": "2026-03-12T11:00:00+08:00",
  "is_all_day": false
}
```

### feishu_task_tasklist

管理任务清单。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | create/list/get/tasks/patch/delete |
| name | string | 否 | 清单名称 |
| tasklist_guid | string | 否 | 清单 GUID |
| members | array | 否 | 清单成员 |

### feishu_task_comment

管理任务评论。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | create/list/get |
| task_guid | string | 是 | 任务 GUID |
| content | string | 是 | 评论内容 |
| reply_to_comment_id | string | 否 | 回复的评论 ID |

### feishu_task_subtask

管理子任务。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | create/list |
| task_guid | string | 是 | 父任务 GUID |
| summary | string | 是 | 子任务标题 |
| description | string | 否 | 子任务描述 |

---

## 📊 多维表格工具

### feishu_bitable_app

管理多维表格应用。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | list/create/get/patch/delete/copy |
| name | string | 否 | 多维表格名称 |
| app_token | string | 否 | 多维表格 token |
| folder_token | string | 否 | 文件夹 token |

### feishu_bitable_app_table

管理数据表。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | list/create/patch/delete |
| app_token | string | 是 | 多维表格 token |
| table | object | 否 | 表定义（name, fields）|

**字段类型**：
| 类型码 | 类型 |
|--------|------|
| 1 | 文本 |
| 2 | 数字 |
| 3 | 单选 |
| 4 | 多选 |
| 5 | 日期 |
| 7 | 复选框 |
| 11 | 人员 |
| 13 | 电话 |
| 15 | 超链接 |
| 17 | 附件 |
| 1001 | 创建时间 |
| 1002 | 修改时间 |

### feishu_bitable_app_table_record

管理记录（行数据）。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | create/update/delete/list |
| app_token | string | 是 | 多维表格 token |
| table_id | string | 是 | 数据表 ID |
| fields | object | 是 | 记录字段 |
| record_id | string | 是 | 记录 ID |

**fields 格式**：
```json
{
  "文本": "内容",
  "数字": 100,
  "单选": "选项名",
  "多选": ["选项1", "选项2"],
  "日期": 1740441600000,
  "复选框": true,
  "人员": [{"id": "ou_xxx"}]
}
```

### feishu_bitable_app_table_field

管理字段（列）。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | create/list/update/delete |
| app_token | string | 是 | 多维表格 token |
| table_id | string | 是 | 数据表 ID |
| field_name | string | 否 | 字段名称 |
| type | number | 否 | 字段类型 |
| field_id | string | 否 | 字段 ID |

### feishu_bitable_app_table_view

管理视图。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | create/list/get/patch/delete |
| app_token | string | 是 | 多维表格 token |
| table_id | string | 是 | 数据表 ID |
| view_name | string | 否 | 视图名称 |
| view_type | string | 否 | 视图类型 |

**view_type**：grid/kanban/gallery/gantt/form

---

## 📊 电子表格工具

### feishu_sheet

管理飞书电子表格（与多维表格不同产品）。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | info/read/write/append/find/create/export |
| url | string | 否 | 表格 URL |
| spreadsheet_token | string | 否 | 表格 token |
| range | string | 否 | 范围，如 A1:D10 |
| values | array | 否 | 要写入的二维数组 |
| title | string | 否 | 表格标题 |

---

## 📚 知识库工具

### feishu_wiki_space

管理知识空间。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | list/get/create |
| space_id | string | 否 | 知识空间 ID |
| name | string | 否 | 知识空间名称 |
| description | string | 否 | 描述 |

### feishu_wiki_space_node

管理知识库节点。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | list/get/create/move/copy |
| space_id | string | 是 | 知识空间 ID |
| parent_node_token | string | 否 | 父节点 token |
| token | string | 否 | 节点 token |
| obj_type | string | 否 | 类型：doc/sheet/bitable |

---

## 🔍 搜索工具

### feishu_search_doc_wiki

搜索文档和知识库。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | search |
| query | string | 是 | 搜索关键词 |
| filter | object | 否 | 过滤条件 |

**filter 选项**：
- `doc_types` - 文档类型列表
- `creator_ids` - 创建者 ID
- `create_time` - 创建时间范围
- `open_time` - 打开时间范围
- `sort_type` - 排序方式

---

## 💬 消息工具

### feishu_im_user_get_messages

获取消息历史。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| chat_id | string | 否 | 会话 ID（与 open_id 二选一）|
| open_id | string | 否 | 用户 ID（与 chat_id 二选一）|
| page_size | number | 否 | 每页数量（1-50）|
| page_token | string | 否 | 分页标记 |
| sort_rule | string | 否 | 排序：create_time_asc/desc |
| relative_time | string | 否 | 相对时间 |
| start_time | string | 否 | 起始时间 |
| end_time | string | 否 | 结束时间 |

### feishu_im_user_message

发送消息。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | send/reply |
| receive_id_type | string | 是 | open_id/chat_id |
| receive_id | string | 是 | 接收者 ID |
| msg_type | string | 是 | 消息类型 |
| content | string | 是 | 消息内容（JSON 字符串）|
| message_id | string | 否 | 被回复消息 ID |

**msg_type 和 content 格式**：

```json
// text
{"text": "消息内容"}

// image
{"image_key": "img_xxx"}

// file
{"file_key": "file_xxx"}

// post (富文本)
{"zh_cn": {"title": "标题", "content": [[{"tag": "text", "text": "正文"}]]}}
```

### feishu_im_user_search_messages

跨会话搜索消息。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| query | string | 否 | 搜索关键词 |
| chat_id | string | 否 | 限定会话 |
| sender_ids | array | 否 | 发送者 ID 列表 |
| mention_ids | array | 否 | 被@用户 ID |
| message_type | string | 否 | 消息类型 |
| relative_time | string | 否 | 相对时间 |

---

## 👤 用户工具

### feishu_get_user

获取用户信息。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| user_id | string | 否 | 用户 ID（不填则获取自己）|

### feishu_search_user

搜索员工。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| query | string | 是 | 搜索关键词（姓名/手机/邮箱）|

---

## 👥 群聊工具

### feishu_chat

管理群聊。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| action | string | 是 | search/get |
| query | string | 否 | 搜索关键词 |
| chat_id | string | 否 | 群 ID |

### feishu_chat_members

获取群成员列表。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| chat_id | string | 是 | 群 ID |

---

## ⚠️ 通用说明

1. **OAuth 授权**：大部分工具需要用户 OAuth 授权
2. **授权流程**：首次使用工具时自动弹出授权卡片
3. **权限要求**：部分操作需要在飞书开放平台开通相应权限
4. **时间格式**：使用 ISO 8601 格式，包含时区
5. **JSON 字符串**：消息 content 等字段必须是合法 JSON 字符串

---

*本文档由 awesome-claw-study 整理*
