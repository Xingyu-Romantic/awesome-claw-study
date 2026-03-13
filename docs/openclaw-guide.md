# OpenClaw 完整指南

> OpenClaw 官方资料汇总 | 引用自官方文档、GitHub、VISION 等

---

## 什么是 OpenClaw？

OpenClaw 是一个运行在你自己设备上的个人 AI 助手。

> **来源**: [GitHub README](https://github.com/openclaw/openclaw)
> 
> "OpenClaw is a personal AI assistant you run on your own devices. It answers you on the channels you already use (WhatsApp, Telegram, Slack, Discord, Google Chat, Signal, iMessage, BlueBubbles, IRC, Microsoft Teams, Matrix, Feishu, LINE, Mattermost, Nextcloud Talk, Nostr, Synology Chat, Tlon, Twitch, Zalo, Zalo Personal, WebChat)."

**核心口号**: 
> "EXFOLIATE! EXFOLIATE!" — A space lobster, probably

**官网**: https://openclaw.ai

---

## 发展历程

OpenClaw 始于一个个人学习 AI 并构建有用工具的游乐场：
- Warelay → Clawdbot → Moltbot → OpenClaw

> **来源**: [VISION.md](https://github.com/openclaw/openclaw/blob/main/VISION.md)
>
> "OpenClaw started as a personal playground to learn AI and build something genuinely useful: an assistant that can run real tasks on a real computer."

**目标**: 打造一个易于使用、支持广泛平台、尊重隐私和安全的个人助手。

---

## 核心特性

### 支持的渠道

| 渠道 | 说明 |
|------|------|
| WhatsApp | 通过 Baileys |
| Telegram | 通过 grammY |
| Discord | 通过 discord.js |
| Slack | 通过 Bolt |
| Google Chat | 通过 Chat API |
| Signal | 通过 signal-cli |
| iMessage | 通过 BlueBubbles（推荐）或 imsg |
| IRC | 支持 |
| Microsoft Teams | 支持 |
| Matrix | 支持 |
| 飞书 (Feishu) | 支持 |
| LINE | 支持 |
| Mattermost | 插件支持 |
| Nextcloud Talk | 支持 |
| Nostr | 支持 |
| Synology Chat | 支持 |
| Tlon | 支持 |
| Twitch | 支持 |
| Zalo | 支持 |
| WebChat | 支持 |

> **来源**: [GitHub README](https://github.com/openclaw/openclaw)

### 核心功能

| 功能 | 描述 |
|------|------|
| **多渠道收件箱** | 单个 Gateway 连接多个消息平台 |
| **多 Agent 路由** | 隔离会话，支持按工作区/发送者路由 |
| **语音模式** | macOS/iOS 语音唤醒 + Android 连续语音 |
| **Live Canvas** | Agent 驱动的可视化工作空间 |
| **第一方工具** | 浏览器、Canvas、节点、Cron、会话等 |
| **伴侣应用** | macOS 菜单栏应用 + iOS/Android 节点 |

> **来源**: [Features](https://docs.openclaw.ai/concepts/features)

---

## 快速开始

### 安装

```bash
# Node ≥ 22
npm install -g openclaw@latest
# 或
pnpm add -g openclaw@latest
```

### 初始化

```bash
openclaw onboard --install-daemon
```

向导会逐步引导设置 Gateway、工作区、渠道和技能。

### 运行

```bash
# 启动 Gateway
openclaw gateway --port 18789 --verbose

# 发送消息
openclaw message send --to +1234567890 --message "Hello from OpenClaw"

# 与助手对话
openclaw agent --message "Ship checklist" --thinking high
```

> **来源**: [GitHub README](https://github.com/openclaw/openclaw)

---

## 技术架构

### 运行时要求

- **Node.js**: ≥ 22（推荐）
- **支持平台**: macOS, Linux, Windows (WSL2 推荐)

### 架构组件

| 组件 | 说明 |
|------|------|
| **Gateway WS 控制平面** | 会话、存在、配置、Cron、Webhook、控制台、Canvas 主机 |
| **CLI 表面** | gateway, agent, send, wizard, doctor |
| **Pi Agent 运行时** | RPC 模式，工具流和块流 |
| **会话模型** | main 用于直接聊天，组隔离，激活模式，队列模式，回回复 |
| **媒体管道** | 图片/音频/视频，转录钩子，大小限制 |

> **来源**: [GitHub README](https://github.com/openclaw/openclaw)

---

## 安全模型

### 核心原则

OpenClaw 安全模型基于**个人助手**假设：
- 一个信任边界（一个用户/ Gateway）
- 不是多租户安全边界

> **来源**: [Security](https://docs.openclaw.ai/gateway/security)
>
> "OpenClaw security guidance assumes a **personal assistant** deployment: one trusted operator boundary, potentially many agents."

### 快速安全检查

```bash
openclaw security audit
openclaw security audit --deep
openclaw security audit --fix
openclaw security audit --json
```

### 安全建议

1. **从最小权限开始** - 先用最小访问权限，然后根据需要扩大
2. **DM 配对** - 未知发送者需要配对码才能使用机器人
3. **不要共享 Gateway** - 为每个用户使用独立的 Gateway
4. **敏感操作需要确认** - 发送、修改、写入等操作"先预览，再确认"

> **来源**: [Security](https://docs.openclaw.ai/gateway/security)

---

## 技能系统 (Skills)

### 什么是 Skills？

Skills 扩展 OpenClaw 的能力，让它可以与外部服务交互、自动化工作流、执行专业任务。

### 安装技能

```bash
clawhub install <skill-slug>
```

### 技能市场

- **ClawHub**: https://clawhub.ai
- **官方技能列表**: 5400+ 社区技能

### 重要技能

| 技能 | 描述 |
|------|------|
| elite-longterm-memory | 长期记忆增强 |
| self-improving-agent | 深度进化 |
| github-repo-search | GitHub 寻宝 |
| feishu-bitable | 飞书多维表格 |
| feishu-calendar | 飞书日历 |

> **来源**: [awesome-openclaw-skills](https://github.com/VoltAgent/awesome-openclaw-skills)

---

## MCP 集成

OpenClaw 通过 **mcporter** 支持 MCP（Model Context Protocol）。

> **来源**: [VISION.md](https://github.com/openclaw/openclaw/blob/main/VISION.md)
>
> "OpenClaw supports MCP through mcporter: https://github.com/steipete/mcporter"

**MCP 集成优势**：
- 添加或更改 MCP 服务器无需重启 Gateway
- 保持核心工具/上下文表面精简
- 减少 MCP 变化对核心稳定性和安全性的影响

---

## 版本与更新

### 发布渠道

| 渠道 | 说明 |
|------|------|
| stable | 正式发布版 (vYYYY.M.D) |
| beta | 预发布版 (vYYYY.M.D-beta.N) |
| dev | 开发版 (main 分支) |

### 切换版本

```bash
openclaw update --channel stable|beta|dev
```

> **来源**: [GitHub README](https://github.com/openclaw/openclaw)

---

## 路线图与优先级

### 当前优先级

1. **安全和安全默认值**
2. **Bug 修复和稳定性**
3. **设置可靠性和首次运行体验**

### 下一阶段优先级

- 支持所有主要模型提供商
- 改进主要消息渠道支持（并添加一些高需求渠道）
- 性能和测试基础设施
- 更好的计算机使用和 Agent 工具能力
- CLI 和 Web 前端的人体工程学
- macOS、iOS、Android、Windows、Linux 上的伴侣应用

### 不会做的事情

- 添加非核心技能到核心（应发布到 ClawHub）
- 完整文档翻译集（计划 AI 生成翻译）
- 不明确适合模型提供商类别的商业服务集成
- 围绕已支持渠道的无明显能力或安全差距的包装渠道
- 在核心中构建第一类 MCP 运行时（mcporter 已提供集成路径）
- Agent 层级框架（管理器-of-管理器/嵌套规划器树）作为默认架构

> **来源**: [VISION.md](https://github.com/openclaw/openclaw/blob/main/VISION.md)

---

## 贡献指南

### 贡献规则

- 一个 PR = 一个问题/主题
- PR 超过 ~5000 行变更只在例外情况下审核
- 不要一次打开大量小型 PR
- 小型相关修复可以合并到一个 PR

### 贡献方式

1. ⭐ Star 本项目
2. 📝 完善文档
3. 🐛 提交问题
4. 💡 提出新功能
5. 📤 提交 PR

> **来源**: [VISION.md](https://github.com/openclaw/openclaw/blob/main/VISION.md)

---

## 社区资源

| 资源 | 链接 |
|------|------|
| 官网 | https://openclaw.ai |
| 文档 | https://docs.openclaw.ai |
| GitHub | https://github.com/openclaw/openclaw |
| Discord | https://discord.gg/clawd |
| ClawHub | https://clawhub.ai |

---

## 许可证

MIT License - 详见 [LICENSE](./LICENSE)

---

*本文档由 awesome-claw-study 整理，引用自 OpenClaw 官方资料*
