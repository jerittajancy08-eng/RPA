# Click Event Trigger

Setup a click event trigger on the indicated UI Element.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NClickTrigger`
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  | The UI element to perform the action on. |
| `Button` | Mouse button | InArgument | [`NMouseButton`](common/NMouseButton.md) |  |  |  | The mouse button that is monitored. |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `KeyModifiers` | Key modifiers | InArgument | [`NKeyModifiers`](common/NKeyModifiers.md) |  |  | Adds a key modifier that is monitored alongside the mouse button. |
| `BlockEvent` | Block event | InArgument | `bool` |  |  | Specifies whether the event is blocked from acting on the indicated element. If False, the event is executed against the element. If True, the event is blocked and can be later replayed within the activity handler, by using the ReplayUserEvent activity. |
| `IncludeChildren` | Include children | InArgument | `bool` |  |  | When selected, the children of the specified UI element are also monitored. By default, this check box is selected. |
| `Mode` | Trigger mode | InArgument | [`NClickTriggerMode`](#nclicktriggermode) |  |  | Specifies if the event is triggered on key down or key up. |
| `SchedulingMode` | Scheduling mode | Property | [`TriggerActionSchedulingMode`](common/TriggerActionSchedulingMode.md) |  |  | It specifies how to execute the actions when a trigger is fired. Sequential: actions are executed one after another; Concurrent: actions execution can overlap; OneTime: executes one action and exits monitoring. For Sequential and Concurrent modes the monitoring continues until either the user stops the execution or a Break activity is met. |

## Enums

### NClickTriggerMode

`UiPath.UIAutomationNext.Triggers.NClickTriggerMode`

The mouse-button event a click trigger fires on.

| Value | Description |
|-------|-------------|
| `NClickTriggerMode.Down` | Triggered on mouse button press. |
| `NClickTriggerMode.Up` | Triggered on mouse button release. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NClickTrigger`.

## Notes

- This activity requires a parent `Use Application/Browser` scope.
- Use the `Mouse button` property to specify which mouse button click to monitor.
- The `Block event` option prevents the click from reaching the target element, allowing custom handling via `ReplayUserEvent`.
- The `Scheduling mode` controls how multiple trigger firings are handled (sequentially, concurrently, or one-time).
- When `Include children` is enabled, clicks on child elements of the target are also monitored.
