# Use Application/Browser

A container activity that opens/attaches to a desktop application or web browser page and perform actions within it.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `TargetApp` | Target application | Property | [`TargetApp`](common/Target.md#targetapp) |  |  |  | The target application to open/attach to. Supplies the app identity — executable path / app id / URL and startup arguments. |
| `OpenMode` | Open | InArgument | [`NAppOpenMode`](common/NAppOpenMode.md) |  |  |  | Defines whether to open the target application before executing the activities in it. |
| `CloseMode` | Close | InArgument | [`NAppCloseMode`](common/NAppCloseMode.md) |  |  |  | Defines whether to close the target application after executing the activities in it. The default value is same as 'If opened by Use App/Browser'. |
| `UserDataFolderMode` | User data folder mode | InArgument | [`BrowserUserDataFolderMode`](common/BrowserUserDataFolderMode.md) |  |  |  | The user data folder mode you want to set. It is used for starting the browser with a specific user data folder. |
| `UserDataFolderPath` | User data folder path | InArgument | `string` |  |  |  | The user data folder that the browser will use. Defaults to "%localappdata%\UiPath\PIP Browser Profiles\BrowserType" if not set. |
| `IsIncognito` | Incognito/private window | InArgument | `bool` |  |  |  | Opens the new browser session in Incognito/private mode. |
| `WebDriverMode` | Browser automation mode | InArgument | [`NWebDriverMode`](common/NWebDriverMode.md) |  |  |  | Specifies the automation method used when opening a new browser session. |
| `HealingAgentBehavior` | Healing Agent mode | InArgument | [`NHealingAgentBehavior`](common/NHealingAgentBehavior.md) |  |  |  | Configures the Healing Agent actions if they are allowed by Governance or Orchestrator process/job/trigger level settings |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `InteractionMode` | Input mode | Property | [`NInteractionMode`](common/NInteractionMode.md) | `Constants.DefaultInteractionMode` |  | The method used to generate keyboard and mouse input. |
| `AttachMode` | Window attach mode | Property | [`NAppAttachMode`](common/NAppAttachMode.md) | `NAppAttachMode.ByInstance` |  | Defines where inner activities search for their target elements. |
| `WindowResize` | Resize window | Property | [`NWindowResize`](common/NWindowResize.md) | `NWindowResize.None` |  | Defines whether the application/browser is resized when initialized. |
| `DialogHandling` | Dialog Handling | Property | [`DialogHandling`](common/DialogHandling.md) |  |  | Configure auto-dismissal of JavaScript dialogs. |
| `IsExactTitleEnabled` | Match exact title | Property | `bool` |  |  | When ON, only apps that exactly match the current app title will be used in the automation. When OFF, the window with the closest matching title will be used in the automation. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` |  | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `Timeout` | Timeout | InArgument | `double` |  | The amount of time (in seconds) to wait for the operation to be performed before generating an error. The default value is 30 seconds. |

## Sub-Objects

See [`DialogHandling`](common/DialogHandling.md) for the full property table and XAML nested-element syntax.

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NApplicationCard`.

## Notes

- This activity is a scope/container activity. Place child activities (Click, Type Into, etc.) inside the body.
- The `Version` attribute is mandatory and must be set to `V2`.
