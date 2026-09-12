# Keyboard Shortcuts

Sends one or more keyboard shortcuts to a UI element.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NKeyboardShortcuts`
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  | The UI element to perform the action on. |
| `ShortcutsArgument` | Shortcuts | InArgument | `string` |  |  |  | The keyboard shortcuts to be sent. |
| `Shortcuts` | Shortcuts | Property | `string` |  |  |  | The keyboard shortcuts to be sent. |
| `VerifyOptions` | Verify execution | Property | [`VerifyExecutionOptions`](common/VerifyExecutionOptions.md) |  |  |  | Define activity execution verification step. |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `ActivateBefore` | Activate | InArgument | `bool` |  |  | Bring the target UI element to the foreground and activate it before sending the shortcut. |
| `DelayBetweenShortcuts` | Delay between shortcuts | InArgument | `double` |  |  | Delay (in seconds) between consecutive shortcuts. |
| `DelayBetweenKeys` | Delay between keys | InArgument | `double` |  |  | Delay (in seconds) between consecutive keystrokes. The maximum value is 1 second. |
| `ClickBeforeMode` | Click before typing | InArgument | [`NClickMode`](common/NClickMode.md) |  |  | The type of click to execute in the specified UI element before sending the shortcut. |
| `InteractionMode` | Input mode | InArgument | [`NChildInteractionMode`](common/NChildInteractionMode.md) |  |  | The method used to execute the click. |
| `HealingAgentBehavior` | Healing Agent mode | InArgument | [`NChildHealingAgentBehavior`](common/NChildHealingAgentBehavior.md) |  |  | Configures the Healing Agent actions if they are allowed by Governance or Orchestrator process/job/trigger level settings |
| `ScopeIdentifier` | Scope identifier | Property | `string` |  |  | Attaches this activity to a specific Application Card. Use the Application Card's `ScopeGuid` property. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |

### Common

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` |  |  | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `Timeout` | Timeout | InArgument | `double` |  |  | The amount of time (in seconds) to wait for the operation to be performed before generating an error. The default value is 30 seconds. |
| `DelayAfter` | Delay after | InArgument | `double` |  |  | Delay (in seconds) after this activity is completed, before next activity starts. The default amount of time is 0.3 seconds. |
| `DelayBefore` | Delay before | InArgument | `double` |  |  | Delay (in seconds) to wait before executing this activity. The default amount of time is 0.2 seconds. |

## CRITICAL: `Shortcuts` vs `ShortcutsArgument`

This activity has **two** shortcut properties -- using the wrong one causes VB bracket parsing failures:

| XAML attribute | C# type | Bracket behavior | When to use |
|----------------|---------|------------------|-------------|
| `Shortcuts` | `string` (plain property) | **Literal text** -- brackets are part of the hotkey encoding | **Always use this** for hardcoded shortcuts |
| `ShortcutsArgument` | `InArgument<string>` | **VB expression** -- `[...]` parsed as VB, will FAIL | Only for dynamic/variable-driven shortcuts |

**NEVER set `ShortcutsArgument` with hotkey encoding directly** -- the VB parser tries to evaluate `d(hk)` as a function call and throws.

## Hotkey Encoding Format

Every shortcut sequence is wrapped in `[d(hk)]...[u(hk)]` delimiters (shortcut-start / shortcut-end). Inside the wrapper, modifiers, special keys, and printable characters are mixed using the tokens below.

### Tokens

| Token | Meaning | Example |
|-------|---------|---------|
| `[d(hk)]` | Start of shortcut sequence | Required at the beginning of each shortcut |
| `[u(hk)]` | End of shortcut sequence | Required at the end of each shortcut |
| `[d(ctrl)]` | Hold Ctrl modifier | `[d(ctrl)]a[u(ctrl)]` = Ctrl+A |
| `[u(ctrl)]` | Release Ctrl modifier | Always pair with `[d(ctrl)]` |
| `[d(shift)]` | Hold Shift | |
| `[u(shift)]` | Release Shift | |
| `[d(alt)]` | Hold Alt | |
| `[u(alt)]` | Release Alt | |
| `[d(lwin)]` | Hold Windows key | |
| `[u(lwin)]` | Release Windows key | |
| `[k(tab)]` | Press Tab | Use `[k(...)]` for non-printable keys |
| `[k(enter)]` | Press Enter | |
| `[k(back)]` | Press Backspace | |
| `[k(del)]` | Press Delete | |
| `[k(f1)]`--`[k(f12)]` | Function keys | |
| `a`, `w`, etc. | Printable character | Plain characters, no brackets |
| ` ` (literal space) | Press Space | NOT `[k(space)]` |

