# NInteractionMode

`UiPath.UIAutomationNext.Enums.NInteractionMode`

The method used to interact with UI elements. Used by **Use Application/Browser** to set the default input mode for child activities.

For child activities (Click, Type Into, etc.), see [`NChildInteractionMode`](NChildInteractionMode.md), which adds a `SameAsCard` value to inherit from the parent card.

## Values

| Value | Description |
|-------|-------------|
| `NInteractionMode.HardwareEvents` | Acts like a normal user using "hardware" such as a mouse or a keyboard to interact with applications. Hardware events are triggered which are sent to the operating system. This option emulates human behavior 100%. However some events might be lost. |
| `NInteractionMode.Simulate` | Simulate using accessibility APIs. Recommended for browsers, Java based apps, SAP. Usually more reliable than Hardware Events. Sends all text in one go. Works even if target app is not in focus. |
| `NInteractionMode.DebuggerApi` | Performs actions using debugger APIs. Works for Chrome and Edge elements only. Sends all text in one go. Works if the target app is not in focus. |
| `NInteractionMode.WindowMessages` | **[Windows Only]** Simulate using Win32 messages. Recommended for desktop apps. Usually more reliable than Hardware Events. Sends all text in one go. Works even if target app is not in focus. |
| `NInteractionMode.Background` | Run in the background. Tries to use Simulate where possible, while complex activities (image, native text) will run as usual (in the foreground). |

## Usage

Reference values as `NInteractionMode.<Value>`, e.g. `NInteractionMode.Simulate`.
