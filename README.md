🌏 [한국어](README.ko.md) | [English](README.md)

# GitHub Upload Skill for Claude

> A Claude Skill that deep-analyzes your project and prepares everything for a clean GitHub upload — README, .gitignore, LICENSE, PDF docs, and more — all in one verified ZIP.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Platform: Claude Skills](https://img.shields.io/badge/Platform-Claude%20Skills-blueviolet)](https://claude.ai)

## 📋 Overview

You've built something great. Now you want it on GitHub — but the README is rough (or missing), there's no .gitignore, you're not sure your API keys are safe, and you don't know where to start.

This skill handles it end-to-end: analyzes your codebase, generates professional README (English + Korean), framework-specific .gitignore, LICENSE, .env.example, security checks, a GitHub Description, and a PDF version for non-developers — then packages **everything** (your original source files + all new/improved files) into one verified ZIP. It also supports updates and releases for existing repositories. Download it, upload to GitHub. Done.

## ✨ Features

### Deep Project Analysis
- Auto-detects project type and sub-framework (Node.js, Python, Flutter, React, Next.js, MCP Server, Discord Bot, etc.)
- Reads actual source code to extract accurate features, tech stack, and commands
- Scans for sensitive files (`.env`, API keys, credentials) and verifies they're excluded
- Rates existing README quality across 9 criteria and identifies specific improvements

### Smart README Generation
- **Create from scratch**: Builds README by analyzing your actual code — zero guessing
- **11+ mandatory sections**: Overview, Features, Tech Stack, Prerequisites, Installation, Usage, How It Works, Project Structure, Troubleshooting, Security, Contributing, License — all guaranteed
- **Type-specific sections**: MCP servers get Configuration section, APIs get Endpoints table, CLI tools get Commands list
- **Recommended sections**: Quick Start, FAQ, Supported Languages, Changelog, Acknowledgments added when relevant
- **Polish existing**: Restructures and refines while preserving 100% of original content

### Bilingual Documentation
- `README.md` (English) as primary — this is what GitHub displays by default
- `README.ko.md` (Korean) with identical structure, content, and links
- Language switcher link at the top of both files
- Technical terms preserved in both versions

### PDF Generation
- Converts README content to a professional PDF document after creation
- Designed for non-developers who prefer offline or printable documentation
- Includes all sections, tables, and code blocks in readable format
- Delivered alongside the ZIP

### GitHub Description
- Generates a one-line project summary for the GitHub repository Description field
- Both English and Korean versions
- Follows best practices: under 80 characters, includes core value proposition

### Environment Documentation
- Extracts `.env` keys and generates `.env.example` with placeholders and usage comments
- Adds API key acquisition URLs where identifiable
- Falls back to scanning source code for `process.env` / `os.environ` patterns

### Intelligent .gitignore
- Framework-specific templates (Node.js, Python, Flutter, Rust, Go, Java, C#, etc.)
- Never removes existing entries — only adds missing ones
- Auto-detects runtime data folders (`data/`, `logs/`) and adds proper exclusions + `.gitkeep`

### Complete ZIP Packaging
- Copies original source files safely using `rsync --exclude` (not naive `cp -r`)
- Includes all new/improved files — user gets one ZIP ready for GitHub
- Excludes everything in `.gitignore` (node_modules, .env, dist, etc.)
- Smart lock file handling: includes `package-lock.json` for apps, optional for libraries

### Self-Verification (4-Step)
- Runs automated 10-point checklist after ZIP creation
- Verifies: all source files present, no sensitive files leaked, correct LICENSE year, clean structure
- Auto-fixes any issues found before delivery

### Update & Release Support
- Detects whether this is an initial upload or an update to an existing repository
- For updates: focuses on changed files, proposes CHANGELOG entries, provides change summary
- **Semantic Versioning guide**: Explains MAJOR.MINOR.PATCH rules and suggests appropriate version numbers
- **CHANGELOG generation**: Follows [Keep a Changelog](https://keepachangelog.com) format with proper categorization (Added, Changed, Fixed, etc.)
- **Release notes**: Generates user-friendly release notes ready to paste into GitHub Releases page
- **GitHub Releases guide**: Step-by-step instructions for creating releases via Web, Desktop, or CLI
- Preserves existing good documentation while improving where needed

## 🛠️ Tech Stack

| Category | Technology |
|---|---|
| Platform | Claude Skills (Claude.ai / Claude Code) |
| Tools | `bash_tool`, `create_file`, `str_replace`, `view`, `present_files` |
| Safe Copy | `rsync` with `--exclude` patterns |
| PDF Generation | Python `reportlab` library |
| Verification | `unzip -l`, `grep` validation |

## 🌐 Supported Languages

| Language | Support |
|---|---|
| Korean (한국어) | ✅ All guidance delivered in Korean |
| English | ✅ README.md generated in English as primary |
| Technical Terms | Preserved in original language with Korean explanations |

## 📦 Prerequisites

- Claude Pro, Team, or Enterprise subscription
- A project you want to upload to GitHub
- Claude.ai or Claude Code environment

## 🚀 Installation

### Claude.ai
1. Download the `github-upload` folder (or the ZIP)
2. Open your Claude Project settings
3. Navigate to the Skills section
4. Upload the `github-upload` folder contents

### Claude Code
1. Download the `github-upload` folder
2. Place it in your skills directory:
   ```bash
   cp -r github-upload/ ~/.claude/skills/github-upload/
   ```

## 💻 Usage

Ask Claude in natural language. The skill triggers automatically when you mention GitHub, README, .gitignore, LICENSE, or repository-related keywords.

### Initial Upload
```
이 프로젝트 깃허브에 올릴 수 있게 정리해줘
```
```
Prepare this project for GitHub upload
```

### Specific Tasks
```
README 만들어줘                    # Create README
기존 README 좀 다듬어줘           # Polish existing README
README 영문 번역해줘              # Translate README to English
.gitignore 만들어줘               # Create .gitignore
LICENSE 추가해줘                  # Add LICENSE
Description 만들어줘              # Generate GitHub Description
```

### Updates & Releases
```
업데이트 내용 정리해줘            # Prepare update
릴리즈 준비해줘                   # Prepare release
변경사항 정리해줘                 # Organize changes
```

## ⚙️ How It Works

The skill follows a 6-step pipeline:

```
[1] Deep Analysis
    Project type detection → Source code reading → File audit → Mode detection
    ↓
[2] File Generation
    .gitignore → .env.example → README.md (EN) + README.ko.md (KO) → LICENSE
    ↓
[3] ZIP Packaging
    Safe copy (rsync --exclude) → Overlay new files → Create ZIP
    ↓
[4] Self-Verification
    10-point checklist → Auto-fix any issues → Re-package if needed
    ↓
[5] PDF Generation
    Convert README to professional PDF for non-developers
    ↓
[6] Delivery
    ZIP + PDF + GitHub Description + Upload instructions
```

Each step has built-in safeguards: rsync prevents accidental inclusion of node_modules, the 10-point checklist catches missing files or leaked secrets, and LICENSE year is always validated against the current date.

## 📁 Project Structure

```
github-upload/
├── SKILL.md          # Skill definition — instructions for Claude
├── README.md         # English documentation (this file)
├── README.ko.md      # Korean documentation (identical content)
└── LICENSE           # MIT License
```

## 🎯 Trigger Keywords

| Korean | English |
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

## 🔧 Troubleshooting

| Symptom | Cause | Solution |
|---|---|---|
| ZIP missing source files | Only new files were packaged | Re-run — skill always includes originals |
| ZIP is hundreds of MB | node_modules or build artifacts included | Check rsync exclude list matches .gitignore |
| Wrong LICENSE year | Copied year from package.json | Skill always uses current date — re-generate |
| README has wrong commands | Scripts not verified against package.json | Skill only uses confirmed scripts |
| PDF not generated | Step skipped or reportlab not installed | Ask Claude to generate PDF separately |
| Korean README out of sync | Edited English version only | Re-run translation to sync both files |

## 🔒 Security

- **Sensitive file scanning**: The skill automatically identifies `.env`, `*.pem`, `credentials.json`, SSH keys, and other dangerous files
- **Exclusion verification**: All sensitive files are verified to be in `.gitignore` before packaging
- **No secrets in output**: `.env.example` only contains placeholder values, never real credentials
- **ZIP safety**: The self-verification step confirms no sensitive files are included in the final ZIP
- **Source code privacy**: All analysis happens within your Claude session — no data is sent externally

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Contribution Ideas
- Add support for more project types (Swift, Kotlin Multiplatform, etc.)
- Improve PDF formatting and styling
- Add more language support beyond Korean/English

## ❓ FAQ

**Q: Does this skill run git commands?**
A: No. This skill prepares and polishes your project files for GitHub upload. It generates/improves documentation, packages everything into a ZIP, and provides upload instructions. You do the actual uploading.

**Q: Will it modify my original source code?**
A: Never. The skill only creates or improves documentation files (README, .gitignore, LICENSE, etc.). Your source code is included in the ZIP exactly as-is.

**Q: Can I use it for updates, not just initial uploads?**
A: Yes. The skill detects whether this is an initial upload or an update and adjusts its workflow accordingly — focusing on changed files and offering CHANGELOG updates.

**Q: Why is README.md in English?**
A: GitHub displays README.md as the default project page. English maximizes global reach. The Korean version (README.ko.md) is always generated with identical content and linked at the top.

**Q: What about the PDF?**
A: A PDF version of the README is automatically generated for sharing with non-developers or for offline reference. It contains the same content in a printable format.

## 🙏 Acknowledgments

- Built for the [Vibe Coding](https://github.com) community
- Designed to make GitHub accessible for non-developers
- Inspired by the best practices of open-source documentation

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
