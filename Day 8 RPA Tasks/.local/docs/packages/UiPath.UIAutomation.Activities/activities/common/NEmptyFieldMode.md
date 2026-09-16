# NEmptyFieldMode

`UiPath.UIAutomationNext.Enums.NEmptyFieldMode`

How to clear the existing content of a text field before typing.

## Values

| Value | Description |
|-------|-------------|
| `NEmptyFieldMode.None` | Do not empty the field. |
| `NEmptyFieldMode.SingleLine` | Clears the existing content using the `[End, Shift+Home, Del]` keyboard sequence. |
| `NEmptyFieldMode.MultiLine` | Clears the existing content using the `[Ctrl+A, Del]` keyboard sequence. |

## Usage

Reference values as `NEmptyFieldMode.<Value>`, e.g. `NEmptyFieldMode.SingleLine`.
