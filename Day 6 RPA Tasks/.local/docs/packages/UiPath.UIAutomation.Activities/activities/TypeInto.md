# Type Into

Enters text in a specified UI element, for example a text box.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NTypeInto`
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  | The UI element to perform the action on. |
| `Text` | Text | InArgument | `string` |  |  |  | The text to enter. You can add special keys from the Text Builder. |
| `SecureText` | Secure text | InArgument | `SecureString` |  |  |  | The SecureString value to enter. |
| `VerifyOptions` | Verify execution | Property | [`VerifyExecutionTypeIntoOptions`](#verifyexecutiontypeintooptions) |  |  |  | Define activity execution verification step. |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `DelayBetweenKeys` | Delay between keys | InArgument | `double` |  |  | Delay (in seconds) between consecutive keystrokes. The maximum value is 1 second. |
| `ActivateBefore` | Activate | InArgument | `bool` |  |  | Bring the UI element to the foreground and activate it before entering the text. |
| `ClickBeforeMode` | Click before typing | InArgument | [`NClickMode`](common/NClickMode.md) |  |  | Performs a click in the specified text-field before typing, in order to activate it. |
| `EmptyFieldMode` | Empty field | InArgument | [`NEmptyFieldMode`](common/NEmptyFieldMode.md) |  |  | Clear the existing content of the text-field before typing the text. Multiple methods available, compatible with various text-field types and applications. |
| `ClipboardMode` | Type by clipboard | InArgument | [`NTypeByClipboardMode`](common/NTypeByClipboardMode.md) |  |  | Indicates whether the clipboard is used for typing the given text. |
| `DeselectAfter` | Deselect at end | InArgument | `bool` |  |  | This option adds a Complete event after the text entry, in order to trigger certain UI responses in web browsers. |
| `AlterIfDisabled` | Alter disabled element | InArgument | `bool` |  |  | When selected, the activity executes the action even if the target element is disabled. Property does not apply if the input mode is Hardware Events. The default value is false. |
| `InteractionMode` | Input mode | InArgument | [`NChildInteractionMode`](common/NChildInteractionMode.md) |  |  | The method used to execute the click. |
| `HealingAgentBehavior` | Healing Agent mode | InArgument | [`NChildHealingAgentBehavior`](common/NChildHealingAgentBehavior.md) |  |  | Configures the Healing Agent actions if they are allowed by Governance or Orchestrator process/job/trigger level settings |
| `ScopeIdentifier` | Scope identifier | Property | `string` |  |  | Attaches this activity to a specific Application Card. Use the Application Card's `ScopeGuid` property. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` |  | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `Timeout` | Timeout | InArgument | `double` |  | The amount of time (in seconds) to wait for the operation to be performed before generating an error. The default value is 30 seconds. |
| `DelayAfter` | Delay after | InArgument | `double` |  | Delay (in seconds) after this activity is completed, before next activity starts. The default amount of time is 0.3 seconds. |
| `DelayBefore` | Delay before | InArgument | `double` |  | Delay (in seconds) to wait before executing this activity. The default amount of time is 0.2 seconds. |

## Special Keys Encoding Format

The `Text` property supports special key encoding mixed with regular text. Keys use `[d(...)]` (press down), `[u(...)]` (release), and `[k(...)]` (tap) tokens:

| Token | Meaning | Example |
|-------|---------|---------|
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

- All key names must be **lowercase**: `ctrl`, `shift`, `enter` -- not `Ctrl`, `SHIFT`, `Enter`.
- Escape a literal `[` by writing `[[`.
- Mix text and special keys: `"Hello[k(enter)]World"` types "Hello", presses Enter, types "World".

### Modifier Combinations

| Pattern | Syntax | Example |
|---------|--------|---------|
| Single modifier | `[d(mod)]key[u(mod)]` | `[d(ctrl)]c[u(ctrl)]` = Ctrl+C |
| Multiple modifiers | `[d(m1)][d(m2)]key[u(m2)][u(m1)]` | `[d(ctrl)][d(shift)]a[u(shift)][u(ctrl)]` = Ctrl+Shift+A |
| Modifier + special key | `[d(mod)][k(key)][u(mod)]` | `[d(alt)][k(f4)][u(alt)]` = Alt+F4 |
| Key sequence | Chain in one string | `[d(ctrl)]a[u(ctrl)][k(del)]` = Select all + delete |

