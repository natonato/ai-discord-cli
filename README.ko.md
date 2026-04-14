# AI-Discord Bridge

AI CLI(Qwen/Gemini)를 디스코드 채널과 연결하여 원활한 AI 어시스턴트 통합을 지원합니다.

[English](./README.md) | [한국어](./README.ko.md)

## 주요 기능

- 🔗 Qwen CLI 또는 Gemini CLI를 디스코드 채널에 연결
- 💬 양방향 통신: 디스코드 → AI → 디스코드
- 🔄 대화 컨텍스트를 유지하는 지속적인 AI 세션
- ⚙️ `.env` 파일로 설정 가능
- 🚀 간단한 CLI 인터페이스
- 🤖 멀티 프로바이더 지원 (Qwen, Gemini)

## 설정 방법

### 1. 의존성 설치

```bash
npm install
```

### 2. 빌드

```bash
npm run build
```

### 3. 전역 설치

빌드 후, 어디서나 실행할 수 있도록 CLI를 전역으로 설치합니다:

```bash
npm install -g .
```

### 4. 설정 (프로젝트 디렉토리 내)

봇을 사용하려는 프로젝트 디렉토리로 이동하여 `.env` 파일을 생성합니다:

```bash
cd /path/to/your-project
```

다음 값들로 `.env` 파일을 작성합니다:

```env
DISCORD_BOT_TOKEN=your_discord_bot_token_here
ALLOWED_CHANNEL_IDS=your_channel_id_here
AI_PROVIDER=qwen
APPROVAL_MODE=yolo
```

이 레포지토리의 `.env.example` 파일을 템플릿으로 사용할 수 있습니다.

### 5. 디스코드 봇 생성

1. [Discord Developer Portal](https://discord.com/developers/applications)에 접속합니다.
2. 새 애플리케이션(New Application)을 생성합니다.
3. "Bot" 섹션에서 봇을 생성합니다.
4. 봇 토큰을 복사하여 프로젝트 `.env`의 `DISCORD_BOT_TOKEN`에 입력합니다.
5. 다음 **Privileged Gateway Intents**를 활성화합니다:
   - Message Content Intent
   - Guild Messages Intent
6. OAuth2 URL 생성기를 사용하여 봇을 서버에 초대합니다 (`bot` 스코프 선택, `Send Messages` 및 `Read Message History` 권한 필요).
7. 대상 채널 ID를 가져와(개발자 모드 활성화 → 채널 우클릭 → ID 복사) 프로젝트 `.env`의 `ALLOWED_CHANNEL_IDS`에 설정합니다.

### 6. AI CLI 프로바이더 설치

**Qwen의 경우:**
```bash
npm install -g @qwen-code/qwen-code
```

**Gemini의 경우:**
```bash
npm install -g @google/gemini-cli
```

### 7. 실행

`.env` 파일이 있는 프로젝트 디렉토리에서 다음을 실행합니다:

```bash
ai-discord start
```

전역으로 설치된 경우 어디서나 실행 가능합니다:

```bash
ai-discord start
```

## 사용법

봇이 실행되면 설정된 디스코드 채널에서 메시지를 보내기만 하면 됩니다. 봇은 다음을 수행합니다:

1. 메시지 수신
2. 설정된 AI CLI(Qwen 또는 Gemini)로 전송
3. AI의 응답을 다시 채널로 반환

### 명령어

| 명령어 | 설명 |
|---------|-------------|
| `ai-discord start` | 브리지 봇 시작 |
| `!help` | 사용 가능한 명령어 표시 |
| `!session clear` | AI 세션 컨텍스트를 초기화하고 새로 시작 |

## 설정 항목

| 변수 | 설명 | 기본값 |
|----------|-------------|---------|
| `DISCORD_BOT_TOKEN` | 디스코드 봇 토큰 (필수) | - |
| `ALLOWED_CHANNEL_IDS` | 허용된 채널 ID, 쉼표로 구분 (필수) | - |
| `ALLOWED_USER_IDS` | 허용된 사용자 ID, 쉼표로 구분 | 모든 사용자 |
| `AI_PROVIDER` | AI CLI 프로바이더: `qwen` 또는 `gemini` | `qwen` |
| `APPROVAL_MODE` | AI 승인 모드 | `yolo` |
| `WORKING_DIR` | AI 작업 디렉토리 | 현재 디렉토리 |
| `LOG_LEVEL` | 로그 레벨 | `info` |

### 승인 모드 (Approval Modes)

- `yolo`: 모든 작업 자동 승인 (신뢰할 수 있는 환경에서 권장)
- `auto_edit`: 파일 수정 자동 승인
- `default`: 수동 승인 필요

## 아키텍처

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  Discord    │────▶│  Bridge Bot  │────▶│  AI CLI     │
│  Channel    │◀────│  (Node.js)   │◀────│  (spawn)    │
└─────────────┘     └──────────────┘     └─────────────┘
     │                    │                    │
     │  사용자 메시지      │  stdin/out         │  프로세스
     │  → 봇              │  텍스트 스트림      │  요청당 생성
```

## 개발

```bash
# 핫 리로드를 포함한 개발 모드
npm run dev

# 빌드
npm run build

# CLI 액세스를 위한 전역 설치
npm install -g .
```

## 라이선스

MIT
