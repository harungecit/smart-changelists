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
| `src/extension.ts` | Entry point, command registration, **per-repo service orchestration** |
| `src/RepositoryManager.ts` | Discovers git repos in workspace (multi-root + submodules), watches `.git` |
| `src/ChangelistService.ts` | Core business logic for **one repo**; state via workspaceState |
| `src/ChangelistTreeProvider.ts` | Tree view UI for Activity Bar panel (the active UI) |
| `src/GitContentProvider.ts` | Git/snapshot content + diff URI handlers (`createGitUri`, `createSnapshotUri`) |
| `src/types.ts` | TypeScript interfaces (`Changelist`, `ShelvedFile`, `GitRepository`, etc.) |
| `src/utils.ts` | Helpers: logging, config, repo discovery, path/state-key handling, UI dialogs |
| `src/ChangelistProvider.ts` | **Unused** — legacy SCM (`vscode.scm`) provider, not imported by `extension.ts` |

**Key design**: Stores **full file content** (not diffs) to prevent corruption. Working files remain intact after creating snapshots.

### Multi-repository model (v2.x)

The extension supports multi-root workspaces, nested repos, and submodules. The flow:

- `RepositoryManager` scans workspace folders and emits `onDidChangeRepositories` when repos appear/disappear.
- `extension.ts` keeps a `Map<repoPath, ChangelistService>` — **one service per repo** — and syncs it with the manager. `ChangelistTreeProvider` aggregates all services (repo nodes shown only when >1 repo).
- Each `ChangelistService` persists its state in `workspaceState` under a per-repo key: `smartChangelists.state.{md5(repoPath)}` (see `getRepoStateKey`). `STATE_VERSION` is 4; legacy single-repo key `smartChangelists.state` is migrated.
- Command handlers receive tree-item args carrying `repoPath` (directly or nested in `file`/`shelvedFile`/`changelist`). `getServiceFromArg()` resolves the right service; it falls back to the sole service when only one repo exists, otherwise prompts the user to pick a repo. **When adding a command, route through `getServiceFromArg` rather than assuming a single repo.**

### Snapshots on disk

When `saveSnapshotsToFile` is enabled, shelved content is written to `.smartchangelists/{changelist}/{filename}` (gitignored) so CLI/AI tools can read it. "Add to Chat" otherwise writes a temp copy under `.smartchangelists/.temp/`.

## Critical Rules

From `.claude/RULES.md` (written in Turkish — read it for the full version-change/release process):

- **NO automatic git commits/push/tag** - Show commands as output for user to run manually
- **NO automatic version changes** - `package.json` `version` changes require explicit user approval
- **NO automatic publish** - User handles `vsce publish` and release tags
- Workflow files (`.github/workflows/`) also require approval; `src/`, tests, and docs are free to edit

## Adding Features

**New command**: Add to `package.json` contributes.commands → Add to `menus` (set the `when` clause using the `viewItem` context value: `changelist` / `working-file` / `shelved-file`) → Register in `extension.ts` `registerCommands()` → Implement handler that resolves the repo via `getServiceFromArg(arg)` before acting

**New config**: Add to `package.json` contributes.configuration.properties → Add type to `types.ts` `ChangelistConfig` → Use via `getConfig().propertyName`
