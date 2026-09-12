# Set Runtime Browser

Sets the currently active runtime browser.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Browser
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NSetRuntimeBrowser`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `BrowserType` | Browser type | InArgument | [`NBrowserType`](common/NBrowserType.md) |  |  |  | Choose the type of browser you want to use. The following options are available: Chrome, Edge, Firefox, None. When converting from a String value, you can use NBrowserTypeFactory.From(String) helper method. |

### Common

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` | Continue executing the activities in the automation if this activity fails. The default value is False. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NSetRuntimeBrowser`.

## Notes

- No mandatory parent scope is required for this activity.
- Use this activity to switch the active browser type at runtime (e.g., Chrome, Edge, Firefox).
- The `NBrowserType.None` option can be used to clear the runtime browser setting.
- When converting from a string value, use the `NBrowserTypeFactory.From(String)` helper method.
