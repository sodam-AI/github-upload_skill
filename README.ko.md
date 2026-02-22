🌏 [한국어](README.ko.md) | [English](README.md)

# GitHub Upload Skill for Claude

> 프로젝트를 심층 분석하여 GitHub 업로드에 필요한 모든 것 — README, .gitignore, LICENSE, PDF 문서 등 — 을 준비하고, 검증된 ZIP 하나로 제공하는 Claude 스킬입니다.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Platform: Claude Skills](https://img.shields.io/badge/Platform-Claude%20Skills-blueviolet)](https://claude.ai)

## 📋 Overview (개요)

멋진 프로젝트를 만들었는데 GitHub에 올리려니 README가 부실하고(혹은 아예 없고), .gitignore도 빠져 있고, API 키가 포함되진 않았는지 걱정되시나요?

이 스킬은 처음부터 끝까지 처리합니다: 코드베이스를 분석하고, 전문가 수준의 README(영문+한글), 프레임워크별 .gitignore, LICENSE, .env.example, 보안 점검, GitHub Description, 비개발자용 PDF까지 생성한 뒤, 원본 소스 파일과 함께 검증된 ZIP 하나로 패키징합니다. 기존 레포의 업데이트/릴리즈도 지원합니다. 다운로드해서 GitHub에 올리면 끝입니다.

## ✨ Features (주요 기능)

### 프로젝트 심층 분석
- 프로젝트 타입 및 세부 프레임워크 자동 감지 (Node.js, Python, Flutter, React, Next.js, MCP 서버, Discord 봇 등)
- 실제 소스 코드를 읽어 기능, 기술 스택(Tech Stack), 명령어를 정확하게 파악
- 민감 파일(`.env`, API 키, 인증 정보) 스캔 및 제외 여부 확인
- 기존 README를 9개 기준으로 품질 진단하고 구체적 개선점 식별

### 스마트 README 생성
- **새로 생성**: 실제 코드 분석 기반 — 추측 없는 정확한 README 작성
- **11개 이상 필수 섹션**: Overview, Features, Tech Stack, Prerequisites, Installation, Usage, How It Works, Project Structure, Troubleshooting, Security, Contributing, License — 모두 보장
- **타입별 특화 섹션**: MCP 서버는 Configuration, API는 Endpoints 테이블, CLI는 Commands 목록 자동 추가
- **추천 섹션**: Quick Start, FAQ, Supported Languages, Changelog, Acknowledgments 등 필요 시 추가
- **기존 다듬기**: 기존 내용 100% 보존하면서 구조와 품질 개선

### 이중 언어 문서
- `README.md`(영문)를 기본 파일로 작성 — GitHub에서 기본 표시되는 파일
- `README.ko.md`(한글)를 동일한 구조, 내용, 링크로 작성
- 양쪽 파일 상단에 언어 전환 링크
- 기술 용어는 양쪽 모두 원문 유지

### PDF 생성
- README 작성 후 동일 내용을 전문적인 PDF 문서로 변환
- 비개발자가 오프라인이나 인쇄물로 참고할 수 있도록 설계
- 모든 섹션, 테이블, 코드블록을 읽기 좋은 형식으로 포함
- ZIP과 함께 제공

### GitHub Description 생성
- GitHub 레포지토리 Description 필드에 넣을 한 줄 프로젝트 요약 생성
- 영문/한글 두 버전 제공
- 80자 이내, 핵심 가치 포함

### 환경 변수 문서화
- `.env`에서 키를 추출해 `.env.example` 자동 생성 (실제 값은 플레이스홀더로 교체)
- API 키 발급 URL 자동 추가 (파악 가능한 경우)
- `.env`가 없어도 소스 코드의 `process.env` / `os.environ` 패턴을 스캔하여 생성

### 지능형 .gitignore
- 프레임워크별 맞춤 템플릿 (Node.js, Python, Flutter, Rust, Go, Java, C# 등)
- 기존 항목은 절대 삭제하지 않고 누락 항목만 추가
- 런타임 데이터 폴더(`data/`, `logs/`) 자동 감지 → 제외 규칙 + `.gitkeep` 생성

### 완전한 ZIP 패키징
- `rsync --exclude`로 안전하게 원본 복사 (`cp -r` 사용 금지)
- 원본 소스 + 신규/수정 파일 전체를 하나의 ZIP으로 — 바로 GitHub 업로드 가능
- `.gitignore` 대상(node_modules, .env, dist 등)은 자동 제외
- lock 파일 스마트 처리: 앱/서버는 포함, 라이브러리는 선택

### 셀프 검증 (4단계)
- ZIP 생성 후 자동 10항목 체크리스트 검증
- 확인: 소스 파일 전수 확인, 민감 파일 미포함, LICENSE 연도 정확, 구조 클린
- 문제 발견 시 자동 수정 후 재패키징

### 업데이트 & 릴리즈 지원
- 첫 업로드인지 기존 레포 업데이트인지 자동 판별
- 업데이트 시: 변경 파일에 집중, CHANGELOG 항목 제안, 변경 요약 제공
- **시맨틱 버전 관리 안내**: MAJOR.MINOR.PATCH 규칙 설명 및 적절한 버전 번호 제안
- **CHANGELOG 생성**: [Keep a Changelog](https://keepachangelog.com) 형식으로 카테고리별(Added, Changed, Fixed 등) 정리
- **릴리즈 노트 생성**: GitHub Releases 페이지에 바로 붙여넣을 수 있는 사용자 친화적 릴리즈 노트 작성
- **GitHub Releases 가이드**: 웹, Desktop, CLI 세 가지 방법으로 릴리즈 생성하는 단계별 안내
- 기존 문서를 보존하면서 필요한 부분만 개선

## 🛠️ Tech Stack (기술 스택)

| 분류 | 기술 |
|---|---|
| 플랫폼 | Claude Skills (Claude.ai / Claude Code) |
| 사용 도구 | `bash_tool`, `create_file`, `str_replace`, `view`, `present_files` |
| 안전 복사 | `rsync` + `--exclude` 패턴 |
| PDF 생성 | Python `reportlab` 라이브러리 |
| 검증 | `unzip -l`, `grep` 기반 검증 |

## 🌐 Supported Languages (지원 언어)

| 언어 | 지원 |
|---|---|
| 한국어 (Korean) | ✅ 모든 안내를 한국어로 제공 |
| English (영어) | ✅ README.md를 영문으로 기본 생성 |
| 기술 용어 | 원문 유지 + 한국어 설명 병기 |

## 📦 Prerequisites (필수 환경)

- Claude Pro, Team, 또는 Enterprise 구독
- GitHub에 올리고 싶은 프로젝트
- Claude.ai 또는 Claude Code 환경

## 🚀 Installation (설치 방법)

### Claude.ai
1. `github-upload` 폴더(또는 ZIP)를 다운로드합니다
2. Claude 프로젝트 설정을 엽니다
3. Skills 섹션으로 이동합니다
4. `github-upload` 폴더 내용을 업로드합니다

### Claude Code
1. `github-upload` 폴더를 다운로드합니다
2. 스킬 디렉토리에 배치합니다:
   ```bash
   cp -r github-upload/ ~/.claude/skills/github-upload/
   ```

## 💻 Usage (사용 방법)

Claude에게 자연어로 요청하면 됩니다. GitHub, README, .gitignore, LICENSE 등 관련 키워드를 언급하면 스킬이 자동으로 트리거됩니다.

### 첫 업로드
```
이 프로젝트 깃허브에 올릴 수 있게 정리해줘
```
```
Prepare this project for GitHub upload
```

### 개별 작업
```
README 만들어줘                    # README 생성
기존 README 좀 다듬어줘           # 기존 README 개선
README 영문 번역해줘              # README 영문 번역
.gitignore 만들어줘               # .gitignore 생성
LICENSE 추가해줘                  # LICENSE 추가
Description 만들어줘              # GitHub Description 생성
```

### 업데이트 & 릴리즈
```
업데이트 내용 정리해줘            # 업데이트 준비
릴리즈 준비해줘                   # 릴리즈 준비
변경사항 정리해줘                 # 변경사항 정리
```

## ⚙️ How It Works (작동 방식)

이 스킬은 6단계 파이프라인을 따릅니다:

```
[1] 심층 분석
    프로젝트 타입 감지 → 소스 코드 분석 → 파일 점검 → 모드 판별
    ↓
[2] 파일 생성
    .gitignore → .env.example → README.md(영문) + README.ko.md(한글) → LICENSE
    ↓
[3] ZIP 패키징
    안전 복사(rsync --exclude) → 신규 파일 덮어쓰기 → ZIP 생성
    ↓
[4] 셀프 검증
    10항목 체크리스트 → 문제 시 자동 수정 → 필요 시 재패키징
    ↓
[5] PDF 생성
    README를 비개발자용 PDF 문서로 변환
    ↓
[6] 결과 전달
    ZIP + PDF + GitHub Description + 업로드 안내
```

각 단계에 안전장치가 내장되어 있습니다: rsync가 node_modules 포함을 방지하고, 10항목 체크리스트가 누락 파일이나 보안 유출을 잡아내며, LICENSE 연도는 항상 현재 날짜로 검증됩니다.

## 📁 Project Structure (프로젝트 구조)

```
github-upload/
├── SKILL.md          # 스킬 정의 — Claude에 대한 지시사항 (727줄)
├── README.md         # 영문 문서 (이 파일의 영문 버전)
├── README.ko.md      # 한글 문서 (이 파일)
└── LICENSE           # MIT 라이선스
```

## 🎯 Trigger Keywords (트리거 키워드)

| 한국어 | English |
|---|---|
| 깃허브 업로드 준비해줘 | Prepare for GitHub upload |
| README 만들어줘 | Create a README |
| README 다듬어줘 | Polish the README |
| README 번역해줘 | Translate the README |
| .gitignore 만들어줘 | Create .gitignore |
| LICENSE 추가해줘 | Add a LICENSE |
| 깃허브 정리해줘 | Prepare for GitHub |
| Description 만들어줘 | Generate Description |
| 업데이트 정리해줘 | Prepare update |
| 릴리즈 준비해줘 | Prepare release |
| 변경사항 정리해줘 | Organize changes |

## 🔧 Troubleshooting (문제 해결)

| 증상 | 원인 | 해결법 |
|---|---|---|
| ZIP에 소스 파일 없음 | 신규 파일만 패키징됨 | 재실행 — 항상 원본 포함 |
| ZIP이 수백 MB | node_modules나 빌드물 포함 | rsync 제외 목록이 .gitignore와 일치하는지 확인 |
| LICENSE 연도 틀림 | package.json의 연도를 복사 | 항상 현재 날짜 사용 — 재생성 |
| README에 틀린 명령어 | package.json scripts 미확인 | 확인된 스크립트만 사용 |
| PDF 미생성 | 단계 누락 또는 reportlab 미설치 | Claude에게 PDF 별도 생성 요청 |
| 한글 README 동기화 안 됨 | 영문만 수정됨 | 번역 재실행으로 동기화 |

## 🔒 Security (보안)

- **민감 파일 스캔**: `.env`, `*.pem`, `credentials.json`, SSH 키 등 위험 파일 자동 식별
- **제외 검증**: 모든 민감 파일이 `.gitignore`에 포함되었는지 패키징 전 확인
- **비밀 값 미포함**: `.env.example`에는 플레이스홀더만 포함, 실제 값은 절대 포함하지 않음
- **ZIP 안전성**: 셀프 검증에서 민감 파일 미포함을 최종 확인
- **소스 코드 프라이버시**: 모든 분석은 Claude 세션 내에서 수행 — 외부 전송 없음

## 🤝 Contributing (기여하기)

1. 레포지토리를 Fork합니다
2. Feature 브랜치를 생성합니다 (`git checkout -b feature/amazing-feature`)
3. 변경사항을 커밋합니다 (`git commit -m 'Add amazing feature'`)
4. 브랜치에 Push합니다 (`git push origin feature/amazing-feature`)
5. Pull Request를 생성합니다

### 기여 아이디어
- 더 많은 프로젝트 타입 지원 (Swift, Kotlin Multiplatform 등)
- PDF 포맷 및 스타일링 개선
- 한국어/영어 외 추가 언어 지원

## ❓ FAQ (자주 묻는 질문)

**Q: 이 스킬이 git 명령어를 실행하나요?**
A: 아닙니다. 이 스킬은 GitHub 업로드를 위해 프로젝트 파일을 준비하고 다듬는 역할입니다. 문서 생성/개선, ZIP 패키징, 업로드 안내를 제공하며, 실제 업로드는 사용자가 직접 합니다.

**Q: 원본 소스 코드를 수정하나요?**
A: 절대 수정하지 않습니다. 이 스킬은 문서 파일(README, .gitignore, LICENSE 등)만 생성/개선합니다. 소스 코드는 ZIP에 원본 그대로 포함됩니다.

**Q: 첫 업로드뿐 아니라 업데이트도 되나요?**
A: 네. 첫 업로드인지 업데이트인지 자동으로 판별하고, 업데이트 시에는 변경 파일에 집중하며 CHANGELOG 업데이트를 제안합니다.

**Q: README.md가 왜 영문인가요?**
A: GitHub는 README.md를 기본 프로젝트 페이지로 표시합니다. 영문이면 전 세계 개발자가 접근할 수 있습니다. 한글판(README.ko.md)은 항상 동일한 내용으로 생성되고 상단에 링크됩니다.

**Q: PDF는 뭔가요?**
A: README와 동일한 내용의 PDF 문서를 자동 생성합니다. 비개발자에게 공유하거나 오프라인 참고용으로 활용할 수 있습니다.

## 🙏 Acknowledgments (감사의 말)

- [바이브코딩(Vibe Coding)](https://github.com) 커뮤니티를 위해 제작
- 비개발자도 GitHub을 쉽게 사용할 수 있도록 설계
- 오픈소스 문서화 모범 사례에서 영감

## 📄 License (라이선스)

이 프로젝트는 MIT 라이선스로 배포됩니다. 자세한 내용은 [LICENSE](LICENSE) 파일을 참조하세요.
