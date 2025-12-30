# Development Guide

## Local Development

### Setup
```bash
npm install
```

### Build
```bash
npm run build          # Production build
npm run watch          # Watch mode
```

### Test
```bash
npm run pretest        # Build + compile tests
npm test               # Run tests (requires display/xvfb)
```

### Lint
```bash
npm run lint
```

### Package
```bash
npx vsce package                    # Creates .vsix
npx vsce package --no-git-tag-version  # Without git tag check
```

---

## Testing Extension Locally

### Install from VSIX
```bash
code --install-extension smart-changelists-x.x.x.vsix --force
```

### Uninstall
```bash
code --uninstall-extension harungecit.smart-changelists
```

### Reload VS Code
- Command Palette → "Developer: Reload Window"

---

## Project Structure

```
smart-changelists/
├── .claude/                 # Claude reference docs (this)
├── .github/
│   └── workflows/
│       ├── ci.yml           # CI workflow
│       └── release.yml      # Release workflow
├── dist/                    # Build output (gitignored)
├── out/                     # Test build output (gitignored)
├── resources/
│   ├── icon.png             # Marketplace icon
│   ├── icon-black.png       # Black variant
│   └── icon-white.png       # Activity Bar icon
├── src/
│   ├── test/
│   │   ├── runTest.ts       # Test runner
│   │   └── suite/
│   │       ├── index.ts     # Mocha loader
│   │       └── extension.test.ts  # Tests
│   ├── ChangelistService.ts
│   ├── ChangelistTreeProvider.ts
│   ├── ChangelistProvider.ts
│   ├── GitContentProvider.ts
│   ├── extension.ts
│   ├── types.ts
│   └── utils.ts
├── .eslintrc.json
├── .gitignore
├── .vscodeignore
├── CHANGELOG.md
├── esbuild.config.js
├── LICENSE
├── NOTICE
├── package.json
├── README.md
├── tsconfig.json
└── tsconfig.test.json
```

---

## Key Files

### `.vscodeignore`
Excludes from VSIX package:
- `.vscode/`, `.vscode-test/`
- `src/`, `out/`
- `node_modules/`
- `.github/`
- `*.ts`, `*.map`
- `tsconfig*.json`
- `.gitchangelists/`, `.smartchangelists/`

### `esbuild.config.js`
- Bundles extension to single `dist/extension.js`
- External: `vscode`
- Platform: node
- Format: cjs

### `tsconfig.json`
- Target: ES2022
- Module: commonjs
- outDir: dist
- rootDir: src

### `tsconfig.test.json`
- Same as main but:
- outDir: out
- include: src/test/**/*

---

## Adding New Features

### New Command
1. Add to `package.json` → `contributes.commands`
2. Add menu entry in `contributes.menus` if needed
3. Register in `src/extension.ts` → `registerCommands()`
4. Implement handler function

### New Configuration
1. Add to `package.json` → `contributes.configuration.properties`
2. Add type in `src/types.ts` → `ChangelistConfig`
3. Update `src/utils.ts` → `getConfig()` default
4. Use via `getConfig().propertyName`

### New Test
1. Add to `src/test/suite/extension.test.ts`
2. Use `suite()` for grouping
3. Use `test()` for individual tests
4. Tests check package.json structure (not runtime)

---

## Common Issues

### Activity Bar Icon Not Showing
- Icon must be PNG for Activity Bar
- Path: `resources/icon-white.png`
- Defined in: `package.json` → `contributes.viewsContainers.activitybar[].icon`

### Extension Not Activating
- Requires `.git` folder in workspace
- Check: `activationEvents` in package.json
- Debug: Output panel → "Smart Changelists"

### VS Code Caching Old Icons
```bash
# Clear cache
rm -rf "$APPDATA/Code/CachedExtensions"
rm -rf "$HOME/.vscode/extensions/harungecit.smart-changelists-*"
# Reinstall extension
```

### Tests Failing in CI
- Tests should check `package.json` structure
- Don't rely on runtime command registration
- Extension may not activate without `.git`
