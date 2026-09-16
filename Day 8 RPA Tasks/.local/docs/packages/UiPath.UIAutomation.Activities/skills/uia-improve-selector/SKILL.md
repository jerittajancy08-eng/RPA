---
name: uia-improve-selector
description: "Fix, improve, or recover a UiPath selector using runtime data. **Use when** (1) a selector stopped working and user has runtime data, (2) user asks to fix/improve/recover a selector, (3) user mentions 'element not found' with a snapshot or runtime data folder, (4) user wants to make an existing selector more robust, (5) a selector needs to be fixed after a failure. **Example phrases** 'fix this selector', 'selector stopped working', 'improve the selector', 'element not found, here\\'s the runtime data', 'make this selector more robust', 'recover this selector', 'element not found'"
argument-hint: "<definition-file> --folder <path> [--mode <recover|improve>] [--preserve-window-selector] [--quiet] [--project-dir <path>]"
allowed-tools: Bash, Read, Write, AskUserQuestion
---

Fix or improve UiPath selector using UiAutomation CLI and runtime data.

Operates on definition file (XAML) in `$FOLDER`. Callers pass `$DEF_FILE` directly. Supports full element-mode and window-only modes.

**IMPORTANT: Use forward slashes in ALL paths.** Backslash paths break Read tool.

## CLI

Define shell **function** `cli` wrapping CLI; call as `cli <subcommand> ...` everywhere below:

```bash
cli() { uip rpa uia "$@"; }
```

If `$PROJECT_DIR` set, define with project dir baked in:

```bash
cli() { uip rpa uia --project-dir "$PROJECT_DIR" "$@"; }
```

Every `cli ...` call below then uses right base command and `--project-dir`. Function (not `CLI="..."` string variable) keeps argument boundaries intact when any path -- `--project-dir`, `--folder-path`, or `--definition-file-path` -- contains spaces.

