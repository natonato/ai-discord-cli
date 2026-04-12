# Qwen-Discord Bridge

Bridge Qwen CLI with Discord channels for seamless AI assistant integration.

## Features

- 🔗 Connect Qwen CLI to any Discord channel
- 💬 Bidirectional communication: Discord → Qwen → Discord
- 🔄 Persistent Qwen session with conversation context
- ⚙️ Configurable via `.env` file
- 🚀 Simple CLI interface

## Setup

### 1. Install Dependencies

```bash
npm install
```

### 2. Build

```bash
npm run build
```

### 3. Install Globally

After building, install the CLI globally so you can run it from anywhere:

```bash
npm install -g .
```

### 4. Configure (in your project directory)

Navigate to the project directory where you want to use the bot, and create a `.env` file there:

```bash
cd /path/to/your-project
```

Create `.env` with your values:

```env
DISCORD_BOT_TOKEN=your_discord_bot_token_here
ALLOWED_CHANNEL_IDS=your_channel_id_here
QWEN_APPROVAL_MODE=yolo
```

You can use `.env.example` in this repo as a template.

### 5. Create a Discord Bot

1. Go to [Discord Developer Portal](https://discord.com/developers/applications)
2. Create a new application
3. Go to "Bot" section and create a bot
4. Copy the bot token to `DISCORD_BOT_TOKEN` in your project's `.env`
5. Enable these **Privileged Gateway Intents**:
   - Message Content Intent
   - Guild Messages Intent
6. Invite the bot to your server using OAuth2 URL generator (select `bot` scope with `Send Messages` and `Read Message History` permissions)
7. Get the target channel ID (Developer Mode → Right-click channel → Copy ID) and set `ALLOWED_CHANNEL_IDS` in your project's `.env`

### 6. Run

From your project directory (where `.env` is located), run:

```bash
qwen-discord start
```

Or from anywhere if installed globally:

```bash
qwen-discord start
```

## Usage

Once the bot is running, simply send messages in the configured Discord channel. The bot will:

1. Receive your message
2. Send it to Qwen CLI
3. Return Qwen's response back to the channel

### Commands

| Command | Description |
|---------|-------------|
| `qwen-discord start` | Start the bridge bot |
| `!help` | Show available commands |
| `!session clear` | Clear Qwen session context and start fresh |

## Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| `DISCORD_BOT_TOKEN` | Discord bot token (required) | - |
| `ALLOWED_CHANNEL_IDS` | Allowed channel IDs, comma-separated (required) | - |
| `ALLOWED_USER_IDS` | Allowed user IDs, comma-separated | All users |
| `QWEN_APPROVAL_MODE` | Qwen approval mode | `yolo` |
| `QWEN_WORKING_DIR` | Qwen working directory | Current directory |
| `LOG_LEVEL` | Log level | `info` |

### Approval Modes

- `yolo`: Auto-approve all actions (recommended for trusted environments)
- `auto_edit`: Auto-approve file edits
- `default`: Manual approval required

## Architecture

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  Discord    │────▶│  Bridge Bot  │────▶│  Qwen CLI   │
│  Channel    │◀────│  (Node.js)   │◀────│  (spawn)    │
└─────────────┘     └──────────────┘     └─────────────┘
     │                    │                    │
     │  User message      │  stdin/out         │  Process
     │  → Bot             │  text stream       │  per-request
```

## Development

```bash
# Development mode with hot reload
npm run dev

# Build
npm run build

# Install globally for CLI access
npm install -g .
```

## License

MIT
