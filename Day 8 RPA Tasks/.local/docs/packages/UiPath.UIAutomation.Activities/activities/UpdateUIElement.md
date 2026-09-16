# Update UI Element

Uses AI to seamlessly update a UI element's state/value.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Semantic
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.Semantic.Activities.NSetValue`
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  |  |
| `Value` | Value | InArgument | `string` |  |  |  | The value that will be set on the field. |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |

### Configuration

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `EnableValidation` | Enable validation | Property | `bool` | Enables execution validation for the run-time value. An exception will be thrown if the internal validation mechanism detects an invalid value after the execution. |
| `InteractionMode` | Input mode | InArgument | [`NChildInteractionMode`](common/NChildInteractionMode.md) | The method used to execute the click. |
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

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.Semantic.Activities.NSetValue`.

## Notes

- This activity must be placed inside a **Use Application/Browser** (`NApplicationCard`) scope.
- The `Value` property specifies the string value to set on the target UI element.
- Enable `EnableValidation` to verify that the element was correctly updated after AI execution.
- AI determines the best interaction method to update the UI element based on its type (text field, dropdown, checkbox, etc.).
