---
name: github-upload
description: "GitHub에 프로젝트를 업로드하거나 관리할 때 사용하는 스킬. 트리거: 'GitHub 업로드', 'git push', '깃허브에 올려줘', '레포 만들어줘', 'README 만들어줘', 'README 다듬어줘', 'README 번역해줘', '.gitignore 만들어줘', 'LICENSE 추가해줘', '깃허브 배포', 'GitHub에 공유', '레포지토리 생성', 'git 초기화', '커밋하고 푸시', 'push to GitHub' 등. 새 프로젝트의 첫 업로드, 기존 레포 변경사항 push, 레포 생성부터 업로드까지 전체 과정, README/LICENSE/.gitignore 자동 생성 및 개선, README 한영 번역을 모두 지원한다. 사용자가 GitHub이나 git을 언급하거나, 프로젝트를 공유/배포/업로드하고 싶다고 말하면 이 스킬을 반드시 사용하라. 코드 저장소 관련 작업이면 이 스킬을 적극 활용하라."
license: MIT
allowed-tools: bash_tool, create_file, str_replace, view, present_files
---

# GitHub Upload Skill

사용자의 프로젝트를 심층 분석하여 GitHub 업로드에 필요한 모든 파일을 자동으로 생성·개선·번역한 뒤, **원본 소스 파일 전체와 함께** ZIP 하나로 정리하여 다운로드를 제공하는 스킬이다. 사용자는 ZIP을 받아서 그대로 GitHub에 올리기만 하면 된다.

모든 안내는 한국어로 하되, 기술 용어는 원문 그대로 표기하고 괄호 안에 쉬운 설명을 덧붙인다.

---

## 실행 흐름

```
사용자가 프로젝트 파일 제공 (업로드 또는 경로 지정)
    ↓
[1] 프로젝트 심층 분석
    타입 감지 → 소스 코드 분석 → 기존 파일 점검 → 민감 정보 스캔
    ↓
    분석 결과 + 작업 계획을 사용자에게 보여주고 확인
    ↓
[2] 파일 생성/개선
    .gitignore → .env.example → README.md + README.ko.md → LICENSE → (선택) 고급 파일
    ↓
[3] ZIP 패키징
    원본 소스를 안전하게 복사(rsync) → 신규/수정 파일 덮어쓰기 → ZIP 생성
    ↓
[4] 셀프 검증
    ZIP 내용물 확인(unzip -l) → 체크리스트 10항목 검증 → 문제 시 자동 수정
    ↓
[5] 결과 전달
    검증 통과 → 다운로드 제공 + (요청 시) GitHub 업로드 방법 안내
```

---

## 핵심 원칙

1. **정확성 최우선**: 프로젝트 코드를 실제로 분석해서 정확한 정보만 넣는다. 추측 금지.
2. **원본 100% 보존**: 기존 파일을 다듬거나 번역할 때 기존 내용의 오류·누락이 절대 없어야 한다.
3. **전문가 품질**: 생성되는 모든 문서는 숙련된 개발자가 직접 작성한 것처럼 보여야 한다.
4. **자동 감지**: 사용자가 세부 지시를 하지 않아도, 프로젝트를 분석하여 누락된 파일을 찾아내고 한번에 처리한다.
5. **ZIP 완결성**: ZIP 하나만 받으면 바로 GitHub에 올릴 수 있어야 한다. **원본 소스 전체 + 신규/수정 파일**을 반드시 모두 포함한다. 신규 파일만 따로 주는 것은 금지.
6. **현재 연도 사용**: LICENSE 등에 연도를 넣을 때 반드시 오늘 날짜 기준의 현재 연도를 사용한다. package.json이나 기존 파일의 연도를 따르지 않는다.
7. **사전 확인**: 작업 시작 전 분석 결과와 작업 계획을 먼저 보여주고, 사용자가 확인하면 실행한다. 단, "알아서 해줘" 식으로 위임하면 바로 실행한다.
8. **셀프 검증**: ZIP 생성 후 반드시 내용물을 검증한다. 검증 없이 사용자에게 전달하지 않는다.

---

## [1] 프로젝트 심층 분석

요청을 받으면 가장 먼저 프로젝트의 현재 상태를 깊이 파악한다.

### 1-1. 프로젝트 타입 감지

프로젝트 루트의 주요 파일을 확인하여 타입을 추정한다:

