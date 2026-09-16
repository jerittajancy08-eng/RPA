# CursorMotionType

`UiPath.UIAutomationNext.Enums.CursorMotionType`

Specifies the type of motion performed by the mouse cursor. Has effect only when input method **Hardware Events** is used.

## Values

| Value | Description |
|-------|-------------|
| `CursorMotionType.Instant` | The cursor jumps to the destination. By default, Instant is selected. |
| `CursorMotionType.Smooth` | The cursor moves in increments. Has no effect if `SendWindowMessages` or `SimulateClick` are enabled. |

## Usage

Reference values as `CursorMotionType.<Value>`, e.g. `CursorMotionType.Instant`.
