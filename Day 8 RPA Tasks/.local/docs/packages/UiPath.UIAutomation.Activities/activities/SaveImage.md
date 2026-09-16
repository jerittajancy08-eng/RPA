# Save Image

Saves the specified image to a file.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NSaveImage`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Image` | Image | InArgument | `Image` | **Yes** |  |  | The image to save to a file. |
| `FileName` | File name | InArgument | `string` | **Yes** |  |  | The name of the file where the image will be saved. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `OutFile` | Saved file | `ILocalResource` | The saved file, as a local resource pointing to the written path. |

### Common

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` | Continue executing the activities in the automation if this activity fails. The default value is False. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NSaveImage`.

## Notes

- No mandatory parent scope is required for this activity, and it does not target a UI element.
- `Image` and `FileName` are both mandatory — leaving either empty produces a design-time validation error and a runtime exception.
- The usual `Image` source is the `OutImage` output of [Take Screenshot](TakeScreenshot.md), or any other activity producing a `UiPath.Core.Image`.
- A relative `FileName` is resolved against the process working directory; the resulting full path is what `OutFile` points to.
- The file format follows the **image**, not the file extension: JPEG images are written as JPEG, everything else as PNG.
- An existing file with the same name is overwritten. There is no auto-increment mode — use [Take Screenshot](TakeScreenshot.md) with `FileNameMode` if you need one.