| 감지 파일 | 프로젝트 타입 |
|---|---|
| `package.json` | Node.js / React / Vue / Next.js / Express / Svelte |
| `requirements.txt`, `pyproject.toml`, `setup.py` | Python |
| `pubspec.yaml` | Flutter / Dart |
| `Cargo.toml` | Rust |
| `go.mod` | Go |
| `pom.xml`, `build.gradle`, `build.gradle.kts` | Java / Kotlin |
| `Gemfile` | Ruby |
| `composer.json` | PHP / Laravel |
| `*.sln`, `*.csproj` | C# / .NET |
| `CMakeLists.txt`, `Makefile` | C / C++ |
| `*.html` + `*.css` 만 있음 | 정적 웹사이트 |
| `SKILL.md` | Claude Skill (문서 프로젝트) |

`package.json`이 있으면 `dependencies`와 `devDependencies`를 읽어 세부 프레임워크를 판별한다:

| 의존성 키워드 | 판별 결과 |
|---|---|
| `react`, `react-dom` | React |
| `next` | Next.js |
| `vue` | Vue.js |
| `nuxt` | Nuxt.js |
| `svelte` | Svelte / SvelteKit |
| `express`, `fastify`, `koa`, `hono` | Node.js 서버 |
| `@modelcontextprotocol/sdk` | MCP 서버 |
| `electron` | Electron 데스크톱 앱 |
| `react-native`, `expo` | React Native 모바일 앱 |
| `discord.js` | Discord 봇 |
| `telegraf`, `node-telegram-bot-api` | Telegram 봇 |

### 1-2. 소스 코드 심층 분석

단순 타입 감지를 넘어, 실제 소스 코드를 읽어서 프로젝트의 기능과 구조를 파악한다.

**분석 대상:**

1. **진입점 파일**: `src/index.ts`, `src/main.py`, `lib/main.dart`, `src/App.tsx` 등
   - 프로젝트가 무엇을 하는지 파악 (서버? CLI? UI? API?)
   - 주요 기능/모듈 식별

2. **의존성 파일**:
   - `package.json` → `name`, `version`, `description`, `scripts`, `dependencies`, `engines`
   - `requirements.txt` → 주요 라이브러리와 버전
   - `pubspec.yaml` → 프로젝트 설명, 의존성

3. **스크립트/명령어**: `package.json`의 `scripts` 등에서 설치/실행/빌드 명령어 파악

4. **디렉토리 구조**:
   ```bash
   find . -not -path '*/node_modules/*' -not -path '*/.git/*' -not -path '*/venv/*' -not -path '*/__pycache__/*' -not -path '*/dist/*' -not -path '*/build/*' | head -60
   ```

5. **환경 변수**: `.env` 파일이 있으면 키 이름만 추출 (값은 절대 포함하지 않음)

6. **설정 파일**: `tsconfig.json`, `vite.config.ts`, `next.config.js`, `tailwind.config.js`, `docker-compose.yml` 등에서 추가 기술 판별

### 1-3. 기존 파일 점검

아래 파일들의 존재 여부를 확인하고 상태를 진단한다:

| 파일 | 있으면 | 없으면 |
|---|---|---|
| `README.md` | 품질 진단 → 다듬기 | 새로 생성 |
| `README.ko.md` | 영문과 동기화 확인 | 한글판 생성 |
| `.gitignore` | 누락 항목 점검 → 보완 | 프로젝트 타입에 맞게 생성 |
| `.env` | 키 추출 → `.env.example` 생성 | - |
| `.env.example` | 내용 확인 | `.env`가 있으면 생성 |
| `LICENSE` | 연도/저작자 확인 | 라이선스 선택 후 생성 |

**README 파일명 비표준 감지**: GitHub 표준은 `README.md`(영문) + `README.ko.md`(한글). 비표준을 발견하면 표준으로 변경 제안:

| 발견된 파일명 | 표준 변환 |
|---|---|
| `README.md`가 한글이고 `README_EN.md`가 영문 | `README.md`(한글) → `README.ko.md`, `README_EN.md` → `README.md` |
| `README-en.md`, `README_EN.md`, `README.en.md` | → `README.md` |
| `README-ko.md`, `README_KR.md`, `README.kr.md` | → `README.ko.md` |

### 1-4. README 품질 진단

기존 README가 있으면 아래 기준으로 품질을 진단한다:

