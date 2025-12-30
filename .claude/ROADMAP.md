# Roadmap & Future Tasks

## Completed Features

### v1.0.0
- [x] Snapshot-based changelist system
- [x] Full file content storage
- [x] Multiple changelists support
- [x] Activity Bar panel
- [x] Diff preview
- [x] Export/Import changelists
- [x] Keyboard shortcuts

### v1.1.0
- [x] Version Comparison (experimental)
- [x] AI/CLI Integration (Add to Chat)
- [x] File-based snapshots option
- [x] Badge counter
- [x] Smart changelist naming

### v1.1.1
- [x] Rebrand to "Smart Changelists"
- [x] Update all internal references

### v1.1.2
- [x] New icon design
- [x] Separate Activity Bar / Marketplace icons

### v1.1.3
- [x] GitHub Actions CI/CD
- [x] Unit test infrastructure
- [x] Multi-platform testing
- [x] Auto release on tag

---

## Pending / Future

### High Priority
- [ ] What's New notification on update
- [ ] Dependabot for dependency updates
- [ ] Issue templates (bug, feature)

### Medium Priority
- [ ] WebView for version comparison (Phase 3 of plan)
- [ ] Merge wizard for multiple versions
- [ ] Localization (multi-language)

### Low Priority
- [ ] Telemetry (usage analytics)
- [ ] Contributing guide
- [ ] Sponsor integration

---

## Version Comparison Plan

Reference: `C:\Users\User\.claude\plans\structured-giggling-cherny.md`

### Phase 1: Foundation (DONE)
- [x] FileVersion type
- [x] enableVersionComparison config
- [x] getFileVersions() method
- [x] compareWith command
- [x] Context menu (config-controlled)

### Phase 2: Quick Compare (DONE)
- [x] QuickPick version list
- [x] HEAD, Working, snapshots
- [x] Diff between two versions

### Phase 3: WebView Report (TODO)
- [ ] ComparisonWebView class
- [ ] HTML/CSS/JS template
- [ ] Diff visualization
- [ ] Quick action buttons

### Phase 4: Merge Wizard (TODO)
- [ ] Selection UI per line
- [ ] Merge logic
- [ ] Apply to working
- [ ] Create new snapshot option

---

## What's New Feature

To implement update notification:

```typescript
// src/whatsnew.ts
export async function showWhatsNew(context: vscode.ExtensionContext) {
    const currentVersion = context.extension.packageJSON.version;
    const previousVersion = context.globalState.get<string>('version');

    if (previousVersion !== currentVersion) {
        context.globalState.update('version', currentVersion);

        const action = await vscode.window.showInformationMessage(
            `Smart Changelists v${currentVersion}`,
            'What\'s New',
            'Dismiss'
        );

        if (action === 'What\'s New') {
            vscode.env.openExternal(vscode.Uri.parse(
                'https://github.com/harungecit/smart-changelists/blob/main/CHANGELOG.md'
            ));
        }
    }
}
```

Call from `activate()` in extension.ts.

---

## Notes

- Keep version at current until features finalized
- Test locally before pushing tags
- CI must pass before release
- Update CHANGELOG.md for each version
