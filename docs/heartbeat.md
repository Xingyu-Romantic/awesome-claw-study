# 心跳系统 vs Cron 定时任务

> 何时使用哪种定时机制

## 快速选择指南

| 使用场景 | 推荐方案 | 原因 |
|----------|----------|------|
| 每 30 分钟检查收件箱 | 心跳 | 批量检查，有上下文 |
| 每天 9:00 发送报告 | Cron | 精确时间 |
| 监控日历 upcoming 事件 | 心跳 | 适合周期性感知 |
| 运行每周深度分析 | Cron | 独立任务，可用不同模型 |
| 20 分钟后提醒我 | Cron | 一次性精确定时 |
| 后台项目健康检查 | 心跳 | 搭便车现有周期 |

---

## 心跳 (Heartbeat)

心跳在**主会话**中以固定间隔运行（默认 30 分钟）。设计用于让 Agent 检查情况并上报重要内容。

### 适用场景

- **多个周期性检查**: 代替 5 个独立的 cron 任务检查邮箱、日历、天气、通知、项目状态，一个心跳可以批量处理所有这些
- **上下文感知决策**: Agent 有完整的主会话上下文，可以智能判断什么是紧急的
- **对话连续性**: 心跳共享同一会话，所以 Agent 记得最近的对话
- **低开销监控**: 一个心跳代替许多小的轮询任务

### 优势

- ✅ 批量多个检查
- ✅ 减少 API 调用
- ✅ 上下文感知
- ✅ 智能抑制（无需关注时回复 `HEARTBEAT_OK`）
- ✅ 自然时间（根据队列负载轻微漂移）

### 配置示例

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

### HEARTBEAT.md 示例

```markdown
# Heartbeat checklist

- Check email for urgent messages
- Review calendar for events in next 2 hours
- If a background task finished, summarize results
- If idle for 8+ hours, send a brief check-in
```

---

## Cron 定时任务

Cron 在精确时间运行，可以在隔离会话中运行而不影响主上下文。

### 适用场景

- **精确时间**: "每周一 9:00 发送"（不是"大约 9 点"）
- **独立任务**: 不需要对话上下文的任务
- **不同模型/思考**: 需要更强大模型的重分析
- **一次性提醒**: `--at` 指定精确未来时间戳
- **嘈杂/频繁任务**: 会弄乱主会话历史的任务
- **外部触发**: 应该在 Agent 是否活跃的情况下独立运行的任务

### 优势

- ✅ 精确时间（5 字段或 6 字段 cron 表达式 + 时区）
- ✅ 内置负载分散（整点任务自动分散到 0-5 分钟窗口）
- ✅ 隔离会话（不污染主历史）
- ✅ 模型覆盖（每个任务可用不同模型）
- ✅ 交付控制（announce 模式直接发布）
- ✅ 无需 Agent 上下文（即使主会话空闲也运行）

### 配置示例

```bash
openclaw cron add \
  --name "Morning briefing" \
  --cron "0 7 * * *" \
  --tz "America/New_York" \
  --session isolated \
  --message "Generate today's briefing: weather, calendar, top emails, news summary." \
  --model opus \
  --announce
```

### 查看任务

```bash
openclaw cron list
openclaw cron run "任务名"  # 手动执行
```