| 항목 | ✅ 양호 | ⚠️ 개선 필요 | ❌ 부족 |
|---|---|---|---|
| 프로젝트 설명 | 무엇이고 왜 필요한지 명확 | 너무 짧거나 모호 | 없음 |
| 기술 스택 | 버전까지 정확히 명시 | 일부만 나열 | 없음 |
| 설치/실행법 | 복붙 가능한 명령어 | 명령어 일부만 | 없음 |
| 프로젝트 구조 | 주요 파일에 설명 포함 | 구조만 있고 설명 없음 | 없음 |
| 라이선스 | 명시되어 있음 | 본문에만 언급 | 없음 |
| 마크다운 품질 | 깔끔한 포맷 | 일부 깨진 문법 | 읽기 어려움 |
| 뱃지 | 라이선스+핵심 기술 | 라이선스만 | 없음 |

### 1-5. 작업 계획 제시

분석 결과를 사용자에게 보고하고, 무엇을 할 것인지 명확하게 제시한다. 새로 생성하는 파일은 왜 필요한지 한 줄 설명을 반드시 붙인다.

```
📊 프로젝트 분석 결과
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
프로젝트 타입: Node.js + TypeScript (MCP Server)
주요 의존성:  @modelcontextprotocol/sdk, zod, express
스크립트:     build, start, dev
파일 수:      12개 (소스 3, 설정 4, 문서 3, 기타 2)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 작업 계획:
  [생성] .gitignore
    → node_modules(수천 개 패키지), dist(빌드물), .env(API 키) 등이
      GitHub에 올라가는 것을 방지
  [생성] .env.example
    → 필요한 환경 변수를 문서화 (실제 값은 제외)
  [생성] README.md (영문)
  [생성] README.ko.md (한글)
  [생성] LICENSE (MIT)
    → 없으면 다른 사람이 코드를 합법적으로 사용할 수 없음
  [유지] src/index.ts, package.json, tsconfig.json ...
    → 원본 소스 그대로 ZIP에 포함
  [제외] node_modules/, .env, dist/
    → 보안/용량 문제로 ZIP에서 제외
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
→ 진행할까요?
```

---

## [2] 파일 생성/개선

### 2-1. .gitignore

**공통 제외 항목** (모든 프로젝트):
```
# OS
.DS_Store
Thumbs.db
desktop.ini

# IDE
.idea/
.vscode/
*.swp
*.swo

# 환경 변수
.env
.env.local
.env*.local
```

**프로젝트별 추가 항목**:

| 프로젝트 타입 | 추가 제외 |
|---|---|
| Node.js 계열 | `node_modules/`, `dist/`, `.next/`, `out/`, `*.log`, `.nuxt/`, `.output/`, `.turbo/` |
| Python | `__pycache__/`, `*.pyc`, `.venv/`, `venv/`, `*.egg-info/`, `.pytest_cache/`, `.mypy_cache/` |
| Flutter/Dart | `.dart_tool/`, `build/`, `.flutter-plugins*`, `*.iml`, `.packages` |
| Rust | `target/`, `Cargo.lock` (라이브러리인 경우) |
| Go | `vendor/` (선택), 바이너리 파일 |
| Java/Kotlin | `build/`, `.gradle/`, `*.class`, `*.jar` (빌드 결과) |
| C# / .NET | `bin/`, `obj/`, `*.user`, `*.suo` |
| Claude Skill | `*.log`, `.cache/`, `*.tmp` |

기존 `.gitignore`가 있으면 기존 항목을 모두 유지하면서 누락된 항목만 카테고리별 주석과 함께 추가한다.

**런타임 데이터 폴더 처리**: `data/`, `logs/`, `tmp/`, `uploads/` 같은 런타임 폴더가 있으면:
- 폴더 내 데이터 파일을 `.gitignore`에 추가 (예: `data/*.json`, `logs/*.log`)
- 빈 폴더를 GitHub에 유지하기 위해 `.gitkeep` 파일 생성

### 2-2. .env.example (환경 변수 문서화)

프로젝트에 `.env` 파일이 있으면, 키 이름만 추출하여 `.env.example`을 생성한다.

**변환 규칙:**
```
# .env (원본 - 절대 공개 금지)
DATABASE_URL=postgresql://user:password@localhost:5432/mydb
OPENAI_API_KEY=sk-abc123...
PORT=3000

# .env.example (생성할 파일)
# Database connection string
DATABASE_URL=postgresql://user:password@localhost:5432/dbname
# OpenAI API key (https://platform.openai.com/api-keys)
OPENAI_API_KEY=your_openai_api_key_here
# Server port
PORT=3000
```

