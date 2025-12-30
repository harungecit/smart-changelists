# Smart Changelists - Project Reference

## Overview
VS Code extension providing JetBrains-style changelists. Save snapshots of file changes, switch between versions, and commit selectively.

## Key Information

| Field | Value |
|-------|-------|
| Name | smart-changelists |
| Publisher | harungecit |
| Current Version | 1.1.3 |
| License | Apache-2.0 |
| VS Code Engine | ^1.85.0 |
| Node.js | 22 |

## Repository
- GitHub: https://github.com/harungecit/smart-changelists
- Marketplace: https://marketplace.visualstudio.com/items?itemName=harungecit.smart-changelists

## Author
- Name: Harun Geçit
- Email: info@harungecit.com
- Web: https://harungecit.dev

---

## Architecture

### Core Components

| File | Purpose |
|------|---------|
| `src/extension.ts` | Entry point, command registration |
| `src/ChangelistService.ts` | Core business logic, state management |
| `src/ChangelistTreeProvider.ts` | Tree view UI provider |
| `src/ChangelistProvider.ts` | Legacy provider (may be deprecated) |
| `src/GitContentProvider.ts` | Git content and diff URIs |
| `src/types.ts` | TypeScript interfaces |
| `src/utils.ts` | Helper functions, logging, config |

### State Management
- State stored in VS Code's `ExtensionContext.workspaceState`
- State version: 3
- Snapshots store **full file content** (not diffs)
- Persistent across sessions

### Activation
- Triggers on: `workspaceContains:.git`
- Sets context: `smartChangelists.enabled`

---

## Commands

| Command ID | Title |
|-----------|-------|
| `smartChangelists.createChangelist` | Create Changelist |
| `smartChangelists.deleteChangelist` | Delete Changelist |
| `smartChangelists.renameChangelist` | Rename Changelist |
| `smartChangelists.shelveFile` | Shelve to Changelist... |
| `smartChangelists.unshelveFile` | Restore to Working |
| `smartChangelists.unshelveAll` | Restore All to Working |
| `smartChangelists.applyAndStage` | Apply & Stage |
| `smartChangelists.applyAllAndStage` | Apply All & Stage |
| `smartChangelists.deleteShelvedFile` | Delete Snapshot |
| `smartChangelists.commitChangelist` | Commit Shelved Changelist |
| `smartChangelists.commitWorkingChanges` | Commit Working Changes |
| `smartChangelists.refreshAll` | Refresh |
| `smartChangelists.exportChangelists` | Export Changelists |
| `smartChangelists.importChangelists` | Import Changelists |
| `smartChangelists.openFile` | Open File |
| `smartChangelists.openDiff` | Open Diff |
| `smartChangelists.previewShelved` | Preview Shelved Changes |
| `smartChangelists.revertFile` | Revert Changes |
| `smartChangelists.setActiveChangelist` | Set as Active Changelist |
| `smartChangelists.moveToChangelist` | Shelve to Changelist... |
| `smartChangelists.addToChat` | Add to Chat |
| `smartChangelists.addWorkingFileToChat` | Add to Chat |
| `smartChangelists.compareWith` | Compare with... |
| `smartChangelists.compareAllVersions` | Compare All Versions |

---

## Configuration

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `showEmptyChangelists` | boolean | true | Show changelists with no files |
| `autoRefreshOnSave` | boolean | true | Auto-refresh when files are saved |
| `confirmBeforeCommit` | boolean | true | Confirmation before commit |
| `confirmBeforeRevert` | boolean | true | Confirmation before revert |
| `saveSnapshotsToFile` | boolean | false | Save to `.smartchangelists/` for CLI tools |
| `enableVersionComparison` | boolean | false | Enable version comparison (experimental) |

---

## Keyboard Shortcuts

| Shortcut | Command |
|----------|---------|
| `Ctrl+Shift+N` / `Cmd+Shift+N` | Create Changelist |
| `Ctrl+Shift+M` / `Cmd+Shift+M` | Shelve to Changelist |

---

## Icons

| File | Usage |
|------|-------|
| `resources/icon.png` | Marketplace icon |
| `resources/icon-black.png` | Alternative (black bg) |
| `resources/icon-white.png` | Activity Bar icon |

---

## Dependencies

### Runtime
- `simple-git` - Git operations

### Development
- `@vscode/test-electron` - Testing
- `@vscode/vsce` - Packaging
- `esbuild` - Bundling
- `eslint` + `@typescript-eslint/*` - Linting
- `mocha` - Test framework
- `typescript` - Language

---

## Build & Package

```bash
# Install dependencies
npm ci

# Build
npm run build

# Lint
npm run lint

# Test
npm test

# Package
npx vsce package
```

Output: `smart-changelists-{version}.vsix`
