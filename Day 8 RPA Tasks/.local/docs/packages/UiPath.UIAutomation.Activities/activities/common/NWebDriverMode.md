# NWebDriverMode

`UiPath.UIAutomationNext.Enums.NWebDriverMode`

The browser automation mode used by **Use Application/Browser** for web targets.

## Values

| Value | Description |
|-------|-------------|
| `NWebDriverMode.Disabled` | Uses the UiPath browser extension for automation. Requires the extension to be installed in the target browser. |
| `NWebDriverMode.WithGUI` | Opens the browser session using WebDriver. |
| `NWebDriverMode.Headless` | Opens the browser session using WebDriver in headless mode (without creating a visual browser window). |
| `NWebDriverMode.DevTools` | Uses DevTools protocol for browser automation. Works with all Chromium-based browsers (Chrome, Edge, etc.) without requiring any additional configuration — no extension installation or WebDriver download needed. |
| `NWebDriverMode.DevToolsHeadless` | Uses DevTools protocol for browser automation in headless mode (without creating a visual browser window). Works with all Chromium-based browsers (Chrome, Edge, etc.) without requiring any additional configuration — no extension installation or WebDriver download needed. |

## Usage

Reference values as `NWebDriverMode.<Value>`, e.g. `NWebDriverMode.DevTools`.