- API 키, 비밀번호, 토큰 → `your_xxx_here` 플레이스홀더로 교체
- 포트, 환경 모드 등 비밀이 아닌 값 → 기본값 유지
- 각 변수 위에 용도 설명 주석 + 키 발급 URL (파악 가능한 경우)

`.env`가 없고 코드에서 `process.env.XXX` 또는 `os.environ['XXX']` 패턴이 발견되면, 그 변수들로 `.env.example`을 생성한다.

### 2-3. README.md

#### A) README가 없는 경우 → 새로 생성

[1]에서 수집한 정보를 바탕으로 작성한다. 기본 구조:

```markdown
# 프로젝트명

> 한 줄 설명

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## 📋 Overview
## ✨ Features
## 🛠️ Tech Stack (테이블 형식)
## 🚀 Getting Started (Prerequisites → Installation → Environment Setup → Usage)
## 📁 Project Structure
## 📄 License
```

**프로젝트 타입별 특화 섹션** — 해당하는 경우 추가:

| 프로젝트 타입 | 추가 섹션 | 내용 |
|---|---|---|
| MCP 서버 | `## ⚙️ MCP Configuration` | `claude_desktop_config.json` 설정 예시, Available Tools 테이블 |
| REST API 서버 | `## 🔌 API Endpoints` | Method, Path, Description 테이블 |
| CLI 도구 | `## 💻 Commands` | 명령어 목록과 옵션 |
| Discord/Telegram 봇 | `## 🤖 Bot Commands` | 봇 명령어 목록 |
| Flutter 앱 | `## 📱 Screenshots` | 스크린샷 placeholder |
| React/Vue 웹앱 | `## 🖥️ Demo` | 데모 링크 placeholder |
| 라이브러리/패키지 | `## 📦 Installation` + `## 🔧 API Reference` | npm/pip 설치법, 주요 API 사용 예시 |
| Docker 포함 | `## 🐳 Docker` | docker-compose 사용법 |

**MCP 서버 특화 README 예시:**
```markdown
## ⚙️ MCP Configuration

### Claude Desktop
`claude_desktop_config.json`에 추가:
\```json
{
  "mcpServers": {
    "프로젝트명": {
      "command": "node",
      "args": ["path/to/dist/index.js"]
    }
  }
}
\```

### Available Tools
| Tool | Description |
|---|---|
| `tool_name` | 도구 설명 |
```

