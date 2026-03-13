# OpenClaw 飞书官方插件使用指南

> 飞书官方插件，让你的飞书长出龙虾钳 🦞

## 简介

在获得你的授权后，OpenClaw 可以直接以**你的身份**看文档找资料、核对日历看档期、理解群聊上下文。

**你说一句话，它就能伸出"钳子"，直接在飞书里帮你把活儿干了。**

## 支持的飞书能力

| 业务类别 | 支持的能力 |
|----------|------------|
| 💬 消息 | 消息读取（群聊/单聊历史、话题回复）、消息发送（含发卡片）、消息回复、消息搜索、图片/文件下载 |
| 📄 文档 | 创建云文档、更新云文档、读取云文档内容 |
| 📊 多维表格 | 创建/管理多维表格、数据表、字段、记录（增删改查、批量操作、高级筛选）、视图 |
| 📊 电子表格 | 创建、编辑、查看电子表格 |
| 📅 日历日程 | 日历管理、日程管理（创建/查询/修改/删除/搜索）、参会人管理、忙闲查询 |
| ✅ 任务 | 任务管理（创建/查询/更新/完成）、清单管理、子任务、评论 |

⚠️ **注意**：以用户身份发消息需在飞书开放平台额外开通机器人 `im:message.send_as_bot` 权限，部分企业（如字节）不支持此操作。

---

## ⚠️ 重要安全与风险提示

### 核心风险

该插件通过飞书接口连接了你的工作数据——消息、文档、日历、联系人，AI 能读到的东西理论上就有泄露的可能。

**强烈建议**：
- 作为机器人供多人使用或者通过公司飞书账号使用可能会导致数据安全和隐私风险
- 请注意使用时需要遵守企业内的数据安全和隐私要求
- 避免发生数据泄露、权限突破、侵犯隐私等后果

### 其他操作风险

- **AI 并不完美，可能存在"幻觉"**：它有时会误解您的意图，或者生成看似合理但不准确的内容
- **部分操作不可逆转**：例如，AI 代发的飞书消息是以您的名义发出的，发出后即成事实
- **应对建议**：对于涉及发送、修改、写入等重要操作，**请务必做到"先预览，再确认"**

### 💡 使用建议

**先拿个人账号安全地"玩"起来**，等后续安全隔离能力更成熟了，再考虑接入真实工作环境。

---

## 从 0 安装飞书插件

### 步骤一：安装并配置 OpenClaw

1. 根据官方指南，执行以下安装命令：
   - **Linux/MacOS**：`curl -fsSL https://openclaw.ai/install.sh | bash`
   - **Windows**：`iwr -useb https://openclaw.ai/install.ps1 | iex`

2. 根据提示信息完成 OpenClaw 配置

3. 安装成功后，打开 OpenClaw Dashboard URL，正常显示管理后台则代表安装成功

4. 在管理后台的聊天页面进行对话验证

### 步骤二：安装飞书官方插件

1. 在命令行终端中，执行以下安装指令：
```bash
npx -y @larksuite/openclaw-lark-tools install
```

2. 执行过程中，可选择 **新建机器人** 或 **关联已有机器人**

3. 若选择新建机器人，可通过飞书客户端扫描二维码，选择 **一键创建飞书机器人**

4. 创建完成后，点击 **打开机器人**，在飞书中向机器人发送任意消息，即可开始对话

5. 快速授权：在飞书对话中发送 `/feishu auth` 来完成批量授权

6. 让 AI 学习新技能：在飞书对话中发送 `学习一下我安装的新飞书插件，列出有哪些能力`

7. 验证安装成功：在飞书对话中发送 `/feishu start`，若返回版本号则代表安装成功

---

## 升级飞书插件版本

1. 运行 `openclaw -v` 查看已安装的 OpenClaw 版本
   - **Linux/MacOS**：需要 **2026.2.26** 及以上
   - **Windows**：需要 **2026.3.2** 及以上

2. 升级飞书官方插件：
```bash
npx -y @larksuite/openclaw-lark-tools update
```

---

## 高级配置指令

### 切换到流式输出

```bash
# 开启流式输出
openclaw config set channels.feishu.streaming true

# 关闭流式输出
openclaw config set channels.feishu.streaming false

# 开启耗时展示
openclaw config set channels.feishu.footer.elapsed true

# 开启状态展示
openclaw config set channels.feishu.footer.status true
```

### 设置多任务并行及独立上下文

```bash
# 开启话题群独立上下文
openclaw config set channels.feishu.threadSession true

# 关闭
openclaw config set channels.feishu.threadSession false
```

### 修改飞书机器人在群内的回复方式

**模式 1：只有 @机器人才回复（最常用）**
```bash
openclaw config set channels.feishu.requireMention true --json
```

**模式 2：不用 @，所有消息都回复**
⚠️ 注意：这个模式在大群里容易刷屏，谨慎使用！
```bash
openclaw config set channels.feishu.requireMention false --json
```

**模式 3：只有指定群 @机器人才回复（高级）**
```bash
# 先设置默认所有群都不需要 @
openclaw config set channels.feishu.requireMention open --json

# 然后给特定群设置需要 @
openclaw config set channels.feishu.groups.oc_xxxxxxxx.requireMention true --json
```

---

## 常用诊断命令

- `/feishu start`：确认是否安装成功
- `/feishu doctor`：检查配置是否正常
- `/feishu auth`：批量完成用户授权

### 终端诊断

```bash
# 检查问题并尝试自动修复
npx @larksuite/openclaw-lark-tools doctor --fix

# 查看版本信息
npx @larksuite/openclaw-lark-tools info

# 查看详细配置信息
npx @larksuite/openclaw-lark-tools info --all
```

---

## 常见问题

### 1. 权限不足怎么办？

在飞书开放平台 **权限管理** → **批量导入/导出权限** 中导入所需权限（详见官方文档）。

### 2. 插件安装完毕运行后，报 cannot find module xxx

**原因**：系统没有安装插件的依赖

**解决方法**：进入插件安装目录，运行 `npm install`

### 3. 升级到 OpenClaw 3.2 版本上无法正常调用工具

在 `openclaw.json` 中添加：
```json
{
  "tools": {
    "profile": "full",
    "sessions": {
      "visibility": "all"
    }
  }
}
```

### 4. 飞书官方插件与社区原有插件有何区别

飞书官方插件能以用户身份读写消息、文档等，体验更丝滑。

---

## 更新日志

### 2026.3.12
- 修复插件升级后 warning 提示"plugin id mismatch" 错误

### 2026.3.10
- **安装流程简化**：只需执行一行命令，3分钟内即可完成安装
- **支持多账号能力**：在飞书中为多个 Agent 配置独立的飞书应用机器人
- **飞书插件已正式开源**：https://github.com/larksuite/openclaw-lark

### 2026.3.8
- 支持电子表格的读写
- 支持在云文档中添加文件和图片
- AI 可感知表情反馈

### 2026.3.6
- 安装支持 Windows 环境
- 话题群和话题模式群能力升级

---

## 相关链接

- [OpenClaw 飞书官方插件 GitHub](https://github.com/larksuite/openclaw-lark)
- [OpenClaw 官方文档](https://docs.openclaw.ai)
- [飞书插件更新日志](./changelog.md)

---

*本文档由 awesome-claw-study 整理自飞书官方文档*
