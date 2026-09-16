# Take Screenshot

Takes a screenshot of an application or UI element.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NTakeScreenshot`
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  | The UI element to perform the action on. |
| `SaveScreenshotTo` | Output to | Property | [`NSaveScreenshotTo`](#nsavescreenshotto) |  |  |  | Indicates how and where the screenshot should be saved. |
| `FileName` | File name | InArgument | `string` |  |  |  | The name of the file where the screenshot of the specified UI element will be saved. |
| `FileNameMode` | Auto increment | InArgument | [`NFileNameMode`](#nfilenamemode) |  |  |  | Defines what to append to the filename in case of filename conflicts. |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |

See [Output modes](#output-modes) for which of these apply, and when `FileName` is required.

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `HealingAgentBehavior` | Healing Agent mode | InArgument | [`NChildHealingAgentBehavior`](common/NChildHealingAgentBehavior.md) |  |  | Configures the Healing Agent actions if they are allowed by Governance or Orchestrator process/job/trigger level settings |
| `ScopeIdentifier` | Scope identifier | Property | `string` |  |  | Attaches this activity to a specific Application Card. Use the Application Card's `ScopeGuid` property. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `SavedTo` | Saved file path | `OutArgument` | The full path of the screenshot file including the appended suffix, if Auto-increment was used; used when Output is set to 'File' |
| `OutImage` | Saved image | `Image` | The screenshot saved as Image; used when Output is set to 'Image'. |
| `OutFile` | Saved file | `ILocalResource` | The screenshot saved as a png file. |
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |

See [Output modes](#output-modes) for which of these are populated in each mode.

### Common

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` |  |  | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `Timeout` | Timeout | InArgument | `double` |  |  | The amount of time (in seconds) to wait for the operation to be performed before generating an error. The default value is 30 seconds. |
| `DelayBeforeScreenshot` | Delay before screenshot | InArgument | `double` |  |  | Delay (in seconds) between bringing the UI element into foreground and actually taking the screenshot. The default amount of time is 0.2 seconds. |
| `DelayBefore` | Delay before | InArgument | `double` |  |  | Delay (in seconds) to wait before executing this activity. The default amount of time is 0.2 seconds. |

## Enums

### NSaveScreenshotTo

`UiPath.UIAutomationNext.Enums.NSaveScreenshotTo`

Indicates how and where the screenshot should be saved.

| Value | Description |
|-------|-------------|
| `NSaveScreenshotTo.File` | Writes the screenshot to disk. This is the default. |
| `NSaveScreenshotTo.Image` | Keeps the screenshot in memory; no file is written. |
| `NSaveScreenshotTo.Clipboard` | Copies the screenshot to the clipboard; no file is written. |

### NFileNameMode

`UiPath.UIAutomationNext.Enums.NFileNameMode`

How the output file name is generated when a file with the same name already exists.

| Value | Description |
|-------|-------------|
| `NFileNameMode.None` | The file name will be exactly as specified, and in case another file with the same name already exists, it will be overwritten. |
| `NFileNameMode.Index` | If one or multiple files that match the pattern `Filename (XX)` already exist, a new file is created with name `Filename (N+1)`, where `N` is the max index from the existing files. |
| `NFileNameMode.DateTime` | If one file with the specified name already exists, a new file is created with name `Filename YYYY.MM.DD at HH.MM.SS`. If a file with that name already exists, an index is appended (e.g. `Filename YYYY.MM.DD at HH:MM:SS (1)`). |

## Output modes

`SaveScreenshotTo` selects the destination and decides which of the other properties apply.

| `SaveScreenshotTo` | Required binding | Writes a file | Outputs populated |
|--------------------|------------------|---------------|-------------------|
| `File` (default)   | `FileName`       | Yes           | `SavedTo`, `OutFile`, `OutImage` |
| `Image`            | `OutImage`       | No            | `OutImage` |
| `Clipboard`        | —                | No            | `OutImage` |

`SavedTo` and `OutFile` are the only mode-dependent outputs, because they need a file on disk. `OutImage` is populated in every mode whenever it is bound, so it can be bound next to `FileName` to get both a file and an in-memory image. `FileNameMode` applies to `File` mode only.

> **IMPORTANT:** `SaveScreenshotTo` defaults to `File`, so **`FileName` is required unless you change it.** Binding only `OutImage` is not enough — with `SaveScreenshotTo` left at `File`, validation fails with `"File name" field is required.` To output an in-memory image, set `SaveScreenshotTo` to `Image` *and* bind `OutImage`; in `Image` mode an unbound `OutImage` fails at runtime with `Required argument 'Saved image' was not provided.`

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NTakeScreenshot`.

## Notes

- **Scope:** the **Required Scope** listed at the top of this page applies to cross-platform projects. In Windows projects the **Use Application/Browser** (`NApplicationCard`) scope is **optional** — unlike most target activities, which require it everywhere — and the activity validates and runs at workflow root.
- Without a scope and without a `Target` or `InUiElement`, the activity captures the whole desktop; with a scope it falls back to the scope's focused window. Setting the `IsDesktop` property to `true` forces a full-desktop capture and ignores any configured target.
- The `FileNameMode` property controls behavior when a file with the same name already exists.
- The `DelayAfter` property is hidden in this activity.
