# NAppCloseMode

`UiPath.UIAutomationNext.Enums.NAppCloseMode`

How **Use Application/Browser** closes the target application or browser when leaving the scope.

## Values

| Value | Description |
|-------|-------------|
| `NAppCloseMode.Never` | Do not close the application instance or browser window after executing the activities in scope, even if it was opened by the Use Application/Browser activity. |
| `NAppCloseMode.IfOpenedByAppBrowser` | Close the application instance or browser window only if it was opened by the Use Application/Browser activity. |
| `NAppCloseMode.Always` | Always close the application instance or browser window after executing the activities in scope. |

## Usage

Reference values as `NAppCloseMode.<Value>`, e.g. `NAppCloseMode.IfOpenedByAppBrowser`.
