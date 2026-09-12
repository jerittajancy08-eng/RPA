# NAppAttachMode

`UiPath.UIAutomationNext.Enums.NAppAttachMode`

How **Use Application/Browser** attaches to a target application instance.

## Values

| Value | Description |
|-------|-------------|
| `NAppAttachMode.ByProcessName` | Inner activities will search in all windows with the same process name as the indicated app. |
| `NAppAttachMode.ByInstance` | Inner activities will search in the indicated application instance, including all parent and child windows (alerts, popups, etc). Other instances of the application are excluded. |
| `NAppAttachMode.SingleWindow` | Inner activities will search only in the indicated window. |

## Usage

Reference values as `NAppAttachMode.<Value>`, e.g. `NAppAttachMode.ByInstance`.
