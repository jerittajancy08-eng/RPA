# NAppOpenMode

`UiPath.UIAutomationNext.Enums.NAppOpenMode`

When **Use Application/Browser** should open a new application instance or browser window.

## Values

| Value | Description |
|-------|-------------|
| `NAppOpenMode.Never` | Never start/open a new application instance or browser window. |
| `NAppOpenMode.IfNotOpen` | Open a new application instance or browser window, if the selector does not match any existing instance. |
| `NAppOpenMode.Always` | Always open a new application instance or browser window. When opening a new instance, the file path and arguments are used to launch the application. When opening a new browser window, the URL is used to open the browser. |

## Usage

Reference values as `NAppOpenMode.<Value>`, e.g. `NAppOpenMode.IfNotOpen`.
