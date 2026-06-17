# AGENTS.md

This file provides guidance for AI agents working on this repository.

## Development Commands

```bash
npm run build          # Build extension with esbuild
npm run watch          # Watch mode for development (auto-rebuild on changes)
npm run lint           # Run ESLint on TypeScript files
npm run test           # Run all unit tests
npm run package        # Package as VSIX (vsce package)
```

### Running Single Tests

Tests use Mocha. To run a specific test suite:
```bash
npm run pretest  # Build first if needed
# Then run specific test file with Mocha
npx mocha out/test/suite/extension.test.js --grep "test name pattern"
```

To run tests in VS Code: use the "Testing" view or press F5 to launch Extension Development Host.

## Code Style Guidelines

### Imports and Structure

- External imports first (vscode, fs, path, etc.), then local imports
- Group local imports by relative path (no circular dependencies)
- Example:
  ```typescript
  import * as vscode from 'vscode';
  import * as fs from 'fs';
  import * as path from 'path';
  import { ChangelistService } from './ChangelistService';
  import { getConfig, log } from './utils';
  ```

### TypeScript Configuration

- **Target**: ES2022
- **Module**: CommonJS
- **Strict mode**: Enabled
- **Always use explicit types** for function parameters and return values
- Avoid `any` - use `unknown` or specific types

### Naming Conventions

- **Classes/Interfaces**: PascalCase (`ChangelistService`, `GitRepository`)
- **Functions/Methods**: camelCase (`getChangelist`, `shelveFile`)
- **Variables/Properties**: camelCase (`changelistId`, `repoPath`)
- **Constants**: UPPER_SNAKE_CASE (`STATE_VERSION`, `SNAPSHOTS_DIR`)
- **Private members**: Underscore prefix (`_onDidChange`, `_state`)
- **Event emitters**: `private readonly _onEventName = new vscode.EventEmitter<T>()`

### Formatting and Syntax

- **Semicolons**: Always required (enforced by ESLint)
- **Quotes**: Double quotes for strings
- **Indentation**: 4 spaces (no tabs)
- **Braces**: Required for all control structures (curly rule in ESLint)
- **Equality**: Always use `===` (eqeqeq rule in ESLint)

### Error Handling

- **User-facing errors**: Use `showError()`, `showWarning()`, `showInfo()` from utils
- **Internal logging**: Use `log()` with level ('info', 'warn', 'error')
- **Never use throw literal**: Always `throw new Error('message')`
- **Try/catch**: All async file operations, git commands, and user interactions
- Example:
  ```typescript
  try {
    await service.shelveFile(path, changelistId);
    showInfo(`Shelved: ${path}`);
  } catch (error) {
    log(`Failed to shelve: ${error}`, 'error');
    showError(`Shelve failed: ${error instanceof Error ? error.message : String(error)}`);
  }
  ```

### File Organization

- `extension.ts` - Entry point, command registration
- `types.ts` - All TypeScript interfaces and type definitions
- `utils.ts` - Pure utility functions (no side effects)
- `ChangelistService.ts` - Core business logic, state management
- `ChangelistTreeProvider.ts` - Tree view UI
- `GitContentProvider.ts` - Custom URI handlers for git/snapshot content
- `RepositoryManager.ts` - Multi-repo management

### State Management

- Use `vscode.workspaceState` for persistence
- State versioning: bump `STATE_VERSION` when schema changes
- Always maintain backward compatibility with migrations
- Repo-specific keys via `getRepoStateKey(repoPath)`

### Event Emitters

- Use `vscode.EventEmitter<T>` for custom events
- Expose as `public readonly onEvent: Event<T> = this._onEvent.event`
- Fire events after state changes: `this._onDidChangeChangelists.fire()`

### User Interactions

- **Input**: Use `promptInput()` from utils
- **Selection**: Use `promptSelect()` for QuickPick
- **Confirmation**: Use `promptConfirm()` for yes/no dialogs
- All user-facing messages prefixed with "Smart Changelists: " automatically

### Git Operations

- Use `simple-git` library for all git commands
- Always check `if (!this.git)` before git operations
- Normalize paths: use `normalizePath()` to convert backslashes to forward slashes
- Repo paths: use absolute paths internally, store relative in state

### Key Patterns

**Service pattern**: Each git repository gets its own `ChangelistService` instance
**Multi-repo support**: All state objects include `repoPath` property
**File snapshots**: Store FULL content (not diffs) to prevent corruption
**Working files**: Never modify working directory without user action

### Testing

- Test files in `src/test/suite/`
- Use `assert` module for assertions
- Test structure: `suite('Name', () => { test('description', () => { ... }); })`
- Pre-requisite: `npm run pretest` builds the extension before testing

### Adding New Features

1. Add command to `package.json` → `contributes.commands`
2. Add to menus if needed → `contributes.menus`
3. Register in `extension.ts` → `registerCommands()`
4. Implement handler function
5. Add TypeScript interface to `types.ts` if needed
6. Add UI helpers to `utils.ts` if reusable

### Critical Constraints

- **NO automatic git commits/push/tag** - Show commands as output only
- **NO automatic version changes** - Version changes require explicit user approval
- **NO automatic publish** - User handles `vsce publish` and release tags
- Working files must remain intact after creating snapshots (do NOT revert files)

### Dependencies

- `vscode` - VS Code Extension API
- `simple-git` - Git operations
- No UI frameworks - uses native VS Code API only
