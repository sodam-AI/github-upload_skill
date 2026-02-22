# GitHub Upload Skill v1.1.0

> README가 대폭 강화되고, PDF 생성·GitHub Description 제안 기능이 추가된 업데이트입니다.

---

## ✨ 새로운 기능

### 📄 PDF 자동 생성
README 작성 후 동일한 내용을 전문적인 PDF 문서로 자동 변환합니다.
비개발자에게 공유하거나 오프라인 참고용으로 활용할 수 있습니다.
ZIP에 포함되고 별도 파일로도 제공됩니다.

### 💬 GitHub Description 자동 제안
분석 결과를 바탕으로 GitHub 레포지토리 Description 필드에 바로 붙여넣을 수 있는 한 줄 요약을 생성합니다. 영문/한글 두 버전 제공, 80자 이내.

---

## 🔧 개선된 기능

### 업데이트 & 릴리즈 지원 대폭 강화
- 시맨틱 버저닝(MAJOR.MINOR.PATCH) 규칙 설명 및 버전 번호 제안
- [Keep a Changelog](https://keepachangelog.com) 형식의 CHANGELOG 자동 생성
- GitHub Releases 페이지에 바로 붙여넣을 릴리즈 노트 생성
- GitHub Releases 생성 단계별 가이드 (웹 / Desktop / CLI 3가지 방법)

### README 문서 구성 강화
새로운 섹션 추가로 문서 완성도를 높였습니다:
- **How It Works**: 6단계 파이프라인 다이어그램
- **Troubleshooting**: 자주 발생하는 6가지 문제와 해결법
- **FAQ**: 자주 묻는 질문 5개
- **Supported Languages**: 언어 지원 현황 표
- **Acknowledgments**: 감사의 말

### 사용성 개선
- 트리거 키워드 4개 → 11개로 확장 (업데이트·릴리즈 관련 키워드 추가)
- 설치 가이드: Claude.ai / Claude Code 각각 상세 단계 안내
- 사용 예시: 첫 업로드 / 개별 작업 / 업데이트·릴리즈 카테고리로 정리

---

## 📦 포함 파일

```
github-upload/
├── SKILL.md          # 스킬 정의 (Claude 지시사항)
├── README.md         # 영문 문서 (업데이트됨)
├── README.ko.md      # 한글 문서 (업데이트됨)
├── CHANGELOG.md      # 변경 이력
└── LICENSE           # MIT 라이선스
```

---

**Full Changelog**: [v1.0.0 → v1.1.0](https://github.com/사용자명/github-upload-skill/compare/v1.0.0...v1.1.0)
