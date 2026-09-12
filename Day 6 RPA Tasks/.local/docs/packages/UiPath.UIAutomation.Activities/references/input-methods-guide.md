# Input Methods

One decision, two consumers: `uip rpa uia interact --input-method` while driving the live app, and workflow activity `InteractionMode`. Prefer `DebuggerApi` (Chromium) / `Simulate` over the `HardwareEvents` default; the method proven while driving is the method activities use.

`click`, `type`, `hover`, `wheel`: `--input-method <HardwareEvents|Simulate|DebuggerApi|WindowMessages>`; default `HardwareEvents`. `HardwareEvents`, `Simulate`, and `DebuggerApi` are cross-platform; `WindowMessages` is Windows-only.

## Selection order

Do not settle on the CLI default. `DebuggerApi`/`Simulate` need no foreground/cursor, lose no events, and are usually more reliable:

1. Chromium (Chrome, Edge) b-refs → `DebuggerApi`.
2. Firefox (b-refs), Java Swing/AWT, SAP GUI → `Simulate`.
3. Any other app → try `Simulate` first; support varies and failures are silent — verify each action (`interact get`/`get-all`/re-snapshot).
4. `Simulate` no-op or wrong result → `HardwareEvents`; background operation still required on Windows → `WindowMessages`.

Exceptions needing key events regardless of `Simulate` support: special keys/modifiers (§ Interactions with other features), date/formatted date-time inputs, SAP hotkeys.

## Methods

| Method | How it works | Use when |
|--------|--------------|----------|
| `DebuggerApi` | Dispatches events through Chromium debugger protocol. No foreground required. | First choice for Chrome / Edge. |
| `Simulate` | Calls element's native API directly — no cursor movement, works on background windows and off-screen elements. Usually auto-clears field before typing: do NOT pass `--clear-before-mode` by default; verify result with `interact get`/`get-all`, retry with it only if field holds unexpected text. | First choice for everything non-Chromium: Firefox (b-ref targets), Java Swing/AWT apps, SAP GUI session windows, background windows, hover-revealed targets. Other desktop apps: try, then verify. |
| `HardwareEvents` (default) | Simulates real mouse/keyboard input. Auto-activates window — target must be foreground and on-screen. Typing appends to existing text; pass `--clear-before-mode` to clear first. | Fallback when `Simulate` no-ops; targets requiring real key events. |
| `WindowMessages` | Posts window messages to target (Windows-only). Works on background windows. | Windows apps where `Simulate` has no effect but background operation still needed. |

## Switch triggers

- **Success without visible effect** — on `Simulate`: app lacks support, fall back to `HardwareEvents`. On `HardwareEvents`: retry `Simulate` (Chromium: `DebuggerApi`).
- **`Cannot send input ... outside of screen bounds`** — common for hover/click-revealed pop-up menu items, autocomplete entries, and dropdown rows under `HardwareEvents`; use `Simulate` or `DebuggerApi`.
- **Window must stay in background** → `Simulate` (or `WindowMessages` on Windows).
- **Fallback chains, browsers:** Chromium `DebuggerApi` → `Simulate` → `HardwareEvents`. Firefox `Simulate` → `HardwareEvents` (`DebuggerApi` unsupported).

## Carry the proven method into the workflow

Driving the app with `interact` (advancing state, revealing elements) doubles as a live input-method support probe. Record the method each app accepted; author activities with it. Never leave activities on the `HardwareEvents` default after `Simulate`/`DebuggerApi` worked live.

CLI `--input-method` values match the activity/coded enum values 1:1 (card: [`NInteractionMode`](../activities/common/NInteractionMode.md); children: [`NChildInteractionMode`](../activities/common/NChildInteractionMode.md), default `SameAsCard`). Studio displays `DebuggerApi` as "Chromium API".

- **App-wide** (method worked for every probed interaction): set once at card level — XAML `NApplicationCard` `InteractionMode`; coded `uiAutomation.Open/Attach(..., interactionMode: NInteractionMode.Simulate)` or `TargetAppOptions.InteractionMode`. Children inherit via `SameAsCard`.
- **Per-activity exception** (one step needed a different method live — special keys, date input, SAP hotkey): keep card value; override that activity's `InteractionMode` (XAML property; coded `*Options.InteractionMode`, e.g. `TypeIntoOptions`).
- **Method not proven live:** do not configure it; keep default.

## Interactions with other features

- **Special keys** (`[k(key)]`/`[d(key)]`/`[u(key)]` in `type`) and `--modifiers` (`click`/`wheel`): fully supported by `HardwareEvents` and `DebuggerApi`. `Simulate` supports only some targets (browser b-refs, SAP session windows); others may silently ignore them.
- **Date/formatted date-time inputs:** use key events (`DebuggerApi` or `HardwareEvents`). `Simulate` sets value without validation-dependent input events. See `uia-elements-interaction-guide.md` § Date and formatted date-time inputs in this `references/` folder.

## Full parameter reference

Sibling `cli-reference.md` § Interact: per-verb flags.
