# AI-Discord Bridge

将 AI CLI (Qwen/Gemini) 与 Discord 频道连接，实现无缝的 AI 助手集成。

[English](./README.md) | [한국어](./README.ko.md) | [简体中文](./README.zh.md) | [日本語](./README.ja.md)

## 功能特点

- 🔗 将 Qwen CLI 或 Gemini CLI 连接到任何 Discord 频道
- 💬 双向通信：Discord → AI → Discord
- 🔄 持久 AI 会话，保留对话上下文
- ⚙️ 通过 `.env` 文件进行配置
- 🚀 简单的 CLI 界面
- 🤖 多供应商支持 (Qwen, Gemini)

## 设置步骤

### 1. 安装依赖

```bash
npm install
```

### 2. 构建

```bash
npm run build
```

### 3. 全局安装

构建完成后，全局安装 CLI 以便在任何地方运行：

```bash
npm install -g .
```

### 4. 配置（在您的项目目录中）

进入您想要使用机器人的项目目录，并创建 `.env` 文件：

```bash
cd /path/to/your-project
```

填写 `.env` 文件：

```env
DISCORD_BOT_TOKEN=your_discord_bot_token_here
ALLOWED_CHANNEL_IDS=your_channel_id_here
AI_PROVIDER=qwen
APPROVAL_MODE=yolo
```

您可以使用此仓库中的 `.env.example` 作为模板。

### 5. 创建 Discord 机器人

1. 前往 [Discord Developer Portal](https://discord.com/developers/applications)
2. 创建一个新应用 (New Application)
3. 在 "Bot" 部分创建一个机器人
4. 将机器人令牌 (Token) 复制到项目 `.env` 中的 `DISCORD_BOT_TOKEN`
5. 启用以下 **Privileged Gateway Intents**:
   - Message Content Intent
   - Guild Messages Intent
6. 使用 OAuth2 URL 生成器将机器人邀请到您的服务器（选择 `bot` 范围，并勾选 `Send Messages` 和 `Read Message History` 权限）
7. 获取目标频道 ID（开启开发者模式 → 右键点击频道 → 复制 ID），并设置到项目 `.env` 的 `ALLOWED_CHANNEL_IDS`

### 6. 安装 AI CLI 供应商

**对于 Qwen:**
```bash
npm install -g @qwen-code/qwen-code
```

**对于 Gemini:**
```bash
npm install -g @google/gemini-cli
```

### 7. 运行

在包含 `.env` 文件的项目目录中运行：

```bash
ai-discord start
```

如果已全局安装，也可以在任何地方运行：

```bash
ai-discord start
```

## 使用方法

机器人运行后，只需在配置的 Discord 频道中发送消息即可。机器人将：

1. 接收您的消息
2. 将其发送到配置的 AI CLI (Qwen 或 Gemini)
3. 将 AI 的响应返回到频道中

### 命令

| 命令 | 描述 |
|---------|-------------|
| `ai-discord start` | 启动桥接机器人 |
| `!help` | 显示可用命令 |
| `!session clear` | 清除 AI 会话上下文并重新开始 |

## 配置选项

| 变量 | 描述 | 默认值 |
|----------|-------------|---------|
| `DISCORD_BOT_TOKEN` | Discord 机器人令牌 (必填) | - |
| `ALLOWED_CHANNEL_IDS` | 允许的频道 ID，逗号分隔 (必填) | - |
| `ALLOWED_USER_IDS` | 允许的用户 ID，逗号分隔 | 所有用户 |
| `AI_PROVIDER` | AI CLI 供应商: `qwen` 或 `gemini` | `qwen` |
| `APPROVAL_MODE` | AI 审批模式 | `yolo` |
| `WORKING_DIR` | AI 工作目录 | 当前目录 |
| `LOG_LEVEL` | 日志级别 | `info` |

### 审批模式 (Approval Modes)

- `yolo`: 自动批准所有操作（推荐用于受信任的环境）
- `auto_edit`: 自动批准文件编辑
- `default`: 需要手动批准

## 架构

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  Discord    │────▶│  Bridge Bot  │────▶│  AI CLI     │
│  Channel    │◀────│  (Node.js)   │◀────│  (spawn)    │
└─────────────┘     └──────────────┘     └─────────────┘
     │                    │                    │
     │  用户消息           │  stdin/out         │  进程
     │  → 机器人           │  文本流            │  逐请求创建
```

## 开发

```bash
# 带有热重载的开发模式
npm run dev

# 构建
npm run build

# 全局安装以进行 CLI 访问
npm install -g .
```

## 许可证

MIT
