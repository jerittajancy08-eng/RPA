# NExecutionWorld

`UiPath.UIAutomationNext.Enums.NExecutionWorld`

The JavaScript execution world used by **Inject Js Script**.

## Values

| Value | Description |
|-------|-------------|
| `NExecutionWorld.Isolated` | Execute the script in an isolated environment. Allows access to the HTML elements, but prevents access to page variables and code. |
| `NExecutionWorld.Page` | Execute the script in the page environment. Allows access to the HTML elements, page variables and code. Use this option if you need to access page variables (e.g. jQuery `$`) or interact with page code (e.g. `window.alert`). |

## Usage

Reference values as `NExecutionWorld.<Value>`, e.g. `NExecutionWorld.Isolated`.
