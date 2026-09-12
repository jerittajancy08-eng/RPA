# Close Popup

Dismisses all popups that are on top of the application and block a target, using the configured close buttons.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Semantic
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NClosePopup`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `PopupException` | Popup detected exception | InArgument | `Exception` |  |  |  | The exception thrown when detecting a popup. |

### Configuration

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `PreferredButtons` | Popup close buttons | InArgument | `string[]` | The labels of the buttons used to close the popup. |
| `EnableAI` | AI-Enhanced mode | Property | `bool` | Leverage AI to close the popup. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `PopupHandled` | Popup handled | [`NPopupHandleState`](#npopuphandlestate) | Indicate if a popup was handled or not. |

### Common

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `PopupAppearTimeout` | Popup Appear Timeout | InArgument | `double` | The amount of time (in seconds) to wait for a popup to appear. The default amount of time is 30 seconds. |
| `DelayAfter` | Delay after | InArgument | `double` | Delay (in seconds) after this activity is completed, before next activity starts. The default amount of time is 0.3 seconds. |
| `DelayBefore` | Delay before | InArgument | `double` | Delay (in seconds) to wait before executing this activity. The default amount of time is 0.2 seconds. |

## Enums

### NPopupHandleState

`UiPath.UIAutomationNext.Enums.NPopupHandleState`

The result of an automatic popup-handling attempt by **Close Popup**.

| Value | Description |
|-------|-------------|
| `NPopupHandleState.NotFound` | Status returned when we could not find a popup to handle. |
| `NPopupHandleState.Handled` | Status returned when we managed to close a popup. |
| `NPopupHandleState.NotHandled` | Status returned when we did not manage to close a popup. |
| `NPopupHandleState.NotMatched` | Status returned when we found target buttons but none matched the configured texts. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NClosePopup`.

## Notes

- This activity must be placed inside a **Use Application/Browser** (`NApplicationCard`) scope, **unless** the `PopupException` property is set — an incoming exception already carries the popup information, so no scope is needed to find the application. With neither, validation fails with `"Activity must be placed inside a 'Use Application/Browser' activity or have its 'Input' property set."`
- The `PreferredButtons` property accepts an array of button label strings that the activity will try to click to dismiss the popup.
- Enable `EnableAI` to leverage AI-based detection for identifying and closing popups.
- The `PopupHandled` output indicates whether a popup was successfully detected and dismissed.
- Use `PopupException` to pass in an exception from a previous activity that failed due to a popup blocking the target.
