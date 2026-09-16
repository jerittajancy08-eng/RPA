# Browser Dialog Scope

Captures and handles browser dialogs such as alert, confirm and prompt.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Browser
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NBrowserDialogScope`
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  | The UI element to perform the action on. |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `DialogScopeType` | Dialog type | Property | [`NBrowserDialogScopeType`](#nbrowserdialogscopetype) | `AlertSingle` |  | The type of browser dialog handled by the Browser Dialog Scope activity. |
| `DialogResponse` | Dialog response | InArgument | [`NBrowserDialogResponse`](common/NBrowserDialogResponse.md) |  |  | Response to the JavaScript dialog. |
| `PromptDialogResponseText` | Prompt response text | InArgument | `string` |  |  | Text response for JavaScript prompt dialogs. |
| `WaitForDialogToAppearTimeout` | Wait for dialog to appear timeout | InArgument | `double` |  |  | The amount of time (in seconds) to wait for the dialog to appear after children activities finish executing. The default value is 30 seconds. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `DialogMessage` | Dialog message | `string` | Variable to store the message of the dialog handled by the dialog scope. |
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |

### Common

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` |  |  | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `Timeout` | Timeout | InArgument | `double` |  |  | The amount of time (in seconds) to wait for the operation to be performed before generating an error. The default value is 30 seconds. |
| `DelayBefore` | Delay before | InArgument | `double` |  |  | Delay (in seconds) to wait before executing this activity. The default amount of time is 0.2 seconds. |

## Enums

### NBrowserDialogScopeType

`UiPath.UIAutomationNext.Enums.NBrowserDialogScopeType`

The browser dialog type handled by **Browser Dialog Scope**.

| Value | Description |
|-------|-------------|
| `NBrowserDialogScopeType.AlertSingle` | A JavaScript `alert()` dialog. |
| `NBrowserDialogScopeType.ConfirmSingle` | A JavaScript `confirm()` dialog. |
| `NBrowserDialogScopeType.PromptSingle` | A JavaScript `prompt()` dialog. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NBrowserDialogScope`.

## Notes

- This activity must be placed inside a **Use Application/Browser** (`NApplicationCard`) scope.
- Place the activities that trigger the browser dialog inside the scope body.
- Supports JavaScript `alert()`, `confirm()`, and `prompt()` dialogs.
- Use `DialogResponse` to accept or dismiss the dialog, and `PromptDialogResponseText` to provide text input for prompt dialogs.
- The `DelayAfter` property is hidden in this activity.