### Combining tokens

**Multiple modifiers** combine in a single `[d(...)]` block, released in reverse order: `[d(alt)d(ctrl)]...[u(ctrl)u(alt)]`.

**Multiple shortcut sequences** are concatenated: `[d(hk)]...[u(hk)][d(hk)]...[u(hk)]`.

### Examples

#### Single-modifier shortcuts

| Shortcut | `Shortcuts` value |
|----------|-------------------|
| Ctrl+A (select all) | `[d(hk)][d(ctrl)]a[u(ctrl)][u(hk)]` |
| Ctrl+C (copy) | `[d(hk)][d(ctrl)]c[u(ctrl)][u(hk)]` |
| Ctrl+V (paste) | `[d(hk)][d(ctrl)]v[u(ctrl)][u(hk)]` |
| Ctrl+Z (undo) | `[d(hk)][d(ctrl)]z[u(ctrl)][u(hk)]` |
| Ctrl+S (save) | `[d(hk)][d(ctrl)]s[u(ctrl)][u(hk)]` |
| Shift+Tab (previous field) | `[d(hk)][d(shift)][k(tab)][u(shift)][u(hk)]` |

#### Multi-modifier shortcuts

Multiple modifiers go in a single `[d(...)]` block; release them in reverse order.

| Shortcut | `Shortcuts` value |
|----------|-------------------|
| Ctrl+Shift+T (reopen tab) | `[d(hk)][d(ctrl)d(shift)]t[u(shift)u(ctrl)][u(hk)]` |
| Ctrl+Shift+J (DevTools console) | `[d(hk)][d(ctrl)d(shift)]j[u(shift)u(ctrl)][u(hk)]` |
| Ctrl+Alt+Del | `[d(hk)][d(ctrl)d(alt)][k(del)][u(alt)u(ctrl)][u(hk)]` |
| Ctrl+Shift+Esc (Task Manager) | `[d(hk)][d(ctrl)d(shift)][k(esc)][u(shift)u(ctrl)][u(hk)]` |

#### Function and special keys

| Shortcut | `Shortcuts` value |
|----------|-------------------|
| Alt+F4 (close window) | `[d(hk)][d(alt)][k(f4)][u(alt)][u(hk)]` |
| F5 (refresh) | `[d(hk)][k(f5)][u(hk)]` |
| Enter | `[d(hk)][k(enter)][u(hk)]` |
| Esc | `[d(hk)][k(esc)][u(hk)]` |
| Space | `[d(hk)] [u(hk)]` |

#### Windows key shortcuts

| Shortcut | `Shortcuts` value |
|----------|-------------------|
| Win+R (Run dialog) | `[d(hk)][d(lwin)]r[u(lwin)][u(hk)]` |
| Win+D (show desktop) | `[d(hk)][d(lwin)]d[u(lwin)][u(hk)]` |
| Win+L (lock) | `[d(hk)][d(lwin)]l[u(lwin)][u(hk)]` |

#### macOS shortcuts

On macOS the primary shortcut modifier is **Command (⌘)**, not Control. Use `command` for ⌘ and `option` for ⌥; `ctrl` and `shift` are the same tokens as on Windows.

| macOS key | Symbol | Token | Windows counterpart |
|-----------|--------|-------|---------------------|
| Command | ⌘ | `command` | Ctrl (for app shortcuts) / Windows key |
| Option | ⌥ | `option` | Alt |
| Control | ⌃ | `ctrl` | Ctrl (the physical Control key; rarely an app-shortcut modifier on macOS) |
| Shift | ⇧ | `shift` | Shift |

