# 浏览器控制

> OpenClaw 管理的浏览器自动化

## 概述

OpenClaw 可以运行一个**专用的 Chrome/Brave/Edge/Chromium 配置**，由 Agent 控制。它与您的个人浏览器隔离，通过 Gateway 内的小型本地控制服务管理（仅限回环）。

**简单理解**：
- 这是一个**独立的、仅 Agent 使用的浏览器**
- `openclaw` 配置**不会**影响您的个人浏览器
- Agent 可以**打开标签页、读取页面、点击和输入**

## 快速开始

```bash
# 检查浏览器状态
openclaw browser --browser-profile openclaw status

# 启动浏览器
openclaw browser --browser-profile openclaw start

# 打开网页
openclaw browser --browser-profile openclaw open https://example.com

# 截图
openclaw browser --browser-profile openclaw snapshot
```

如果显示 "Browser disabled"，需要在配置中启用并重启 Gateway。

## 配置模式：`openclaw` vs `chrome`

| 模式 | 说明 |
|------|------|
| `openclaw` | 托管的隔离浏览器（无需扩展）|
| `chrome` | 扩展中继到您的**系统浏览器**（需要 OpenClaw 扩展）|

设置默认模式：
```json
{
  "browser": {
    "defaultProfile": "openclaw"
  }
}
```

## 配置文件

浏览器设置位于 `~/.openclaw/openclaw.json`：

```json5
{
  browser: {
    enabled: true,           // 默认: true
    defaultProfile: "openclaw",
    color: "#FF4500",        // 浏览器主题色
    headless: false,
    noSandbox: false,
    attachOnly: false,
    // 使用 Brave 浏览器
    executablePath: "/Applications/Brave Browser.app/Contents/MacOS/Brave Browser",
    profiles: {
      openclaw: { cdpPort: 18800, color: "#FF4500" },
      work: { cdpPort: 18801, color: "#0066CC" },
      remote: { cdpUrl: "http://10.0.0.42:9222", color: "#00AA00" },
    },
  },
}
```

### 配置说明

| 配置项 | 说明 |
|--------|------|
| `enabled` | 是否启用浏览器 |
| `defaultProfile` | 默认使用的配置 |
| `headless` | 无头模式（不显示浏览器窗口）|
| `noSandbox` | 禁用沙盒（Linux root 用户）|
| `attachOnly` | 仅附加，不自动启动浏览器 |
| `executablePath` | 指定浏览器路径 |
| `profiles` | 多配置支持 |

## 使用 Brave 或其他浏览器

如果您的**系统默认**浏览器是基于 Chromium 的（Chrome/Brave/Edge 等），OpenClaw 会自动使用它。

手动指定：

```bash
# CLI
openclaw config set browser.executablePath "/usr/bin/google-chrome"
```

```json5
// macOS
{
  "browser": {
    "executablePath": "/Applications/Brave Browser.app/Contents/MacOS/Brave Browser"
  }
}

// Windows
{
  "browser": {
    "executablePath": "C:\\Program Files\\BraveSoftware\\Brave-Browser\\Application\\brave.exe"
  }
}

// Linux
{
  "browser": {
    "executablePath": "/usr/bin/brave-browser"
  }
}
```

## 浏览器工具

Agent 可以使用的浏览器操作：

| 操作 | 说明 |
|------|------|
| `open` | 打开 URL |
| `snapshot` | 获取页面快照 |
| `screenshot` | 截图 |
| `click` | 点击元素 |
| `type` | 输入文本 |
| `drag` | 拖拽 |
| `select` | 选择 |
| `fill` | 填充表单 |
| `evaluate` | 执行 JavaScript |
| `close` | 关闭标签页 |

## 安全配置

### SSRF 防护

```json5
{
  "browser": {
    "ssrfPolicy": {
      "dangerouslyAllowPrivateNetwork": true,  // 默认: true（信任网络模式）
      "hostnameAllowlist": ["*.example.com", "example.com"],
      "allowedHostnames": ["localhost"]
    }
  }
}
```

- `dangerouslyAllowPrivateNetwork: true` - 允许访问私有网络
- `dangerouslyAllowPrivateNetwork: false` - 严格限制仅访问公共网络

## 多配置支持

可以创建多个浏览器配置：

```json
{
  "browser": {
    "profiles": {
      "openclaw": { "cdpPort": 18800, "color": "#FF4500" },
      "work": { "cdpPort": 18801, "color": "#0066CC" },
      "personal": { "cdpPort": 18802, "color": "#00AA00" }
    }
  }
}
```

使用指定配置：
```bash
openclaw browser --browser-profile work open https://example.com
```

## 故障排查

### 浏览器无法启动

1. 检查浏览器是否已安装
2. 确认配置中 `enabled: true`
3. 尝试设置 `executablePath`

### 无法连接

1. 检查端口是否被占用
2. 确认防火墙允许本地连接
3. 查看日志：`openclaw logs`

### 扩展模式问题

如果使用 `chrome` 模式：
1. 确认已安装 OpenClaw 扩展
2. 在扩展中点击 "Attach" 按钮
3. 确认标签页已连接（徽章显示 ON）

---

*本文档由 awesome-claw-study 整理*