**작성 규칙:**
- **뱃지**: 라이선스 뱃지 항상 포함. Node.js/Python/TypeScript 뱃지는 해당 시만.
- **Features**: 실제 코드에서 확인된 기능만. 추측 금지.
- **Tech Stack**: 테이블 형식. 버전은 설정 파일의 실제 버전.
- **Getting Started**: `scripts`에서 확인된 명령어만.
- **Project Structure**: 실제 디렉토리 확인 후 주요 파일에 한 줄 설명.
- **이모지**: 섹션 제목(##)에만 사용.
- **.env.example이 있으면**: Environment Setup 섹션 반드시 포함.

#### B) 기존 README 다듬기

**절대 규칙: 기존 내용의 정보·의미를 100% 보존하면서** 품질을 높인다:
- 구조 정리, 문장 다듬기, 누락 섹션 추가, 특화 섹션 추가, 뱃지 추가, 마크다운 문법 교정, 일관성 확보
- 기존 정보 삭제 금지, 이미지 경로·외부 링크·기존 뱃지 유지

### 2-4. README 한↔영 번역

**번역 절대 규칙:**
1. **원본 내용 100% 보존**: 정보 누락·임의 추가 금지
2. **기술 용어**: 고유명사·명령어·코드블록 변경 금지. 한글판은 영문 병기 (예: "의존성(dependencies)")
3. **파일명**: 영문 `README.md` + 한글 `README.ko.md`, 각 파일 상단에 언어 전환 링크: `🌏 [한국어](README.ko.md) | [English](README.md)`
4. **구조 동기화**: 섹션 순서, 코드블록, 이미지, 뱃지, 링크 모두 동일

### 2-5. LICENSE 파일

LICENSE가 없으면 안내 후 생성. 모르겠다고 하면 MIT 추천.

| 라이선스 | 한 줄 설명 | 추천 상황 |
|---|---|---|
| MIT | 자유롭게 사용·수정·배포 가능 | 기본 추천 |
| Apache 2.0 | MIT + 특허권 보호 | 기업 프로젝트 |
| GPL 3.0 | 수정 시 동일 라이선스로 공개 필수 | 오픈소스 강제 유지 |

**현재 연도**(오늘 날짜 기준)와 저작자명을 자동으로 채운다.

### 2-6. GitHub 전용 파일 (선택)

| 파일 | 용도 | 제안 기준 |
|---|---|---|
| `CONTRIBUTING.md` | 기여 가이드 | 오픈소스 배포 목적 |
| `.github/ISSUE_TEMPLATE/` | 이슈 템플릿 | 팀/공개 프로젝트 |
| `CHANGELOG.md` | 변경 이력 | 라이브러리, CLI, 패키지 |
| `.github/FUNDING.yml` | 후원 링크 | 개인 오픈소스 |

개인/학습용에는 제안하지 않는다.

---

## [3] ZIP 패키징

⚠️ **핵심: ZIP = 원본 소스 전체 + 신규/수정 파일. 신규만 따로 주는 것 금지.**

### 3-1. 민감 정보 최종 점검

`.gitignore`에 포함 여부 확인:

| 위험 파일 | 설명 |
|---|---|
| `.env`, `.env.local`, `.env.production` | API 키, 비밀번호 |
| `*.pem`, `*.key`, `*.p12` | 인증서, 비밀키 |
| `credentials.json`, `serviceAccountKey.json` | 클라우드 인증 |
| `*.sqlite`, `*.db` | 로컬 DB |
| `id_rsa`, `id_ed25519` | SSH 키 |

### 3-2. 대용량 파일 점검

- 단일 파일 100MB 이상 → 경고 (Git LFS 필요)
- 이미지, 폰트, 동영상 → 필요 여부 확인

### 3-3. 안전한 파일 복사 및 ZIP 생성

⚠️ **`cp -r 원본/*`을 쓰지 않는다.** node_modules 등이 함께 복사되어 ZIP이 수백 MB가 될 수 있다.

**안전한 패키징 절차:**

```bash
# 변수 설정
PROJECT="프로젝트명"
SRC="원본경로"
DEST="/home/claude/github-ready/$PROJECT"
mkdir -p "$DEST"

# ① 안전하게 원본 복사 (rsync로 제외 대상을 명시적으로 빼고 복사)
rsync -a \
  --exclude='node_modules' \
  --exclude='.git' \
  --exclude='dist' \
  --exclude='build' \
  --exclude='.next' \
  --exclude='out' \
  --exclude='.env' \
  --exclude='.env.local' \
  --exclude='__pycache__' \
  --exclude='.venv' \
  --exclude='venv' \
  --exclude='.dart_tool' \
  --exclude='target' \
  --exclude='.turbo' \
  --exclude='.nuxt' \
  --exclude='.output' \
  "$SRC/" "$DEST/"

# rsync가 없으면 tar로 대체:
# cd "$SRC" && tar --exclude='node_modules' --exclude='.git' --exclude='dist' \
#   --exclude='.env' -cf - . | (cd "$DEST" && tar xf -)

# ② 신규/수정 파일 덮어쓰기
cp /home/claude/생성파일/README.md "$DEST/"
cp /home/claude/생성파일/README.ko.md "$DEST/"
cp /home/claude/생성파일/.gitignore "$DEST/"
cp /home/claude/생성파일/LICENSE "$DEST/"
# .env.example, .gitkeep 등도 해당 위치에 복사

# ③ 비표준 파일 정리 (해당 시)
# 예: README_EN.md → README.md로 대체된 경우
rm -f "$DEST/README_EN.md"

# ④ ZIP 생성 + 출력
cd /home/claude/github-ready && zip -r "/home/claude/$PROJECT.zip" "$PROJECT/"
cp "/home/claude/$PROJECT.zip" /mnt/user-data/outputs/
```

**rsync --exclude 목록은 2-1에서 생성한 .gitignore와 동기화한다.**

**lock 파일 처리**: 앱/서버 → 포함, 라이브러리 → 선택, 모르면 포함.

---

## [4] 셀프 검증

ZIP 생성 후, 사용자에게 전달하기 전에 반드시 수행한다.

### 4-1. ZIP 내용물 확인

```bash
unzip -l /home/claude/프로젝트명.zip
```

### 4-2. 체크리스트 (10항목)

| # | 검증 항목 | 실패 시 |
|---|---|---|
| 1 | 원본 소스 파일이 전부 있는가? | [3]으로 돌아가서 누락 파일 복사 |
| 2 | 설정 파일(package.json 등)이 있는가? | 누락 파일 복사 |
| 3 | .gitignore가 있는가? | [2]로 돌아가서 생성 |
| 4 | README.md + README.ko.md가 있는가? | [2]로 돌아가서 생성 |
| 5 | LICENSE가 있는가? | [2]로 돌아가서 생성 |
| 6 | node_modules가 없는가? | 제거 후 재압축 |
| 7 | .env가 없는가? | 제거 후 재압축 |
| 8 | dist/build 폴더가 없는가? | 제거 후 재압축 |
| 9 | 비표준 파일이 정리되었는가? | 제거 |
| 10 | LICENSE 연도가 현재 연도인가? | 수정 후 재압축 |

```bash
# 핵심 파일 존재 확인
unzip -l "$ZIP" | grep -E "(README\.md|README\.ko\.md|\.gitignore|LICENSE)"

# 제외 대상 미포함 확인
unzip -l "$ZIP" | grep -E "(node_modules|\.env$|/dist/|/build/)" \
  && echo "⚠️ 제외 대상 발견!" || echo "✅ 클린"

# LICENSE 연도 확인
unzip -p "$ZIP" "*/LICENSE" | head -3
```

### 4-3. 검증 실패 시

문제를 즉시 수정하고 ZIP을 다시 만든다. 중간 보고 없이, 수정 완료 후 최종 결과만 전달한다.

---

## [5] 결과 전달 + GitHub 업로드 안내

### 5-1. 완료 메시지

```
📋 GitHub 업로드 준비 완료!
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📦 ZIP 포함 파일 (총 N개):
  [원본 유지] src/index.ts, package.json, tsconfig.json ...
  [새로 생성] .gitignore, .env.example, LICENSE
  [개선]     README.md, README.ko.md
  [제외됨]   node_modules/, .env, dist/ (보안/용량)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ ZIP 다운로드 → GitHub에 업로드하면 끝!
```

**반드시 포함:**
- ZIP 파일 목록 (원본 유지 / 새로 생성 / 개선 / 제외됨 구분)
- 삭제 필요 파일 안내 (예: `README_EN.md` 대체)
- .env.example 생성 시: "배포 시 `.env` 파일을 만들어 값을 채워야 합니다" 안내

### 5-2. GitHub 업로드 방법 안내

사용자가 요청하면 아래 3가지 방법 중 하나를 안내한다:

**방법 1: GitHub 웹 (가장 쉬움)**
```
1. github.com 로그인 → "+" → "New repository"
2. Repository name 입력 → "Create repository"
3. "uploading an existing file" 클릭
4. ZIP에서 꺼낸 파일들을 드래그 앤 드롭
5. "Commit changes" → 끝!
```

**방법 2: GitHub Desktop (GUI)**
```
1. GitHub Desktop 설치 → File → New Repository
2. ZIP 파일들을 해당 폴더에 넣기
3. Summary에 "Initial commit" → "Commit to main"
4. "Publish repository" → 끝!
```

**방법 3: Git CLI**
```bash
cd 프로젝트폴더
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/사용자명/프로젝트명.git
git branch -M main
git push -u origin main
```

---

## 에러 방지 & 주의사항

### 자주 발생하는 실수

| 실수 | 원인 | 방지법 |
|---|---|---|
| ZIP에 원본 소스 빠짐 | 신규 파일만 ZIP에 넣음 | 원칙 5 + [4] 검증 항목 1,2 |
| LICENSE 연도 틀림 | 기존 파일의 연도 복사 | 원칙 6 + [4] 검증 항목 10 |
| node_modules 포함 | `cp -r *` 사용 | rsync --exclude + [4] 검증 항목 6 |
| .env 포함 | 민감 파일 제외 누락 | [3-1] 점검 + rsync --exclude + [4] 검증 항목 7 |
| 없는 스크립트 기재 | scripts 미확인 | 원칙 1: 실제 확인된 것만 |
| ZIP 수백 MB | 빌드물/미디어 포함 | [3-2] 대용량 점검 |
| 파일 인코딩 깨짐 | EUC-KR 등 | 복사 전 인코딩 확인 |

### 경로 주의사항

- Windows `\` → bash에서 따옴표로 감싸거나 `/`로 변환
- 한글·공백 포함 경로 → 반드시 따옴표: `"H:\내 프로젝트"`
- 특수문자(`&`, `(`, `)`, `!`) → 이스케이프 또는 따옴표
