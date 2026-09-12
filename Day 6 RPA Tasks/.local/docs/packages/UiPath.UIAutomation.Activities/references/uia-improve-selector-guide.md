# Improve Selector Guide

Caller-facing invocation for `uia-improve-selector` — agent-invoked (not via CLI); the internal procedure the subagent executes is [SKILL.md](../skills/uia-improve-selector/SKILL.md). Each recipe shows (1) CLI commands caller runs to stage folder, (2) skill invocation arguments, (3) CLI command writing improved result back to source.

## Modes

Pass `--mode <recover|improve>` to specify fix kind:

- **`recover`** — selector *broken* (element not found, selector stopped working, runtime failure). Assumes original selector no longer matches; rewrites against current DOM. For failure-triggered fixes.
- **`improve`** — selector still *works* but fragile (positional, auto-generated IDs, content-reflecting attributes, etc.). Produces more robust rewrite still targeting same element.

`--mode` omitted → skill infers from invocation phrasing ("fix", "stopped working", "element not found" → `recover`; "improve", "robust", "harden" → `improve`). Pass explicitly for non-interactive callers with no natural-language prompt.

## Form 1 — XAML activity (recommended when you have activity + workflow)

**Stage:**
```bash
uip rpa uia target-anchorable get-definition \
  --definition-file-path "$FOLDER/target.xaml" \
  --activity-id "$ACT_REF" \
  --workflow-file-path "$XAML_REL_PATH"
```

**Invoke the skill with:**
```
"$FOLDER/target.xaml"
--folder "$FOLDER"
--mode recover
```

**Write back:**
```bash
uip rpa uia target-anchorable link \
  --targets "[{\"workflowFilePath\":\"$XAML_REL_PATH\",\"activityId\":\"$ACT_REF\",\"definitionFilePath\":\"$FOLDER/target.xaml\"}]"
```

## Form 2 — Object Repository reference (recommended when you have an OR ref)

**Stage:**
```bash
uip rpa uia object-repository get-element-definition \
  --definition-file-path "$FOLDER/target.xaml" \
  --reference-id "$OR_REF"
```

**Invoke the skill with:**
```
"$FOLDER/target.xaml"
--folder "$FOLDER"
--mode recover
```

**Write back:** `object-repository update-element` (when available).

**Prefer Form 2 over Form 1 when a ref exists.** `get-definition` commands pull `ActivityType` from source automatically, so improve run uses accurate rules for that activity (e.g., `GetText` avoids content-reflecting attributes, `Check` avoids state attributes). Form 1 requires explicit `--activity-type` -- defaults to `Click` if omitted, producing suboptimal selectors for other activity types.

## Preparing the folder

Skill runs `snapshot capture` automatically when tree files missing; pre-stage only for offline or recover scenarios. Table covers every situation — pick one matching your state:

**IMPORTANT**: `WindowSelector` refers to `SelectorArgument` for windows/screens and `ScopeSelectorArgument` for elements.

| Mode | Situation | Pre-stage in `$FOLDER` | Def file `WindowSelector` | Capture |
|---|---|---|---|---|
| **(a) Offline, full runtime data** | App not reachable; runtime data shipped from another machine | `ApplicationLevelNodeTreeInfo.json` (+ screenshot, metadata) | as-received | skip |
| **(b.1) Failure dump available** | Runtime dumped trees at failure moment | `ApplicationLevelNodeTreeInfo.json` from the dump | as-received | skip |
| **(b.2) Live, window selector works** | Element search failed; window itself still findable | nothing | keep valid `WindowSelector` | `snapshot capture` → produces app-level tree |
| **(c) Live, window selector broken** | Window itself can't be found at runtime | nothing | leave as-is (even broken) | `snapshot capture` → writes top-level tree first, then fails finding the app; top-level tree on disk is what's needed |

**Mode (c) note:** `snapshot capture` with broken `WindowSelector` returns non-zero exit and stderr like "Could not find application window". Expected — before failing, writes `TopLevelNodeTreeInfo.json` to `$FOLDER`. Skill picks it up and recovers window selector. Pre-staging top-level tree yourself skips capture entirely.
