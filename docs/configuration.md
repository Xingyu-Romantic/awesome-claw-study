# 配置说明

> OpenClaw 配置文件详解

## 配置文件位置

`~/.openclaw/openclaw.json`（JSON5 格式，支持注释）

## 最小配置

```json5
{
  agents: { defaults: { workspace: "~/.openclaw/workspace" } },
  channels: { whatsapp: { allowFrom: ["+15555550123"] } },
}
```

## 编辑配置的方式

### 1. 交互式向导

```bash
openclaw onboard       # 完整设置向导
openclaw configure     # 配置向导
```

### 2. CLI 命令

```bash
openclaw config get agents.defaults.workspace
openclaw config set agents.defaults.heartbeat.every "2h"
openclaw config unset tools.web.search.apiKey
```

### 3. 控制台 UI

打开 http://127.0.0.1:18789 的 **Config** 标签页

### 4. 直接编辑

编辑 `~/.openclaw/openclaw.json`，网关会自动热重载。

## 严格验证

⚠️ OpenClaw 只接受完全符合 schema 的配置。未知键、类型错误或无效值会导致网关**拒绝启动**。

验证失败时：
- 网关不会启动
- 只允许诊断命令（`doctor`, `logs`, `health`, `status`）
- 运行 `openclaw doctor` 查看具体问题
- 运行 `openclaw doctor --fix` 自动修复

## 常用配置示例

### 设置模型

```json5
{
  agents: {
    defaults: {
      model: {
        primary: "anthropic/claude-sonnet-4-5",
        fallbacks: ["openai/gpt-5.2"],
      },
    },
  },
}
```

### 配置渠道

```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123:abc",
      dmPolicy: "pairing",   // pairing | allowlist | open | disabled
      allowFrom: ["tg:123"],
    },
  },
}
```

### 配置心跳

```json5
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last",
        activeHours: { start: "08:00", end: "22:00" },
      },
    },
  },
}
```

## 完整配置参考

更多配置项请查看 [官方配置参考](https://docs.openclaw.ai/gateway/configuration-reference)
