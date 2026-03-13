# 技能系统 (Skills)

> OpenClaw 技能系统完全指南

## 什么是 Skills？

OpenClaw 使用 **AgentSkills 兼容**的技能文件夹来教 Agent 如何使用工具。每个技能是一个目录，包含一个 `SKILL.md` 文件（YAML 前置内容和指令）。

## 技能位置与优先级

技能从 **三个** 位置加载：

| 位置 | 说明 |
|------|------|
| **Bundled skills** | 随安装包一起提供 |
| **Managed/local skills** | `~/.openclaw/skills` |
| **Workspace skills** | `<workspace>/skills` |

**优先级**（高到低）：
```
<workspace>/skills > ~/.openclaw/skills > bundled skills
```

## 多 Agent 技能管理

在 **多 Agent** 设置中，每个 Agent 有自己的 workspace：

- **私有技能**：位于 `<workspace>/skills`，仅该 Agent 可用
- **共享技能**：位于 `~/.openclaw/skills`，所有 Agent 可见

## ClawHub 技能市场

ClawHub 是 OpenClaw 的公共技能注册表。

**官网**：https://clawhub.com

### 常用命令

```bash
# 安装技能到 workspace
clawhub install <skill-slug>

# 更新所有已安装技能
clawhub update --all

# 同步（扫描并发布更新）
clawhub sync --all
```

## 安全注意事项

⚠️ **重要**：
- 将第三方技能视为**不受信任的代码**
- 启用前务必阅读技能代码
- 对于不受信任的输入和风险工具，优先使用沙盒运行
- 详见 [Security](/gateway/security)

## 技能格式

`SKILL.md` 必须包含：

```markdown
---
name: nano-banana-pro
description: Generate or edit images via Gemini 3 Pro Image
---
```

### 可选前置字段

| 字段 | 说明 |
|------|------|
| `homepage` | 技能官网 URL |
| `user-invocable` | 是否作为用户斜杠命令暴露（默认 true）|
| `disable-model-invocation` | 是否从模型提示中排除（默认 false）|
| `command-dispatch` | 设为 `tool` 时，斜杠命令直接调度到工具 |
| `command-tool` | 当 command-dispatch 为 tool 时，要调用的工具名 |

## 技能 gating（加载时过滤）

OpenClaw 使用 `metadata` 在加载时过滤技能：

```markdown
---
name: nano-banana-pro
description: Generate or edit images via Gemini 3 Pro Image
metadata:
  {
    "openclaw":
      {
        "requires": { "bins": ["uv"], "env": ["GEMINI_API_KEY"], "config": ["browser.enabled"] },
        "primaryEnv": "GEMINI_API_KEY",
      },
  }
---
```

### metadata 字段

| 字段 | 说明 |
|------|------|
| `requires.bins` | 需要存在的可执行文件 |
| `requires.env` | 需要设置的环境变量 |
| `requires.config` | 需要存在的配置项 |
| `primaryEnv` | 主要环境变量（用于显示）|

## 常用技能推荐

### 必备技能

```bash
clawhub install elite-longterm-memory    # 长期记忆增强
clawhub install self-improving-agent     # 深度进化
clawhub install github-repo-search       # GitHub 寻宝
clawhub install skill-vetter             # 技能安全审查
```

### 飞书相关

```bash
# 飞书技能已内置
feishu-bitable    # 多维表格
feishu-calendar   # 日历
feishu-task       # 任务
```

### 其他实用技能

| 技能 | 描述 |
|------|------|
| weather | 天气查询 |
| video-frames | 视频帧提取 |
| coding-agent | 代码开发代理 |
| healthcheck | 健康检查 |

## 创建自定义技能

技能目录结构：

```
my-skill/
├── SKILL.md        # 必需
└── scripts/        # 可选
    └── ...
```

### SKILL.md 示例

```markdown
---
name: my-custom-skill
description: 我的自定义技能
metadata: {"openclaw": {"requires": {"env": ["MY_API_KEY"]}}}
---

# 使用说明

这个技能用于...

## 工具

本技能提供以下工具：

### tool1

描述：做某事

参数：
- param1: 参数1说明
- param2: 参数2说明
```

## 故障排查

### 技能不生效

1. 检查技能位置是否正确
2. 确认 SKILL.md 格式正确
3. 检查 metadata gating 条件是否满足

### 技能冲突

如果同名技能存在于多个位置，优先级高的会覆盖低的。

---

*本文档由 awesome-claw-study 整理*