> **WARNING -- never put the command in a string variable. Re-expanding command from string breaks on paths with spaces. Use `cli` function, or write `uip rpa uia ...` in full, every path double-quoted at call site. Re-declare `cli` at top of each Bash invocation calling it (shell state doesn't persist between tool calls).
>
> **Misleading error decode.** If CLI reports `'uia' requires the 'UiPath.UIAutomation.CLI' package`, this is almost always a **bad or truncated `--project-dir`** (commonly project path with spaces, word-split), **not** a real missing-package problem. Verify `--project-dir` arrived as single quoted argument before touching package versions.

## Input Parsing

Extract from `$ARGUMENTS`:

- First positional argument or `--definition` -> `$DEF_FILE`. Path to definition file (XAML) containing `TargetApp` or `TargetAnchorable`.
- `--folder <path>` -> `$WORK_FOLDER`. Folder for snapshot artifacts (tree JSONs, screenshot, instruction files).
- `--mode recover` or `--mode improve` -> `$MODE`. If unspecified, infer from phrasing:
  - **recover** (default): "fix", "broken", "stopped working", "element not found", "recover", "failed"
  - **improve**: "improve", "robust", "optimize", "strengthen", "harden", "resilient"
  - If unclear, default to `recover`.
- `--preserve-window-selector` -> `$PRESERVE_WINDOW_SELECTOR=true` (default: `false`). Keep window selector unchanged; improve/recover only element selector.
- `--quiet` -> `$QUIET=true` (default: `false`). Suppress all output -- write files only. Used when skill runs as sub-step of another skill.
- `--project-dir <path>` -> `$PROJECT_DIR` (optional). UiPath project directory.

No `$WORK_FOLDER` but `$DEF_FILE` given: use `$DEF_FILE`'s directory. No `$DEF_FILE`: ask user for one.

### Refresh snapshot artifacts

Run `snapshot capture` to fill gaps and refresh highlighted screenshot when app is live:

```bash
cli snapshot capture --folder-path "$WORK_FOLDER" --definition-file-path "$DEF_FILE"
```

`snapshot capture` skips tree extractions for existing files, preserving pre-staged offline folders. Live app: fresh `ApplicationScreenshot.png` with target highlighted; unreachable app: no updated screenshot.

## IMPROVE-1: Get Instructions

Generate instruction files via CLI. Writes three files to `$WORK_FOLDER`:
- `selector-system-prompt.md` -- system prompt with rules and constraints
- `selector-user-message.md` -- user message with task, selectors, DOM data
- `selector-schema-config.md` -- JSON schema for expected output

When `$PRESERVE_WINDOW_SELECTOR` true, pass `--preserve-window-selector`:

```bash
PRESERVE_ARG=""
[ "$PRESERVE_WINDOW_SELECTOR" = "true" ] && PRESERVE_ARG="--preserve-window-selector"
cli selector-intelligence get-instructions --folder-path "$WORK_FOLDER" --definition-file-path "$DEF_FILE" --mode "$MODE" $PRESERVE_ARG
```

## IMPROVE-2: Read Instructions and Generate Selectors

**Use ONLY the Read tool and Write tool for this step. Do NOT use Bash, Python, or any other tool to parse, process, or extract content from the instruction files.**

1. Read three instruction files with Read tool (use `offset` and `limit` for large files):
   - `$WORK_FOLDER/selector-system-prompt.md`
   - `$WORK_FOLDER/selector-user-message.md` (can be large due to DOM JSON)
   - `$WORK_FOLDER/selector-schema-config.md`
2. Read `$WORK_FOLDER/ApplicationScreenshot.png` for visual context (skip if missing).
3. Execute task from `selector-user-message.md` following rules from `selector-system-prompt.md`. Write JSON result (per schema in `selector-schema-config.md`) to `$WORK_FOLDER/improve-selector-output.json` with Write tool.

## IMPROVE-3: Evaluate and Retry Loop

Evaluate->fix loop. **Max 3 iterations total.**

Set `$ATTEMPT = 1`.

### Evaluate

```bash
cli selector-intelligence evaluate --folder-path "$WORK_FOLDER" --definition-file-path "$DEF_FILE" --improve-selector-response-file-path "$WORK_FOLDER/improve-selector-output.json" --mode "$MODE" > "$WORK_FOLDER/evaluation-result.txt" 2>&1
```

Read `$WORK_FOLDER/evaluation-result.txt`.

### At least one valid -> done

Pick selector with highest FinalScore. Read top-level `reasoning` field from `$WORK_FOLDER/improve-selector-output.json` for root cause and strategy.

Distinctness penalty on every candidate (any `ToolingFeedback` reporting selector not distinct from similar elements): no rewrite fixes this — target is one of several similar nodes. Apply best candidate. Report penalty verbatim, with colliding-node count when determinable; never present as acceptable — pinning the distinguishing value (parametrization via loop variable, or anchor) is the caller's decision. [selector-variables.md § When to parametrize](../../references/selector-variables.md#when-to-parametrize-per-item-actions-in-a-loop).

Apply winning candidate to `$DEF_FILE` with appropriate `update-definition` CLI command.

From winning candidate, read:
- `$WINDOW_SELECTOR` = candidate's `WindowSelector`.
- `$EDITABLE_PARTIAL_SELECTOR` = candidate's `EditablePartialSelector` (may be absent — leave unset).

**Element definition** (`TargetAnchorable`) — set scope selector to `$WINDOW_SELECTOR`, full selector to `$EDITABLE_PARTIAL_SELECTOR`. **Only pass `--full-selector` when the candidate actually has an `EditablePartialSelector`** — passing `--full-selector ""` overwrites existing full selector with empty value.

When candidate has `EditablePartialSelector`:

```bash
cli target-anchorable update-definition \
  --definition-file-path "$DEF_FILE" \
  --scope-selector "$WINDOW_SELECTOR" \
  --full-selector "$EDITABLE_PARTIAL_SELECTOR"
```

When candidate has no `EditablePartialSelector`, omit option entirely:

```bash
cli target-anchorable update-definition \
  --definition-file-path "$DEF_FILE" \
  --scope-selector "$WINDOW_SELECTOR"
```

**Window definition** (`TargetApp`) — set selector to `$WINDOW_SELECTOR`:

```bash
cli target-app update-definition \
  --definition-file-path "$DEF_FILE" \
  --selector "$WINDOW_SELECTOR"
```

Jump to **Output**.

### None valid -> fix and retry

If `$ATTEMPT >= 3`: save last evaluation result and jump to **Output** with errors.

Otherwise:

1. Read `$WORK_FOLDER/evaluation-result.txt` -- focus on `ToolingFeedback` per candidate for what went wrong (schema violations, invalid attributes, missing required fields, etc.).
2. Re-read `$WORK_FOLDER/improve-selector-output.json` -- what you generated.
3. Fix generation per feedback: adjust selectors to address flagged issues. Write corrected result to `$WORK_FOLDER/improve-selector-output.json` (overwrite).
4. Increment `$ATTEMPT` and go back to **Evaluate**.

## Output

**If `$QUIET` is `true`:** emit no template. Final message: one line `APPLIED <selector>`; append `BLOCKING: not distinct — N similar nodes` when every candidate carried a distinctness penalty. The calling skill will continue with its next step.

**If no valid selectors after 3 attempts:** present last evaluation errors and stop improving attempts.

**Otherwise:** read `$DEF_FILE` for selectors. Present with this template (fixed selector LAST, visible in terminal):

````
---
### Selector <Fixed|Improved>  (Score: <FinalScore>/1.0)

> **Root cause:** <one sentence -- why the original selector broke>  <- recover mode only

> **Strategy:** <one sentence -- what makes the selector more robust>

> <If there's a score penalty, add one line explaining it's a structural UI property, not fixable by selector changes. Exception: a distinctness penalty carried by every candidate is blocking for per-item/looped usage — report `BLOCKING: not distinct — N similar nodes`, never "acceptable".>

**Original:**
```
<Selector or ScopeSelectorArgument from the definition file XAML>
<each tag on its own line from PartialSelector>  <- omit if empty
```

**Window:**
```xml
<WindowSelector from evaluation result>
```

**Target:**  <- omit entire block if no EditablePartialSelector
```xml
<each tag on its own line from EditablePartialSelector>
```
---
````

- If window selector didn't change from original, omit from both **Original:** and **Window:** sections.
- Keep tight -- no full analysis dump. Detailed candidate analysis already saved in output files if needed.
- Do NOT show all 3 selectors. Do NOT retry for score penalties -- often structural UI properties, not fixable by adding tags.
