🌏 [한국어](README.ko.md) | [English](README.md)

# GitHub Upload Skill for Claude

A Claude Skill that deep-analyzes your project, auto-generates every file needed for a clean GitHub upload, packages **your entire source code plus all new files** into a single verified ZIP, and walks you through uploading it.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## 📋 Overview

You've built something great. Now you want it on GitHub — but the README is rough (or missing), there's no .gitignore, you're not sure your API keys are safe, and you don't know where to start.

This skill handles it end-to-end: analyzes your codebase, generates professional README (English + Korean), framework-specific .gitignore, LICENSE, .env.example, and security checks — then packages **everything** (your original source files + all new/improved files) into one verified ZIP. Download it, upload to GitHub. Done.

## ✨ Features

### Deep Project Analysis
- Auto-detects project type and sub-framework (Node.js, Python, Flutter, React, Next.js, MCP Server, Discord Bot, etc.)
- Reads actual source code to extract accurate features, tech stack, and commands
- Scans for sensitive files (`.env`, API keys, credentials) and verifies they're excluded
- Rates existing README quality and identifies specific improvements needed

### Smart README Generation
- **Create from scratch**: Builds README by analyzing your actual code — zero guessing
- **Polish existing**: Restructures and refines while preserving 100% of original content
- **Type-specific sections**: MCP servers get Configuration section, APIs get Endpoints table, CLI tools get Commands list, Flutter apps get Screenshots placeholder
- **Translate KO↔EN**: Bidirectional translation with proper technical term handling
- **Dual-language**: `README.md` (English) + `README.ko.md` (Korean) with language switcher

### Intelligent .gitignore
- Framework-specific templates (Node.js, Python, Flutter, Rust, Go, Java, C#, etc.)
- Scans existing `.gitignore` for missing entries — never removes existing ones
- Auto-detects runtime data folders (`data/`, `logs/`) and adds proper exclusions + `.gitkeep`

### Environment Documentation
- Extracts `.env` keys and generates `.env.example` with placeholders and usage comments
- Adds API key acquisition URLs where identifiable
- Falls back to scanning source code for `process.env` / `os.environ` patterns

### Complete ZIP Packaging
- Copies original source files safely using `rsync --exclude` (not naive `cp -r`)
- Includes all new/improved files — user gets one ZIP ready for GitHub
- Excludes everything in `.gitignore` (node_modules, .env, dist, etc.)
- Smart lock file handling: includes `package-lock.json` for apps, optional for libraries

### Self-Verification (4-Step)
- Runs automated 10-point checklist after ZIP creation
- Verifies: all source files present, no sensitive files leaked, correct LICENSE year, clean structure
- Auto-fixes any issues found before delivery — user always gets a verified package

### GitHub Upload Guide
- Three methods explained: GitHub Web (easiest), GitHub Desktop (GUI), Git CLI
- Step-by-step instructions for complete beginners

## 🛠️ Tech Stack

| Category | Technology |
|---|---|
| Platform | Claude Skills (Claude.ai / Claude Code) |
| Tools | `bash_tool`, `create_file`, `str_replace`, `view`, `present_files` |
| Safe Copy | `rsync` with `--exclude` patterns |
| Verification | `unzip -l`, `grep` validation |
| Language | All guidance in Korean with English technical terms |

## 🚀 Getting Started

### Prerequisites

- Claude Pro, Team, or Enterprise subscription
- A project you want to upload to GitHub

### Installation

1. Download the `github-upload` folder
2. Place it in your Claude Skills directory:
   - **Claude.ai**: Upload via Skills interface in Project settings
   - **Claude Code**: Place in `~/.claude/skills/github-upload/`

### Usage

Ask Claude in natural language:

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

The skill triggers automatically when you mention GitHub, README, .gitignore, LICENSE, or repository-related keywords.

## 📁 Project Structure

```
github-upload/
├── SKILL.md          # Skill definition (instructions for Claude)
├── README.md         # English documentation (this file)
├── README.ko.md      # Korean documentation
└── LICENSE           # MIT License
```

## 🎯 Trigger Keywords

| Korean | English |
|---|---|
| 깃허브에 올려줘 | Upload to GitHub |
| README 만들어줘 | Create a README |
| README 다듬어줘 | Polish the README |
| README 번역해줘 | Translate the README |
| .gitignore 만들어줘 | Create .gitignore |
| LICENSE 추가해줘 | Add a LICENSE |
| 깃허브 정리해줘 | Prepare for GitHub |

## 📄 License

MIT License. See [LICENSE](LICENSE) for details.
