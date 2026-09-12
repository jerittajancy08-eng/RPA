# NChildInteractionMode

`UiPath.UIAutomationNext.Enums.NChildInteractionMode`

The interaction method used by activities placed inside a **Use Application/Browser** scope. Adds the `SameAsCard` option (compared to [`NInteractionMode`](NInteractionMode.md)) so child activities can inherit the parent card's setting.

## Values

| Value | Description |
|-------|-------------|
| `NChildInteractionMode.SameAsCard` | Use the InputMode setting from the parent Application/Browser activity. |
| `NChildInteractionMode.HardwareEvents` | Acts like a normal user using "hardware" such as a mouse or a keyboard to interact with applications. Hardware events are triggered which are sent to the operating system. This option emulates human behavior 100%. However some events might be lost. |
| `NChildInteractionMode.Simulate` | Simulate using accessibility APIs. Recommended for browsers, Java based apps, SAP. Usually more reliable than Hardware Events. Sends all text in one go. Works even if target app is not in focus. |
| `NChildInteractionMode.DebuggerApi` | Performs actions using debugger APIs. Works for Chrome and Edge elements only. Sends all text in one go. Works if the target app is not in focus. |
| `NChildInteractionMode.WindowMessages` | **[Windows Only]** Simulate using Win32 messages. Recommended for desktop apps. Usually more reliable than Hardware Events. Sends all text in one go. Works even if target app is not in focus. |

## Usage

Reference values as `NChildInteractionMode.<Value>`, e.g. `NChildInteractionMode.SameAsCard`.