| Shortcut | `Shortcuts` value |
|----------|-------------------|
| ⌘C (copy) | `[d(hk)][d(command)]c[u(command)][u(hk)]` |
| ⌘V (paste) | `[d(hk)][d(command)]v[u(command)][u(hk)]` |
| ⌘A (select all) | `[d(hk)][d(command)]a[u(command)][u(hk)]` |
| ⌘S (save) | `[d(hk)][d(command)]s[u(command)][u(hk)]` |
| ⌘Q (quit app) | `[d(hk)][d(command)]q[u(command)][u(hk)]` |
| ⌘⇧T (reopen tab) | `[d(hk)][d(command)d(shift)]t[u(shift)u(command)][u(hk)]` |
| ⌥⌘Esc (Force Quit) | `[d(hk)][d(option)d(command)][k(esc)][u(command)u(option)][u(hk)]` |
| ⌘Space (Spotlight) | `[d(hk)][d(command)] [u(command)][u(hk)]` |

**Cross-platform auto-mapping.** When Keyboard Shortcuts runs on macOS it automatically rewrites the Windows-key modifier (`win` / `lwin` / `rwin`) to **Command** and `alt` (`lalt` / `ralt`) to **Option**. A shortcut authored with the Windows key therefore adapts per platform: `[d(hk)][d(lwin)]c[u(lwin)][u(hk)]` sends Win+C on Windows and ⌘C on macOS. `ctrl` is **not** remapped -- it stays the Control key (⌃) on macOS, so a Windows `Ctrl+C` shortcut does not become ⌘C. When you specifically want ⌘, use `command` (or rely on the Windows-key auto-mapping above).

#### Multiple shortcuts in sequence

Concatenate `[d(hk)]...[u(hk)]` blocks. The `DelayBetweenShortcuts` property controls pacing between them.

| Action | `Shortcuts` value |
|--------|-------------------|
| Save, then confirm with Enter | `[d(hk)][d(ctrl)]s[u(ctrl)][u(hk)][d(hk)][k(enter)][u(hk)]` |
| Select all, then delete | `[d(hk)][d(ctrl)]a[u(ctrl)][u(hk)][d(hk)][k(del)][u(hk)]` |
| Open Find, type "todo", press Enter | `[d(hk)][d(ctrl)]f[u(ctrl)][u(hk)][d(hk)]todo[u(hk)][d(hk)][k(enter)][u(hk)]` |

#### XAML — hardcoded shortcut

Use the **`Shortcuts`** property (plain `string`). Brackets are part of the encoding and are not parsed as VB.

```xml
<uix:NKeyboardShortcuts Shortcuts="[d(hk)][d(ctrl)]s[u(ctrl)][u(hk)]" Version="V5">
  <uix:NKeyboardShortcuts.Target>
    <uix:TargetAnchorable Version="V6" />
  </uix:NKeyboardShortcuts.Target>
</uix:NKeyboardShortcuts>
```

#### XAML — dynamic shortcut from a variable

Use **`ShortcutsArgument`** (`InArgument<string>`) when the shortcut is built at runtime. The encoding lives in the variable's value, not in the VB expression — never put `[d(hk)]...` directly into `ShortcutsArgument` text, or the VB parser will try to evaluate it.

```xml
<uix:NKeyboardShortcuts Version="V5">
  <uix:NKeyboardShortcuts.ShortcutsArgument>
    <InArgument x:TypeArguments="x:String">[shortcutValue]</InArgument>
  </uix:NKeyboardShortcuts.ShortcutsArgument>
  <uix:NKeyboardShortcuts.Target>
    <uix:TargetAnchorable Version="V6" />
  </uix:NKeyboardShortcuts.Target>
</uix:NKeyboardShortcuts>
```

To compose a shortcut around a runtime value (for example, a digit picked from data), build the encoded string with `String.Format`:

```text
String.Format("[d(hk)][d(ctrl)]{0}[u(ctrl)][u(hk)]", tabIndex)
```

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NKeyboardShortcuts`.

## Notes

- This activity must be placed inside a **Use Application/Browser** (`NApplicationCard`) scope.
- Supports sending multiple shortcuts in sequence, with configurable delay between them.
- Use `ActivateBefore` to ensure the target element has focus before sending shortcuts.
- The `ClickBeforeMode` property allows clicking the element before sending the shortcuts to ensure focus.
- `InteractionMode="HardwareEvents"` is the most reliable mode for keyboard shortcuts.