### Common Examples

| Action | Text value |
|--------|-----------|
| Select all | `[d(ctrl)]a[u(ctrl)]` |
| Copy | `[d(ctrl)]c[u(ctrl)]` |
| Paste | `[d(ctrl)]v[u(ctrl)]` |
| Undo | `[d(ctrl)]z[u(ctrl)]` |
| Select all + delete (clear field) | `[d(ctrl)]a[u(ctrl)][k(del)]` |
| Tab to next field | `value1[k(tab)]value2[k(tab)]value3` |
| Shift+Tab (go back) | `[d(shift)][k(tab)][u(shift)]` |

### macOS special keys

On macOS the modifier layout differs from Windows. The primary shortcut modifier is **Command (⌘)**, not Control -- the macOS equivalents of Windows `Ctrl+C` / `Ctrl+V` / `Ctrl+A` / `Ctrl+S` are `⌘C` / `⌘V` / `⌘A` / `⌘S`.

| macOS key | Symbol | Token | Windows counterpart |
|-----------|--------|-------|---------------------|
| Command | ⌘ | `command` | Ctrl (for app shortcuts) / Windows key |
| Option | ⌥ | `option` | Alt |
| Control | ⌃ | `ctrl` | Ctrl (the physical Control key; rarely an app-shortcut modifier on macOS) |
| Shift | ⇧ | `shift` | Shift |

Type Into does **not** auto-convert Windows modifiers to macOS ones -- write the macOS token explicitly. `command` and `option` are used exactly like the Windows modifiers (`[d(...)]` / `[u(...)]`); `ctrl`, `shift`, `tab`, `enter`, function keys, etc. use the same tokens as Windows.

| Action | Text value |
|--------|-----------|
| Select all (⌘A) | `[d(command)]a[u(command)]` |
| Copy (⌘C) | `[d(command)]c[u(command)]` |
| Paste (⌘V) | `[d(command)]v[u(command)]` |
| Undo (⌘Z) | `[d(command)]z[u(command)]` |
| Clear field (⌘A then Delete) | `[d(command)]a[u(command)][k(del)]` |
| Move one word right (⌥→) | `[d(option)][k(right)][u(option)]` |

## Type Into Pitfalls

### Newlines trigger Enter

Typing a newline character sends an Enter key press. In messaging apps (Slack, Teams, etc.), this **sends the message** instead of creating a new line.

- **Slack, Teams, chat apps:** Use `[d(shift)][k(enter)][u(shift)]` for a newline within the message (Shift+Enter = newline, Enter = send).
- **Excel, Google Sheets:** Use `[d(alt)][k(enter)][u(alt)]` for a newline within a cell (Alt+Enter = newline, Enter = move to next cell).
- **Word, Notepad, most editors:** Enter = newline (no workaround needed).

### Auto-bulleted lists

Apps like Slack, Teams, PowerPoint, and Word auto-add bullets when pressing Enter in a list context. If you also type a bullet character, you get doubled bullets (`- - Item`). Only add the bullet for the first item; the app adds the rest.

## Sub-Objects

### VerifyExecutionTypeIntoOptions

`UiPath.UIAutomationNext.Activities.VerifyExecutionTypeIntoOptions`

Extends [`VerifyExecutionOptions`](common/VerifyExecutionOptions.md) with an additional `ExpectedText` property used to compare the actual typed result against an expected value.

| Property | Display Name | Type | Description |
|----------|-------------|------|-------------|
| `Target` | Target | [`TargetAnchorable`](common/Target.md#targetanchorable) | Target information that defines the UI element used for verification. |
| `Mode` | Verify mode | [`NVerifyMode`](common/NVerifyMode.md) | Defines whether to check if the verification target appears or disappears. |
| `Retry` | Retry | `InArgument<bool>` | When selected, the action is retried for the duration of the activity timeout, if the expected outcome was not achieved. |
| `Timeout` | Timeout | `InArgument<double>` | The amount of time (in seconds) to wait for the verification element to appear, disappear, or change. |
| `ExpectedText` | Expected text | `InArgument<string>` | The expected text to be compared with the actual result of typing (optional). |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NTypeInto`.

## Notes

- This activity must be placed inside a `UiPath.UIAutomationNext.Activities.NApplicationCard` scope.
