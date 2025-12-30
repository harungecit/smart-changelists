# Session History & Decisions

## Rebranding (2025-12-11)

### Issue
Original name "git-changelists" was taken on VS Code Marketplace.

### Decision
Renamed to "smart-changelists" with display name "Smart Changelists".

### Changes Made
- All `gitChangelists` → `smartChangelists` in code
- All `gitChangelist` → `smartChangelist` in code
- `.gitchangelists/` → `.smartchangelists/` folder
- package.json name and displayName updated

---

## Icon Issues (2025-12-12)

### Problem 1: Activity Bar icon format
- Error: "property icon is mandatory and must be of type string"
- Cause: Used object format `{"light": "...", "dark": "..."}` for Activity Bar
- Solution: Activity Bar icon must be single string path

### Problem 2: SVG too complex
- 512x512 viewBox SVGs didn't render at 24x24 Activity Bar size
- Solution: Use PNG icons instead

### Problem 3: VS Code caching
- Old icons persisted after updates
- Solution: Uninstall extension, clear cache, reinstall

### Final Icon Setup
- `icon.png` - Marketplace (detailed)
- `icon-white.png` - Activity Bar (white on transparent, monochrome)
- `icon-black.png` - Alternative variant

---

## CI/CD Setup (2025-12-13)

### Requirements
- User wanted Node 22 (local version) for workflows
- Tests must not depend on extension runtime activation

### Test Fix
Original tests tried to check runtime command registration, but:
- Extension only activates with `.git` folder
- CI may not have proper git workspace

Solution: Tests now check `package.json` structure instead of runtime.

### Workflow Setup
- `ci.yml` - Runs on push/PR, tests on 3 OS
- `release.yml` - Runs on tag push, publishes to Marketplace

---

## Key Learnings

### VS Code Extension Icons
- Marketplace icon: PNG, any size (128x128+ recommended)
- Activity Bar icon: PNG or SVG, monochrome, 24x24 effective
- Define in different places:
  - Marketplace: `package.json` → `icon`
  - Activity Bar: `package.json` → `contributes.viewsContainers.activitybar[].icon`

### Testing
- VS Code extension tests run in actual VS Code instance
- Need xvfb for headless CI (Linux)
- Extension activation depends on workspace conditions
- Prefer checking package.json structure over runtime behavior

### Release Process
- Tag push triggers release
- Must have `VSCE_PAT` secret configured
- If tag exists, delete and recreate to re-trigger

---

## User Preferences

- Don't increment version unnecessarily
- Don't run git commands directly - provide commands for user to run
- Keep things simple and professional
- Use Node 22 for CI (matches local environment)
