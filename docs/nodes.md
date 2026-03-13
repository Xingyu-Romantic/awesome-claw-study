# 节点 (Nodes)

> OpenClaw 伴侣设备连接指南

## 什么是 Nodes？

**节点（Node）** 是连接到 Gateway 的伴侣设备（macOS/iOS/Android/无头设备），通过 WebSocket（与操作员相同端口）连接，并暴露命令表面（如 `canvas.*`、`camera.*`、`device.*`、`notifications.*`、`system.*`）。

**注意**：
- 节点是**外设**，不是网关
- Telegram/WhatsApp 等消息到达**网关**，不到节点
- 节点不能运行网关服务

## 支持的平台

| 平台 | 功能 |
|------|------|
| **macOS** | 菜单栏应用、Canvas、相机、屏幕录制、语音 |
| **iOS** | 配对、Canvas、相机、屏幕录制、位置、语音 |
| **Android** | 配对、Connect tab、聊天、语音、Canvas/相机、设备控制、通知、联系人/日历、运动、照片、SMS |
| **Linux (无头)** | 远程执行命令 |

## 配对与状态

### 快速命令

```bash
# 列出设备
openclaw devices list

# 批准配对请求
openclaw devices approve <requestId>

# 拒绝配对请求
openclaw devices reject <requestId>

# 查看节点状态
openclaw nodes status

# 查看节点详情
openclaw nodes describe --node <idOrNameOrIp>
```

## 远程节点主机 (Remote Node Host)

当 Gateway 在一台机器上运行，但希望命令在另一台机器上执行时，使用**节点主机**。

### 架构说明

| 组件 | 角色 |
|------|------|
| **Gateway 主机** | 接收消息、运行模型、路由工具调用 |
| **节点主机** | 在节点机器上执行 `system.run`/`system.which` |
| **审批** | 通过节点主机上的 `~/.openclaw/exec-approvals.json` 执行 |

### 启动节点主机（前台）

在节点机器上：

```bash
openclaw node run --host <gateway-host> --port 18789 --display-name "Build Node"
```

### 通过 SSH 隧道连接

如果 Gateway 绑定到回环（本地模式），远程节点无法直接连接：

```bash
# 终端 A（保持运行）：转发本地 18790 -> 网关 127.0.0.1:18789
ssh -N -L 18790:127.0.0.1:18789 user@gateway-host

# 终端 B：导出网关 token 并通过隧道连接
export OPENCLAW_GATEWAY_TOKEN="<gateway-token>"
openclaw node run --host 127.0.0.1 --port 18790 --display-name "Build Node"
```

### 启动节点主机（服务）

```bash
openclaw node install --host <gateway-host> --port 18789 --display-name "Build Node"
openclaw node restart
```

### 配对与命名

在网关主机上：

```bash
openclaw devices list
openclaw devices approve <requestId>
openclaw nodes status
```

**命名选项**：
- `--display-name` 在 `openclaw node run/install` 时设置
- `openclaw nodes rename --node <id|name|ip> --name "Build Node"`（网关覆盖）

### 命令白名单

Exec 审批是**按节点主机**的。从网关添加白名单条目：

```bash
openclaw approvals allowlist add --node <id|name|ip> "/usr/bin/uname"
```

## macOS 节点模式

macOS 菜单栏应用可以连接到 Gateway 的 WS 服务器，并将其本地 canvas/camera 命令作为节点暴露（所以 `openclaw nodes …` 可以作用于这台 Mac）。

## 节点功能

### Canvas

- 渲染可视化界面
- A2UI 支持

### 相机

- 拍照
- 录像
- 截取屏幕

### 设备控制

- 获取设备信息
- 位置服务
- 通知管理
- 联系人/日历
- 短信

### 语音

- 语音输入
- TTS 输出

## 故障排查

### 节点无法连接

1. 确认 Gateway 正在运行
2. 检查网络连接
3. 验证 token 正确
4. 查看日志：`openclaw logs`

### 配对请求未收到

1. 运行 `openclaw devices list` 查看待处理请求
2. 确认节点显示的配对码正确

### 命令无法执行

1. 检查审批白名单
2. 确认节点主机已启动
3. 验证命令路径正确

---

*本文档由 awesome-claw-study 整理*
