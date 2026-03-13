# 快速入门指南

> 从零开始，5 分钟快速上手 OpenClaw

## 前置要求

- **Node.js**: 推荐 Node 24（Node 22 LTS 也支持）
- **API Key**: 你选择的模型提供商的 API Key

检查 Node 版本：
```bash
node --version
```

## 快速安装

### macOS / Linux

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

### Windows (PowerShell)

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

## 初始化配置

运行引导向导：

```bash
openclaw onboard --install-daemon
```

向导会帮你配置：
- 认证信息
- 网关设置
- 可选渠道

## 启动网关

检查网关状态：

```bash
openclaw gateway status
```

前台运行（用于测试/排查）：

```bash
openclaw gateway --port 18789
```

## 打开控制台

```bash
openclaw dashboard
```

然后打开浏览器访问 http://127.0.0.1:18789/

## 发送测试消息

需要先配置渠道：

```bash
openclaw message send --target +15555550123 --message "Hello from OpenClaw"
```

## 环境变量

常用环境变量：

| 变量 | 作用 |
|------|------|
| `OPENCLAW_HOME` | 设置主目录 |
| `OPENCLAW_STATE_DIR` | 覆盖状态目录 |
| `OPENCLAW_CONFIG_PATH` | 覆盖配置文件路径 |

## 下一步

- [配置说明](./configuration.md) - 配置文件详解
- [消息通道](./channels.md) - 连接更多渠道
- [技能系统](./skills.md) - 安装和使用技能
