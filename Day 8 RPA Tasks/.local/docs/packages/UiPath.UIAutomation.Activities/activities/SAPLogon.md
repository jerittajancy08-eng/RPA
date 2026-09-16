# SAP Logon

Use the activity to directly log on to an SAP system. Provide the exact SAP connection name from the SAP Logon or the SAP Logon Pad window used to log on to your SAP system.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.SAP
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NSAPLogon`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `TargetApp` | Target application | Property | `TargetApp` |  |  |  | Expand for more options. |

**The SAP connection name is set in `TargetApp.Arguments`.** The activity surfaces this `TargetApp` field in the designer as **Connection Name**. At runtime the value is read from `TargetApp.Arguments` and used to open that SAP connection.

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `Retries` | Number Of Retries | InArgument | `int` |  |  | The number of times that the activity is trying to connect to SAP Scripting interface. (Default: 5) |
| `DelayBetweenRetries` | Retry Interval | InArgument | `double` |  |  | Specifies the amount of time (in seconds) between each retry. The default amount of time is 0.5 seconds. |
| `InteractionMode` | Input mode | Property | [`NInteractionMode`](common/NInteractionMode.md) | `Constants.DefaultInteractionMode` |  | The method used to generate keyboard and mouse input. |
| `AttachMode` | Window attach mode | Property | [`NAppAttachMode`](common/NAppAttachMode.md) | `NAppAttachMode.ByInstance` |  | Defines where inner activities search for their target elements. |
| `OpenMode` | Open | InArgument | [`NAppOpenMode`](common/NAppOpenMode.md) |  |  | Defines whether to open the target application before executing the activities in it. |
| `CloseMode` | Close | InArgument | [`NAppCloseMode`](common/NAppCloseMode.md) |  |  | Defines whether to close the target application after executing the activities in it. The default value is same as 'If opened by Use App/Browser'. |
| `WindowResize` | Resize window | Property | [`NWindowResize`](common/NWindowResize.md) | `NWindowResize.None` |  | Defines whether the application/browser is resized when initialized. |
| `DialogHandling` | Dialog Handling | Property | [`DialogHandling`](common/DialogHandling.md) |  |  | Configure auto-dismissal of JavaScript dialogs. |
| `HealingAgentBehavior` | Healing Agent mode | InArgument | [`NHealingAgentBehavior`](common/NHealingAgentBehavior.md) |  |  | Configures the Healing Agent actions if they are allowed by Governance or Orchestrator process/job/trigger level settings |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |

### Common

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` |  |  | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `Timeout` | Timeout | InArgument | `double` |  |  | The amount of time (in seconds) to wait for the operation to be performed before generating an error. The default value is 30 seconds. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NSAPLogon`.

## Notes

- Before automating SAP, read [`sap-guide.md`](../references/sap-guide.md).
- This activity acts as a scope (similar to `Use Application/Browser`) specifically for SAP systems.
- Provide the exact SAP connection name from the SAP Logon or SAP Logon Pad window.
- The `Number Of Retries` and `Retry Interval` properties control the retry behavior when connecting to the SAP Scripting interface.
- Inner activities such as Click, Type Into, and Get Text can be placed inside this scope to interact with the SAP GUI.
