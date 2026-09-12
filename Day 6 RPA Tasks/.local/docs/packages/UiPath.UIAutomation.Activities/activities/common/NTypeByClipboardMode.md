# NTypeByClipboardMode

`UiPath.UIAutomationNext.Enums.NTypeByClipboardMode`

Whether the clipboard is used for typing the given text.

## Values

| Value | Description |
|-------|-------------|
| `NTypeByClipboardMode.Never` | Never use the clipboard. |
| `NTypeByClipboardMode.Always` | Always use the clipboard. |
| `NTypeByClipboardMode.WhenPossible` | Use the clipboard when possible. This depends on the OS and the text to be typed (e.g. if any special key is used, then the clipboard will not be used). |

## Usage

Reference values as `NTypeByClipboardMode.<Value>`, e.g. `NTypeByClipboardMode.WhenPossible`.
