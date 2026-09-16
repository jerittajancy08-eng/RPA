# Indication Fallback Workflow

Manual Object Repository registration for unsupported activities ([uia-configure-target-guide.md § Unsupported Activities](uia-configure-target-guide.md#unsupported-activities)) or targets that bounded automated advancement/capture cannot expose. Both commands require user to click target.

Post-interaction visibility alone is not indication criterion. If `uip rpa uia interact` can expose target, finish current-state OR registration, advance, then rerun `uia-configure-target` for new state.

## Before indicating

Indication is **screen-blocking and user-driven**: CLI overlays screen until user clicks or cancels. Before either command:

1. **Describe target:** application, screen, specific element/control/window. Never assume it is visible.
2. **Wait for readiness acknowledgment** via harness confirmation (e.g., `AskUserQuestion`). Never invoke speculatively: overlay captures current screen while user may still navigate.
3. **No short shell timeout.** User action routinely takes minutes; short timeout kills command prematurely.

## Workflow

Indicate screen first (creates App if absent), then use its response `Data.reference` as element `--parent-id`. Confirm readiness before each command.

```bash
uip rpa indicate-application \
  --name "<ScreenName>" \
  --description "<ScreenDescription>" \
  --project-dir "<PROJECT_DIR>" \
  --output json

uip rpa indicate-element \
  --name "<ElementName>" \
  --activity-class-name "<TypeInto|Click|GetText|...>" \
  --parent-id "<screen-reference>" \
  --project-dir "<PROJECT_DIR>" \
  --output json
```

Both commands return:

```json
{ "Data": { "reference": "..." } }
```

Cancel, nonzero, or missing `Data.reference` -> stop; never invoke dependent element indication. Preserve already-created screen when later element indication stops.

Use reference for Object Repository lookups and target attachment.

## After indication

Then:

- **Coded:** `ObjectRepository.cs` regeneration requires Studio Desktop open on project plus at least one `[Workflow]`/`[TestCase]` `.cs`; save/reopen to trigger. Pure CLI cannot generate it. If missing/stale, confirm entries with `uip rpa object-repository get --project-dir "<PROJECT_DIR>" --output json`; `Descriptors.*` still requires generated file.
- **XAML:** attach each reference per `uia-target-attachment-guide.md` in this `references/` folder.

## Pitfalls

- **Never pass App display name to `--parent-name`** (e.g., `"Acme"`): it matches AppVersion names (e.g., `"1.0.0"`), not App names. Pass AppVersion reference via `--parent-id`.
- **Never pass App `_reference` from `ObjectRepository.cs` to `--parent-id`:** it is App, not AppVersion, reference. Read AppVersion reference from `.objects/` metadata.

## Full parameter reference

All flags, troubleshooting, examples: sibling `cli-reference.md` § Indicate.
