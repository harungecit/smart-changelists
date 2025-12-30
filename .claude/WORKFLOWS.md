# CI/CD Workflows

## GitHub Actions

### 1. CI Workflow (`.github/workflows/ci.yml`)

**Triggers:**
- Push to `main` or `master`
- Pull requests to `main` or `master`

**Matrix:**
- OS: ubuntu-latest, windows-latest, macos-latest
- Node: 22

**Steps:**
1. Checkout
2. Setup Node.js 22
3. `npm ci`
4. `npm run lint`
5. `npm run build`
6. `npm test` (with xvfb for headless display)
7. `npx vsce package --no-dependencies`
8. Upload VSIX artifact (ubuntu + node 22 only)

---

### 2. Release Workflow (`.github/workflows/release.yml`)

**Triggers:**
- Tag push matching `v*` (e.g., `v1.1.3`)

**Jobs:**

#### Job 1: Test
- Same as CI workflow
- Must pass before release

#### Job 2: Release (depends on test)
1. Checkout
2. Setup Node.js 22
3. `npm ci`
4. `npm run build`
5. `npx vsce package`
6. Create GitHub Release with VSIX
7. Publish to VS Code Marketplace (`VSCE_PAT`)
8. Publish to Open VSX (`OVSX_PAT`) - optional

---

## Required Secrets

| Secret | Required | Source |
|--------|----------|--------|
| `VSCE_PAT` | Yes | Azure DevOps Personal Access Token |
| `OVSX_PAT` | No | Open VSX token |

### Getting VSCE_PAT
1. Go to https://dev.azure.com/
2. User Settings → Personal Access Tokens
3. New Token
4. Organization: "All accessible organizations"
5. Scopes: Marketplace → Manage
6. Copy token
7. Add to GitHub: Repo → Settings → Secrets → Actions → `VSCE_PAT`

---

## Release Process

```bash
# 1. Update version in package.json
# 2. Update CHANGELOG.md
# 3. Commit changes
git add .
git commit -m "v1.x.x: Description" bla bşs3

# 4. Create tag
git tag v1.x.x

# 5. Push
git push origin master
git push origin v1.x.x
```

Tag push triggers release workflow automatically.

---

## Fixing Failed Release

If tag exists but release failed:

```bash
# Delete local tag
git tag -d v1.x.x

# Delete remote tag
git push origin --delete v1.x.x

# Recreate tag
git tag v1.x.x

# Push again
git push origin v1.x.x
```
