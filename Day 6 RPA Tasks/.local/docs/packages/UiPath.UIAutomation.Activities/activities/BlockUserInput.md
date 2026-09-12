# Block User Input

Suppress keyboard/mouse input until the set key combination is pressed, or timeout exceeded.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NBlockUserInput`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  |  |
| `BlockType` | Block | InArgument | [`NBlockInputType`](#nblockinputtype) |  |  |  | Indicates whether both keyboard and/or mouse are blocked. Default value is Both, which means that both keyboard and mouse are blocked. |
| `KeyModifiers` | Unblock using key modifiers | InArgument | [`NKeyModifiers`](common/NKeyModifiers.md) |  |  |  | Indicates the key modifiers that are part of the unblock key sequence, alongside the key. The unblock key sequence allows unblocking the user input, while the activity is still executing. |
| `Keys` | Unblock using key | InArgument | `string` |  |  |  | Indicates the key that is part of the unblock key sequence, alongside the key modifiers. The unblock key sequence allows unblocking the user input, while the activity is still executing. |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |

### Configuration

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `DisableUnblock` | Disable automatic unblock | InArgument | `bool` | Indicates whether to disable the automatic unblock of input after the inner activities are executed and the scope finishes execution. User input can be manually unblocked using the 'Unblock User Input' activity. Default value is false. |
| `Allow3rdPartyApps` | Allow 3rd party applications | InArgument | `bool` | Indicates whether input sent by other 3rd party applications is allowed or also blocked. Default value is false. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |

### Timings

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Timeout` | Timeout | InArgument | `double` | The amount of time (in seconds) to wait for the inner body sequence to be performed.
If the value is greater than 0, and body has not finished execution within the allotted timeout, then a timeout exception will be thrown.
The default value is 0 seconds, which means infinite/no time limit. |

## Enums

### NBlockInputType

`UiPath.UIAutomationNext.Shared.Enums.NBlockInputType`

The kind of user input blocked by the **Block User Input** activity.

| Value | Description |
|-------|-------------|
| `NBlockInputType.Both` | Both keyboard and mouse are blocked during execution. |
| `NBlockInputType.Keyboard` | Only the keyboard is blocked. |
| `NBlockInputType.Mouse` | Only the mouse is blocked. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NBlockUserInput`.

## Notes

- No mandatory parent scope is required for this activity.
- Use the `BlockType` property to control whether keyboard, mouse, or both input types are blocked.
- Configure `KeyModifiers` and `Keys` to define a key combination that allows the user to manually unblock input.
- When `DisableUnblock` is set to true, input remains blocked until an explicit **Unblock User Input** activity is executed.
- The `Allow3rdPartyApps` option controls whether input from other automation tools or third-party applications is also blocked.
