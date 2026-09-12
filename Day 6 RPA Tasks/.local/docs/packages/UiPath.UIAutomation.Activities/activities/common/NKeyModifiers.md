# NKeyModifiers

`UiPath.UIAutomationNext.Enums.NKeyModifiers`

Key modifiers used in combination with click and keyboard activities. This is a `[Flags]` enum — values can be combined using bitwise OR.

## Values

| Value | Description |
|-------|-------------|
| `NKeyModifiers.None` | No modifier. |
| `NKeyModifiers.Alt` | Alt key. |
| `NKeyModifiers.Ctrl` | Ctrl key. |
| `NKeyModifiers.Shift` | Shift key. |
| `NKeyModifiers.Win` | Windows key. |

## Usage

Reference values as `NKeyModifiers.<Value>`, e.g. `NKeyModifiers.Ctrl`. Combine flags with bitwise OR: `NKeyModifiers.Ctrl | NKeyModifiers.Shift`.
