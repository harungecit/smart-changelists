# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Quick Reference

**Read `.claude/RULES.md` first** - Contains important restrictions for AI agents.

Detailed documentation is in the `.claude/` folder:
- `RULES.md` - AI agent rules, git restrictions, version change approval process
- `PROJECT.md` - Project overview, commands reference, configuration options
- `DEVELOPMENT.md` - Local development setup, project structure, adding features
- `WORKFLOWS.md` - GitHub Actions CI/CD documentation
- `ROADMAP.md` - Completed and pending features
- `HISTORY.md` - Session history and decisions

## Project Overview

Smart Changelists is a VS Code extension that brings JetBrains-style changelists to VS Code. Save snapshots of file changes without reverting working files, organize them into named changesets, commit selectively.

## Development Commands

```bash
npm run build       # Build extension with esbuild
npm run watch       # Watch mode for development
npm run lint        # Run ESLint on TypeScript files
npm run test        # Run unit tests (requires pretest build)
npm run package     # Package as VSIX (npx vsce package)
```

## Architecture

| File | Purpose |
|------|---------|
| `src/extension.ts` | Entry point, command registration |
| `src/ChangelistService.ts` | Core business logic, state management via workspaceState |
| `src/ChangelistTreeProvider.ts` | Tree view UI for Activity Bar panel |
| `src/GitContentProvider.ts` | Git content and diff URI handlers |
| `src/types.ts` | TypeScript interfaces (`Changelist`, `ShelvedFile`, etc.) |
| `src/utils.ts` | Helpers: logging, config, path handling, UI dialogs |

**Key design**: Stores **full file content** (not diffs) to prevent corruption. Working files remain intact after creating snapshots.

## Critical Rules

From `.claude/RULES.md`:

- **NO automatic git commits/push/tag** - Show commands as output for user to run manually
- **NO automatic version changes** - Version changes require explicit user approval
- **NO automatic publish** - User handles `vsce publish` and release tags

## Adding Features

**New command**: Add to `package.json` contributes.commands → Add to menus if needed → Register in `extension.ts` registerCommands() → Implement handler

**New config**: Add to `package.json` contributes.configuration.properties → Add type to `types.ts` ChangelistConfig → Use via `getConfig().propertyName`
