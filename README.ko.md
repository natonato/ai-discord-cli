# Qwen-Discord Bridge

Qwen CLI와 디스코드 채널을 연결하는 AI 어시스턴트 통합 도구입니다.

## 기능

- 🔗 Qwen CLI를 디스코드 채널에 연결
- 💬 양방향 통신: 디스코드 → Qwen → 디스코드
- 🔄 대화 컨텍스트를 유지하는 Qwen 세션
- ⚙️ `.env` 파일로 설정 가능
- 🚀 간단한 CLI 인터페이스

## 설치 및 설정

### 1. 의존성 설치

```bash
npm install
```

### 2. 빌드

```bash
npm run build
```

### 3. 전역 설치

빌드 후 전역으로 설치하여 어디서나 CLI를 실행할 수 있게 합니다:

```bash
npm install -g .
```

### 4. 설정 (프로젝트 디렉토리에서)

봇을 사용할 프로젝트 디렉토리로 이동하고 `.env` 파일을 생성하세요:

```bash
cd /path/to/your-project
```

`.env` 파일을 생성하고 값을 입력하세요:

```env
DISCORD_BOT_TOKEN=your_discord_bot_token_here
ALLOWED_CHANNEL_IDS=your_channel_id_here
QWEN_APPROVAL_MODE=yolo
```

이 저장소의 `.env.example`을 템플릿으로 사용할 수 있습니다.

### 5. 디스코드 봇 만들기

1. [Discord Developer Portal](https://discord.com/developers/applications) 접속
2. 새 애플리케이션 생성
3. "Bot" 섹션에서 봇 생성
4. 봇 토큰을 복사하여 프로젝트 `.env`의 `DISCORD_BOT_TOKEN`에 입력
5. **Privileged Gateway Intents** 활성화:
   - Message Content Intent
   - Guild Messages Intent
6. OAuth2 URL 생성기로 봇을 서버에 초대 (`bot` 스코프, `Send Messages` 및 `Read Message History` 권한 선택)
7. 대상 채널 ID 복사 (개발자 모드 → 채널 우클릭 → ID 복사) 후 프로젝트 `.env`의 `ALLOWED_CHANNEL_IDS`에 입력

### 6. 실행

`.env`가 있는 프로젝트 디렉토리에서 실행하세요:

```bash
qwen-discord start
```

전역 설치 후 어디서나 실행 가능:

```bash
qwen-discord start
```

## 사용법

봇이 실행 중인 상태에서 설정된 디스코드 채널에 메시지를 보내면:

1. 봇이 메시지를 수신
2. Qwen CLI로 전송
3. Qwen의 응답을 채널로 반환

### 명령어

| 명령어 | 설명 |
|---------|-------------|
| `qwen-discord start` | 브리지 봇 시작 |
| `!help` | 사용 가능한 명령어 표시 |
| `!session clear` | Qwen 세션 컨텍스트 초기화 |

## 설정 항목

| 변수 | 설명 | 기본값 |
|----------|-------------|---------|
| `DISCORD_BOT_TOKEN` | 디스코드 봇 토큰 (필수) | - |
| `ALLOWED_CHANNEL_IDS` | 허용된 채널 ID, 쉼표로 구분 (필수) | - |
| `ALLOWED_USER_IDS` | 허용된 사용자 ID, 쉼표로 구분 | 전체 사용자 |
| `QWEN_APPROVAL_MODE` | Qwen 승인 모드 | `yolo` |
| `QWEN_WORKING_DIR` | Qwen 작업 디렉토리 | 현재 디렉토리 |
| `LOG_LEVEL` | 로그 레벨 | `info` |

### 승인 모드

- `yolo`: 모든 작업 자동 승인 (신뢰된 환경에서 권장)
- `auto_edit`: 파일 수정 자동 승인
- `default`: 수동 승인 필요

## 아키텍처

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  Discord    │────▶│  Bridge Bot  │────▶│  Qwen CLI   │
│  Channel    │◀────│  (Node.js)   │◀────│  (spawn)    │
└─────────────┘     └──────────────┘     └─────────────┘
     │                    │                    │
     │  사용자 메시지      │  stdin/out         │  프로세스
     │  → 봇              │  텍스트 스트림      │  요청 단위
```

## 개발

```bash
# 개발 모드 (핫 리로드)
npm run dev

# 빌드
npm run build

# 전역 설치
npm install -g .
```

## 라이선스

MIT
