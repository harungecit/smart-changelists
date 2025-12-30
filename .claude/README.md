# .claude Directory

This directory contains reference documentation for Claude AI assistant.

## Files

| File | Purpose |
|------|---------|
| `RULES.md` | **ÖNCE OKU** - AI agent kuralları, yasaklar, izinler |
| `PROJECT.md` | Project overview, architecture, commands, config |
| `DEVELOPMENT.md` | Local development guide, structure, common issues |
| `WORKFLOWS.md` | GitHub Actions CI/CD documentation |
| `ROADMAP.md` | Completed features, pending tasks, future plans |
| `HISTORY.md` | Session history, decisions made, learnings |

## Öncelik Sırası

1. **RULES.md** - Her oturumda ilk okunmalı
2. PROJECT.md - Proje yapısını anlamak için
3. Diğerleri - Gerektiğinde

## Usage

When starting a new session, Claude should read these files to understand:
1. Project structure and architecture
2. Current version and state
3. Pending tasks and roadmap
4. Past decisions and learnings
5. User preferences

## Updating

After significant changes, update relevant files:
- New feature → `ROADMAP.md` (mark complete), `PROJECT.md` (if architecture changed)
- Bug fix or decision → `HISTORY.md`
- Workflow change → `WORKFLOWS.md`
- Dev process change → `DEVELOPMENT.md`

## Note

This directory is for AI reference only and should be added to `.gitignore` if not meant to be committed. Currently it IS committed as part of the project documentation.
