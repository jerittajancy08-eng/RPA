# Set CV Server

Overrides the Computer Vision server settings (server URL, API key and local-server usage) at runtime. The provided values replace the current configuration entirely, take effect from the moment the activity is executed and take precedence over the Computer Vision project settings.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NSetCVServer`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Server` | Server URL | InArgument | `string` |  |  |  | The Computer Vision server URL to use. Leave empty to configure no remote server. |
| `ApiKey` | API Key | InArgument | `string` |  |  |  | The Computer Vision license API key. Leave empty to use no API key from this point on. |
| `UseLocalServer` | Use Local Server | InArgument | `bool` |  | `False` |  | Whether to use a local Computer Vision edge server instead of the configured server URL. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NSetCVServer`.

## Notes

- No mandatory parent scope is required for this activity, and it does not target a UI element.
- The three properties are applied **as a set**: whatever is left empty is applied as empty. Setting only `Server` clears the API key that was in effect.
- The override wins over the Computer Vision project settings and stays in effect for the lifetime of the current process, or until another **Set CV Server** runs.
- Workflows started with **Invoke Workflow File** in isolated mode inherit the configuration through an environment variable; already-running isolated children keep their old configuration.
- Coded workflow equivalent: `uiAutomation.SetCVServer(server, apiKey, useLocalServer)` — see [coded-api.md](../coded/coded-api.md#setcvserver).
