# 记忆系统

> OpenClaw 的记忆机制

## 核心概念

OpenClaw 记忆是**工作空间中的普通 Markdown 文件**。文件是唯一真相来源；模型只"记住"写入磁盘的内容。

## 记忆文件结构

默认工作空间使用两层记忆：

### 1. 每日日志

- 路径: `memory/YYYY-MM-DD.md`
- 用途: 每日记录（只追加）
- 读取: 每次会话开始时读取今天和昨天的日志

### 2. 长期记忆

- 路径: `MEMORY.md`（可选）
- 用途: 精选的长期记忆
- ⚠️ **只在主、私人会话中加载**（群聊中不加载）

## 记忆工具

OpenClaw 提供两个记忆工具：

| 工具 | 用途 |
|------|------|
| `memory_search` | 语义搜索记忆片段 |
| `memory_get` | 读取指定 Markdown 文件/行范围 |

## 何时写入记忆

- **决策、偏好和持久事实** → 写入 `MEMORY.md`
- **日常笔记和运行上下文** → 写入 `memory/YYYY-MM-DD.md`
- 如果有人说"记住这个"，**写下来**（不要只记在内存里）

## 自动记忆刷新

当会话**接近自动压缩**时，OpenClaw 触发一个**静默的智能轮次**，提醒模型在上下文压缩**之前**写入持久记忆。

配置示例：

```json5
{
  agents: {
    defaults: {
      compaction: {
        reserveTokensFloor: 20000,
        memoryFlush: {
          enabled: true,
          softThresholdTokens: 4000,
          systemPrompt: "Session nearing compaction. Store durable memories now.",
          prompt: "Write any lasting notes to memory/YYYY-MM-DD.md; reply with NO_REPLY if nothing to store.",
        },
      },
    },
  },
}
```

## 向量记忆搜索

OpenClaw 可以对 `MEMORY.md` 和 `memory/*.md` 构建小型向量索引，支持语义查询。

默认配置：
- 默认启用
- 监视记忆文件变化（防抖）
- 使用远程嵌入模型
- 支持本地模式（需要 node-llama-cpp）

支持的嵌入提供商：
- OpenAI
- Gemini
- Voyage
- Mistral
- Ollama（自托管）
