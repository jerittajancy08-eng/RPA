# Configure Target Guide

**Always use `uia-configure-target`** to find/create Object Repository targets: capture app, discover elements, generate/improve selectors, register in OR.

> **Working directory:** run every `uip rpa uia` call from project directory containing `project.json`.

## Execution Model

**Execute steps inline in main conversation.** Never delegate entire skill; its procedure spawns required subagents.

- **OR references** stay visible for workflow attachment ([uia-target-attachment-guide.md](uia-target-attachment-guide.md)).
- **Context continuity:** main conversation tracks registered screens/elements through earlier returned references plus live OR CLI queries, preventing duplicate captures and incoherent workflow builds.

Read SKILL.md; execute every procedure step. Spawn `Agent` only where explicitly required.

## Invocation

Read [`SKILL.md`](../skills/uia-configure-target/SKILL.md) for the procedure — a **reference doc, not a Skill-tool slash command**. It documents every argument with defaults and valid values (`--screen-reference-id`, `--cv`, `--semantic`, `--no-improve`, `--project-dir`); CV sub-procedure invoked via automatic fallback: [SKILL.md § CV Element Resolution](../skills/uia-configure-target/SKILL.md#cv-element-resolution-sub-procedure).

**Locate file:** fixed package path `{PROJECT_DIR}/.local/docs/packages/UiPath.UIAutomation.Activities/skills/uia-configure-target/SKILL.md`; open with direct `Read`. `Glob` pattern **must start `**/`** because matching is relative to working directory, not `path`; literal folder prefix misses:

- ✅ `Glob(path=".../UiPath.UIAutomation.Activities", pattern="**/uia-configure-target/SKILL.md")`
- ❌ `Glob(path=".../UiPath.UIAutomation.Activities", pattern="skills/uia-configure-target/SKILL.md")` — returns "No files found" though file exists.

`Glob` miss does not prove absence. Installed package version in `project.json` is authoritative.

Check [Unsupported Activities](#unsupported-activities) first. Listed activity skips `uia-configure-target`; use [Indication Fallback](#indication-fallback).

### What the Skill Does

Ensures screen (optionally one or more elements) exists in Object Repository: searches OR before creation; resolves targets through selector, CV, then semantic routes; improves/anchors where applicable; registers in OR. Returns one reference ID per element plus screen reference, for workflow attachment.

### Invocation Modes

- **TargetAnchorable** (element within window — Click, TypeInto, GetText, etc.):

  ```
  --window <description> --elements <description>
  ```

- **TargetApp** (window only — Use Application/Browser):

  ```
  --window <description>
  ```

- **Disable CV fallback** (route selector failures directly to semantic fallback):

  ```
  --window <description> --elements <description> --cv false
  ```

- **Selector only** (disable both fallbacks):

  ```
  --window <description> --elements <description> --cv false --semantic false
  ```

### Automatic Fallback Chain

CV and semantic fallback are enabled by default: selector -> CV -> semantic. An element leaves selector when:

- Element description: no clear match in live tree (after UIA-framework retry and screenshot disambiguation pass).
- Produced selector still `NEEDS_IMPROVEMENT` after snapshot-retry and uia-improve-selector subagent pass.

Both cases: fallback enabled -> move element to CV without prompting. CV failure moves to semantic when enabled. With `--cv false`, selector failures move directly to semantic; only `--cv false --semantic false` surfaces them. Elements resolving cleanly stay on selector; final registration may mix routes.

### Batch Element Configuration

Separate element descriptions with `|` in single `--elements` value: window captured once, reused for all elements:

```
--window <description> --elements "element one | element two | element three"
```

Avoids redundant window captures and screen lookups when multiple elements live on same screen. Batch may mix selector, CV, and semantic targets.

### Multi-Screen Capture Sessions

Skill returns screen reference ID alongside element IDs. Capturing several UI states of **same window** (advance → capture → advance): pass that ID on every subsequent invocation:

```
--window <description> --elements "..." --screen-reference-id <id-from-first-invocation>
```

Skips OR screen lookup on every screen after the first. Combined with batch `|` elements, each screen runs same short set of chained invocations. Omit flag when window (application) changes — skill then searches OR as usual. Advance and ordering rules: [Multi-Step UI Flows](#multi-step-ui-flows).

### Unsupported Activities

`uia-configure-target` does not configure targets for:

- **UI Automation.Semantic**: Fill Form, Update UI Element, Close Popup, Extract Form Data, Extract UI Data
- **Extract Table Data**

## Rules

**Never manually call internal selector-building `uip rpa uia` CLIs.** Direct calls bypass improvement and OR registration, leaving fragile unregistered selectors. Only SKILL.md flow is valid.

## Multi-Step UI Flows

Elements may appear only after earlier interactions (e.g., compose after "New mail", confirmation after submit). `uia-configure-target` sees current state only; **advance to each state** before capture.

> **CRITICAL: Complete-then-advance.** Finish full `uia-configure-target` flow, including OR registration, for ALL currently visible elements before advancing. Advance may irreversibly hide previous elements and break registration.
>
> **Never test with `uip rpa uia interact` during capture** (e.g., autocomplete/button behavior). Test completed workflow later. During capture, interact ONLY advances to next state for newly revealed targets.

### Advancing UI State

After OR registration, use `uip rpa uia interact` on element/sibling to reach next needed capture state. Use every legitimately required verb (e.g., open menu then click; type then Enter), but never explore ahead or verify behavior. NEVER navigate by typing URL from memory or training knowledge — next state is reached by interacting with captured elements, not by guessing addresses. Verbs/refs/flags: [Advancing UI State](../ui-automation-guide.md#advancing-ui-state). Never read attributes to hand-write/edit selectors; configure-target owns construction/improvement.

Advance with the preferred input method ([input-methods-guide.md § Selection order](input-methods-guide.md#selection-order)), not the `HardwareEvents` default. Noting which method each advance accepted is part of advancing, not behavior testing — it becomes the workflow's `InteractionMode`.

**Reuse current capture refs.** `interact` resolves against the latest in-memory snapshot. Pass same e-refs (`e28`, `e35`, etc.). Unchanged UI needs no ref re-minting.

Re-capture only after UI advances; pre-advance refs do not resolve in new state. Commands: [Advancing UI State](../ui-automation-guide.md#advancing-ui-state).

### Capture Loop

1. **Capture current state completely:** run full skill through OR registration for ALL visible elements; never stop at raw selector.
2. **Advance UI** to next state via `uip rpa uia interact` CLI.
3. **Capture new state:** rerun full skill for newly visible elements.
4. **Repeat** until all targets are in OR.

**Never advance via partial `uip rpa run` / `debug start`:** workflow lifecycle may close app. Stateless `uip rpa uia interact` performs one action and leaves resulting state.

### Per-Screen Batching (call-count discipline)

Use existing batched OR CLI entry points per screen: one all-element round-trip, not N calls.

- **One shared snapshot per screen.** Pass same snapshot folder to screen- and element-registration. Per-element recapture wastes calls and risks stale/shifted DOM if app moves.
- **One element-registration call per screen.** Pass all current-screen definition paths; never loop per element.
- **One element-XAML retrieval per screen.** Pass all just-registered reference IDs; never loop per ID.
- **Screen-XAML retrieval is per-screen:** single-target by design, one extra call per screen, not element.
- **No cross-screen batching:** N screens require N rounds separated by `interact` advances.

Batch subcommands, flags, argument shapes: search [cli-reference.md](cli-reference.md) § Object Repository; do not read full file.

## Cross-Process Helper Dialogs (Sign-in, OAuth, System Pop-ups)

Sign-in, consent, or system dialogs may use **separate process**, not same-app window (e.g., Microsoft Store sign-in `WWAHost.exe`, OAuth system browser, Save/Open/UAC via `consent.exe`/`dllhost.exe`). For XAML, compare captured `app=` per [Cross-Process Helper Dialogs](../ui-automation-guide.md#cross-process-helper-dialogs-sign-in-oauth-system-pop-ups): different requires helper card; matching only passes compatibility, so confirm same attached instance before reuse. Coded workflows attach each Object Repository screen handle.

### Capturing Targets for Helper Processes

Finish/register host trigger, run [Window Baseline](../ui-automation-guide.md#window-baseline), then launch via `uip rpa uia interact` (e.g., "Sign In"). Configure newly visible helper window, register elements, continue. Treat as own Complete-then-advance capture screen; never capture through host selector.

## Indication Fallback

Use indication only when:

- Activity appears in [Unsupported Activities](#unsupported-activities); or
- Bounded automated advancement/capture cannot expose target.

Post-interaction visibility alone is not indication criterion. If `interact` can expose target, finish current-state OR registration, advance, then rerun `uia-configure-target`. Indication requires user to physically click target.

Steps, response shape, coded/XAML OR regeneration, CLI reference: [indication-fallback-workflow.md](indication-fallback-workflow.md).

## Attaching Targets to Workflow Activities

After OR registration via skill or indication, attach to XAML per [uia-target-attachment-guide.md](uia-target-attachment-guide.md), source for both paths' commands, flags, outputs.

**Path choice:**

- **Link — DEFAULT:** attach by `sap2010:WorkflowViewState.IdRef`; screen first, then all elements in one batch. Studio need not open file.
- **Embed — per-reference link-failure fallback:** inline failed reference's OR-resolved XAML under consuming activity only, never whole screen.

Link first. On reference failure, embed it immediately; never try activity-id/display-name variations (§ CLI Pitfalls).

### Multi-Screen Workflows

For multi-screen XAML, honor caller ordering: capture-first tasks defer authoring until all targets register; otherwise add each screen's activities when references arrive. Returned refs per screen are not an instruction to author per screen. Everything configured before next `uip rpa uia interact` advance is one Complete-then-advance batch. Validate each batch; attach via [uia-target-attachment-guide.md](uia-target-attachment-guide.md).

## CLI Pitfalls

For canonical flags, values, artifact names, search [cli-reference.md](cli-reference.md); do not read full file.

- **Snapshot filter missing-argument error:** requires target definition file plus folder arguments.
- **`Invalid --refs entry`:** selector resolution requires each element ref paired with owning definition file.
- **OR creation rejects inline JSON:** generate per-element definition files; pass paths to create-elements.
- **Interact `unknown option`:** actions accept only interaction-shape flags; discovery/global folder, ref, project-dir-style flags belong to other families.
- **`Could not retrieve the activity from the workflow`:** not activity-id/display-name/reference-ID issue. Stop after **first** failure for reference; embed fallback (§ Attaching Targets).
