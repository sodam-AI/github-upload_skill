🌏 [한국어](README.ko.md) | [English](README.md)

# GitHub Upload Skill for Claude

프로젝트를 심층 분석하여 GitHub 업로드에 필요한 모든 파일을 자동으로 생성·개선·번역하고, **원본 소스 코드 전체와 함께** 검증된 ZIP 하나로 정리하여 다운로드까지 제공하는 Claude 스킬입니다.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## 📋 Overview (개요)

멋진 프로젝트를 만들었는데 GitHub에 올리려니 README가 부실하고(혹은 아예 없고), .gitignore도 빠져 있고, API 키가 포함되진 않았는지 걱정되시나요?

이 스킬은 프로젝트를 분석해서 업로드에 필요한 파일 전체를 한번에 만들어줍니다. 전문가 수준의 영문/한글 README, 프레임워크에 맞는 .gitignore, LICENSE, .env.example, 민감 정보 점검까지. 원본 소스 파일과 신규/개선 파일을 **모두 합쳐** 검증된 ZIP 하나로 정리합니다. 다운로드해서 GitHub에 올리면 끝입니다.

## ✨ Features (주요 기능)

### 프로젝트 심층 분석
- 프로젝트 타입 자동 감지 (Node.js, Python, Flutter, React, Next.js, MCP 서버, Discord 봇 등)
- 실제 소스 코드를 읽어 기능, 기술 스택(Tech Stack), 명령어를 정확하게 파악
- 민감 파일(`.env`, API 키, 인증 정보) 스캔 및 제외 여부 확인
- 기존 README 품질 진단 및 구체적 개선점 식별

### 스마트 README 생성
- **새로 생성**: 실제 코드 분석 기반 — 추측 없는 정확한 README 작성
- **기존 다듬기**: 기존 내용 100% 보존하면서 구조와 품질 개선
- **타입별 특화 섹션**: MCP 서버는 Configuration, API는 Endpoints 테이블, CLI는 Commands 목록, Flutter는 Screenshots placeholder 자동 추가
- **한↔영 번역**: 기술 용어를 정확하게 처리하는 양방향 번역
- **이중 언어**: `README.md`(영문) + `README.ko.md`(한글) + 언어 전환 링크

### 지능형 .gitignore
- 프레임워크별 맞춤 템플릿 (Node.js, Python, Flutter, Rust, Go, Java, C# 등)
- 기존 `.gitignore`의 누락 항목 보완 — 기존 항목은 절대 삭제하지 않음
- 런타임 데이터 폴더(`data/`, `logs/`) 자동 감지 → 제외 규칙 + `.gitkeep` 생성

### 환경 변수 문서화
- `.env`에서 키를 추출해 `.env.example` 자동 생성 (실제 값은 플레이스홀더로 교체)
- API 키 발급 URL 자동 추가 (파악 가능한 경우)
- `.env`가 없어도 소스 코드의 `process.env` / `os.environ` 패턴을 스캔하여 생성

### 완전한 ZIP 패키징
- `rsync --exclude`로 안전하게 원본 복사 (나이브한 `cp -r` 사용 금지)
- 원본 소스 + 신규/수정 파일 전체를 하나의 ZIP으로 — 바로 GitHub 업로드 가능
- `.gitignore` 대상(node_modules, .env, dist 등)은 자동 제외
- lock 파일 스마트 처리: 앱/서버는 포함, 라이브러리는 선택

### 셀프 검증 (4단계)
- ZIP 생성 후 자동 10항목 체크리스트 검증
- 확인: 소스 파일 전수 확인, 민감 파일 미포함, LICENSE 연도 정확, 구조 클린
- 문제 발견 시 자동 수정 후 재패키징 — 사용자에게는 항상 검증된 패키지 전달

### GitHub 업로드 가이드
- 3가지 방법 안내: GitHub 웹(가장 쉬움), GitHub Desktop(GUI), Git CLI
- 초보자도 따라할 수 있는 단계별 설명

## 🛠️ Tech Stack (기술 스택)

| 분류 | 기술 |
|---|---|
| 플랫폼 | Claude Skills (Claude.ai / Claude Code) |
| 사용 도구 | `bash_tool`, `create_file`, `str_replace`, `view`, `present_files` |
| 안전 복사 | `rsync` + `--exclude` 패턴 |
| 검증 | `unzip -l`, `grep` 기반 검증 |
| 안내 언어 | 한국어 (기술 용어는 영문 병기) |

## 🚀 Getting Started (시작하기)

### Prerequisites (사전 요구사항)

- Claude Pro, Team, 또는 Enterprise 구독
- GitHub에 올리고 싶은 프로젝트

### Installation (설치)

1. `github-upload` 폴더를 다운로드합니다
2. Claude Skills 디렉토리에 배치합니다:
   - **Claude.ai**: 프로젝트 설정의 Skills 인터페이스에서 업로드
   - **Claude Code**: `~/.claude/skills/github-upload/`에 배치

### Usage (사용법)

Claude에게 자연어로 요청하기만 하면 됩니다:

```
이 프로젝트 깃허브에 올릴 수 있게 정리해줘
```
```
README 만들어줘
```
```
기존 README 좀 다듬어줘
```
```
README 영문 번역해줘
```
```
.gitignore 만들어줘
```

GitHub, README, .gitignore, LICENSE, 저장소 관련 키워드를 언급하면 스킬이 자동으로 트리거됩니다.

## 📁 Project Structure (프로젝트 구조)

```
github-upload/
├── SKILL.md          # 메인 스킬 정의 파일 (Claude에 대한 지시사항)
├── README.md         # 영문 문서
├── README.ko.md      # 한글 문서 (이 파일)
└── LICENSE           # MIT 라이선스
```

## 🎯 Trigger Keywords (트리거 키워드)

| 한국어 | English |
|---|---|
| 깃허브에 올려줘 | Upload to GitHub |
| README 만들어줘 | Create a README |
| README 다듬어줘 | Polish the README |
| README 번역해줘 | Translate the README |
| .gitignore 만들어줘 | Create .gitignore |
| LICENSE 추가해줘 | Add a LICENSE |
| 깃허브 정리해줘 | Prepare for GitHub |

## 📄 License (라이선스)

MIT 라이선스로 배포됩니다. 자세한 내용은 [LICENSE](LICENSE) 파일을 참조하세요.
