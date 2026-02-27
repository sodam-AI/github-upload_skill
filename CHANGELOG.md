# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

## [1.2.0] - 2026-02-28

### Added
- **GitHub Description Generator**: Automatically generates one-line project summary for GitHub repository Description field (English & Korean)
- **GitHub Topics Recommender**: Suggests 5-10 relevant tags for GitHub repository Topics (improves discoverability)
- **Monorepo Support**: Auto-detects and handles monorepo projects (npm/yarn/pnpm workspaces, Lerna, Nx, Turborepo)
  - Generates root README with Package overview and Architecture sections
  - Supports per-package README generation
- **PDF Document Generation**: Converts README to professional PDF for non-developers (included in ZIP + separate download)
- **CHANGELOG Management**: Guides for creating and updating CHANGELOG.md in Keep a Changelog format
- **Release Notes Generator**: Creates GitHub Releases-ready release notes with changelog categorization (Added/Changed/Fixed/Deprecated/Removed)
- **GitHub Releases Guide**: Step-by-step instructions for creating releases via Web, Desktop, and CLI
- **Social Preview Image Guide**: Explains OG image setup for GitHub repository links (1280×640px recommended)
- **Comprehensive FAQ Section**: 6 common questions and answers in README
- **Troubleshooting Guide**: 6 common issues with solutions
- **Security Section**: Dedicated documentation on sensitive file scanning and exclusion verification
- **Enhanced Installation Guide**: Separate instructions for Claude.ai and Claude Code

### Changed
- **SKILL.md Massive Expansion**: 555 lines → 1,265 lines (+128%)
  - New GitHub Description generation logic (Section 1-6)
  - New GitHub Topics recommendation system (Section 1-6-1)
  - Expanded monorepo detection and handling (Section 1-1)
  - New CHANGELOG.md management guide (Section 2-7)
  - New Release notes generation guide (Section 2-8)
  - Complete PDF generation process with code examples (Section 5)
  - Social Preview image setup guide (Section 6-2-1)
  - GitHub Releases creation step-by-step (Section 6-4)
  - Comprehensive error prevention with auto-recovery procedures
- **README.md Comprehensive Overhaul**: 
  - 12 detailed Features with explanations
  - Troubleshooting table (6 items)
  - FAQ section (6 Q&A)
  - Security best practices
  - Contributing guidelines
  - Better organized trigger keywords
- **Self-Verification Enhancement**: 10 items → 15 items
  - Auto-recovery procedures for each potential error
  - Detailed error prevention documentation
- **Mode Detection System**: Explicit priority-based system for Initial vs Update detection

### Improved
- **Error Prevention**: Expanded from 7 items to 11 items with auto-recovery procedures
- **Monorepo Handling**: Full detection criteria + per-package README strategy
- **Trigger Keywords**: Better categorization (Initial / Tasks / Updates & Releases)
- **Security Documentation**: Enhanced sensitive file scanning details
- **Windows Compatibility**: Added path handling guidance

### Fixed
- **License Year**: Now always uses current date (`date +%Y`), not package.json date
- **README Standardization**: Detailed non-standard filename detection + conversion rules
- **Lock File Policy**: Clarified for apps (include) vs libraries (optional)

---

## [1.1.0] - 2025-12-XX

### Added
- PDF auto-generation from README
- GitHub Description suggestion
- Update & release support with semantic versioning
- Enhanced README sections (How It Works, Troubleshooting, FAQ)
- Installation guide expansion

---

## [1.0.0] - 2025-11-XX

### Added
- Initial release
- Deep project analysis
- README generation (EN & KO)
- .gitignore, LICENSE, .env.example generation
- ZIP packaging with verification
- GitHub upload guidance

---

[Unreleased]: https://github.com/sodam-ai/github-upload-skill/compare/v1.2.0...HEAD
[1.2.0]: https://github.com/sodam-ai/github-upload-skill/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/sodam-ai/github-upload-skill/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/sodam-ai/github-upload-skill/releases/tag/v1.0.0
