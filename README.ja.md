# AI-Discord Bridge

AI CLI (Qwen/Gemini) を Discord チャンネルと接続し、シームレスな AI アシスタント統合を実現します。

[English](./README.md) | [한국어](./README.ko.md) | [简体中文](./README.zh.md) | [日本語](./README.ja.md)

## 主な機能

- 🔗 Qwen CLI または Gemini CLI を任意の Discord チャンネルに接続
- 💬 双方向通信: Discord → AI → Discord
- 🔄 会話コンテキストを維持する持続的な AI セッション
- ⚙️ `.env` ファイルによる設定
- 🚀 シンプルな CLI インターフェース
- 🤖 マルチプロバイダー対応 (Qwen, Gemini)

## セットアップ

### 1. 依存関係のインストール

```bash
npm install
```

### 2. ビルド

```bash
npm run build
```

### 3. グローバルインストール

ビルド後、どこからでも実行できるように CLI をグローバルにインストールします：

```bash
npm install -g .
```

### 4. 設定 (プロジェクトディレクトリ内)

ボットを使用したいプロジェクトディレクトリに移動し、`.env` ファイルを作成します：

```bash
cd /path/to/your-project
```

以下の値を `.env` に設定します：

```env
DISCORD_BOT_TOKEN=your_discord_bot_token_here
ALLOWED_CHANNEL_IDS=your_channel_id_here
AI_PROVIDER=qwen
APPROVAL_MODE=yolo
```

このリポジトリの `.env.example` をテンプレートとして使用できます。

### 5. Discord ボットの作成

1. [Discord Developer Portal](https://discord.com/developers/applications) にアクセスします
2. 新しいアプリケーション (New Application) を作成します
3. "Bot" セクションでボットを作成します
4. ボットトークンをコピーし、プロジェクトの `.env` 内の `DISCORD_BOT_TOKEN` に貼り付けます
5. 以下の **Privileged Gateway Intents** を有効にします：
   - Message Content Intent
   - Guild Messages Intent
6. OAuth2 URL ジェネレーターを使用してボットをサーバーに招待します（`bot` スコープを選択し、`Send Messages` と `Read Message History` 権限を付与します）
7. 対象のチャンネル ID を取得し（開発者モードを有効化 → チャンネルを右クリック → ID をコピー）、プロジェクトの `.env` 内の `ALLOWED_CHANNEL_IDS` に設定します

### 6. AI CLI プロバイダーのインストール

**Qwen の場合:**
```bash
npm install -g @qwen-code/qwen-code
```

**Gemini の場合:**
```bash
npm install -g @google/gemini-cli
```

### 7. 実行

`.env` ファイルがあるプロジェクトディレクトリから実行します：

```bash
ai-discord start
```

グローバルにインストールされている場合は、どこからでも実行可能です：

```bash
ai-discord start
```

## 使い方

ボットが起動したら、設定した Discord チャンネルでメッセージを送信するだけです。ボットは以下の動作を行います：

1. メッセージを受信
2. 設定された AI CLI (Qwen または Gemini) に送信
3. AI の応答をチャンネルに返信

### コマンド

| コマンド | 説明 |
|---------|-------------|
| `ai-discord start` | ブリッジボットを開始 |
| `!help` | 利用可能なコマンドを表示 |
| `!session clear` | AI セッションのコンテキストをクリアして新しく開始 |

## 設定項目

| 変数 | 説明 | デフォルト値 |
|----------|-------------|---------|
| `DISCORD_BOT_TOKEN` | Discord ボットトークン (必須) | - |
| `ALLOWED_CHANNEL_IDS` | 許可されたチャンネル ID (カンマ区切り、必須) | - |
| `ALLOWED_USER_IDS` | 許可されたユーザー ID (カンマ区切り) | すべてのユーザー |
| `AI_PROVIDER` | AI CLI プロバイダー: `qwen` または `gemini` | `qwen` |
| `APPROVAL_MODE` | AI 承認モード | `yolo` |
| `WORKING_DIR` | AI 作業ディレクトリ | カレントディレクトリ |
| `LOG_LEVEL` | ログレベル | `info` |

### 承認モード (Approval Modes)

- `yolo`: すべてのアクションを自動承認（信頼できる環境で推奨）
- `auto_edit`: ファイル編集を自動承認
- `default`: 手動承認が必要

## アーキテクチャ

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  Discord    │────▶│  Bridge Bot  │────▶│  AI CLI     │
│  Channel    │◀────│  (Node.js)   │◀────│  (spawn)    │
└─────────────┘     └──────────────┘     └─────────────┘
     │                    │                    │
     │  ユーザーメッセージ  │  stdin/out         │  プロセス
     │  → ボット           │  テキストストリーム  │  リクエスト毎に作成
```

## 開発

```bash
# ホットリロード付きのデバッグモード
npm run dev

# ビルド
npm run build

# CLI アクセスのためのグローバルインストール
npm install -g .
```

## ライセンス

MIT
