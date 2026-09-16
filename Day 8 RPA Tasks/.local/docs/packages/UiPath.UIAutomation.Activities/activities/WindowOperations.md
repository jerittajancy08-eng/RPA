# Window Operation

Perform various operations on the specified window element.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NWindowOperation`
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  |  |
| `Operation` | Operation | InArgument | [`NWindowOperationType`](#nwindowoperationtype) |  |  |  | The operation to be performed on the given window. |
| `X` | X | InArgument | `int` |  |  |  | The new position of the window's left edge, in relation to the desktop. |
| `Y` | Y | InArgument | `int` |  |  |  | The new position of the window's top edge, in relation to the desktop. |
| `Width` | Width | InArgument | `int` |  |  |  | The new width of the window. |
| `Height` | Height | InArgument | `int` |  |  |  | The new height of the window. |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |

### Configuration

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `HealingAgentBehavior` | Healing Agent mode | InArgument | [`NChildHealingAgentBehavior`](common/NChildHealingAgentBehavior.md) | Configures the Healing Agent actions if they are allowed by Governance or Orchestrator process/job/trigger level settings. |
| `ScopeIdentifier` | Scope identifier | Property | `string` | Attaches this activity to a specific Application Card. Use the Application Card's `ScopeGuid` property. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |

### Common

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `Timeout` | Timeout | InArgument | `double` | The amount of time (in seconds) to wait for the operation to be performed before generating an error. The default value is 30 seconds. |
| `DelayAfter` | Delay after | InArgument | `double` | Delay (in seconds) after this activity is completed, before next activity starts. The default amount of time is 0.3 seconds. |
| `DelayBefore` | Delay before | InArgument | `double` | Delay (in seconds) to wait before executing this activity. The default amount of time is 0.2 seconds. |

## Enums

### NWindowOperationType

`UiPath.UIAutomationNext.Shared.Enums.NWindowOperationType`

The window operation performed by the **Window Operations** activity.

| Value | Description |
|-------|-------------|
| `NWindowOperationType.Show` | Show window. |
| `NWindowOperationType.Hide` | Hide window. |
| `NWindowOperationType.ToForeground` | Bring window to foreground. |
| `NWindowOperationType.Maximize` | Maximize window. |
| `NWindowOperationType.Minimize` | Minimize window. |
| `NWindowOperationType.Restore` | Restore window. |
| `NWindowOperationType.Close` | Close window. |
| `NWindowOperationType.Move` | Move window. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NWindowOperation`.

## Notes

- This activity must be placed inside a **Use Application/Browser** (`NApplicationCard`) scope.
- The `Operation` property determines the window action (e.g., Maximize, Minimize, Restore, Move, Resize, Close).
- The `X`, `Y`, `Width`, and `Height` properties are only relevant for Move and Resize operations.
