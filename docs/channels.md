# 飞书渠道配置

> 连接 OpenClaw 到飞书/ Lark

## 简介

飞书（Lark）是企业协作平台。本插件通过 WebSocket 事件订阅连接 OpenClaw 到飞书机器人，无需暴露公网 Webhook URL。

## 快速开始

### 方法 1: 向导（推荐）

```bash
openclaw onboard
```

向导会引导你：
1. 创建飞书应用并收集凭证
2. 在 OpenClaw 中配置应用凭证
3. 启动网关

### 方法 2: CLI 添加

```bash
openclaw channels add
```

选择 **Feishu**，然后输入 App ID 和 App Secret。

---

## 步骤 1: 创建飞书应用

### 1. 打开飞书开放平台

访问 [飞书开放平台](https://open.feishu.cn/app)

Lark（全球版）租户使用 https://open.larksuite.com/app 并在配置中设置 `domain: "lark"`

### 2. 创建应用

1. 点击 **创建企业应用**
2. 填写应用名称和描述
3. 选择应用图标

### 3. 复制凭证

从 **凭证与基础信息** 复制：
- **App ID**（格式: `cli_xxx`）
- **App Secret**

⚠️ **重要**: 保护好 App Secret

### 4. 配置权限

在 **权限** 点击 **批量导入**，粘贴：

```json
{
  "scopes": {
    "tenant": [
      "aily:file:read",
      "aily:file:write",
      "application:application.app_message_stats.overview:readonly",
      "application:application:self_manage",
      "application:bot.menu:write",
      "cardkit:card:read",
      "cardkit:card:write",
      "contact:user.employee_id:readonly",
      "corehr:file:download",
      "event:ip_list",
      "im:chat.access_event.bot_p2p_chat:read",
      "im:chat.members:bot_access",
      "im:message",
      "im:message.group_at_msg:readonly",
      "im:message.p2p_msg:readonly",
      "im:message:readonly",
      "im:message:send_as_bot",
      "im:resource"
    ],
    "user": ["aily:file:read", "aily:file:write", "im:chat.access_event.bot_p2p_chat:read"]
  }
}
```

### 5. 启用机器人能力

在 **应用能力** > **机器人**：
1. 启用机器人能力
2. 设置机器人名称

### 6. 配置事件订阅

⚠️ **重要**: 配置事件订阅前确保：
1. 已运行 `openclaw channels add` 添加飞书
2. 网关正在运行

在 **事件订阅**：
1. 选择 **使用长连接接收事件**（WebSocket）
2. 添加事件: `im.message.receive_v1`

⚠️ 如果网关未运行，长连接设置可能无法保存。

### 7. 发布应用

1. 在 **版本管理与发布** 创建版本
2. 提交审核并发布

---

## 步骤 2: 配置 OpenClaw

在 `~/.openclaw/openclaw.json` 中添加：

```json5
{
  channels: {
    feishu: {
      appId: "cli_xxx",
      appSecret: "你的AppSecret",
    },
  },
}
```

---

## 验证

```bash
openclaw gateway status
openclaw logs --follow
```

---

## 飞书支持的功能

| 功能 | 支持 |
|------|------|
| 私聊消息 | ✅ |
| 群聊消息 | ✅ |
| @机器人 | ✅ |
| 消息卡片 | ✅ |
| 文件/图片 | ✅ |
| 表情反应 | ✅ |
