# Microsoft Azure Computer Vision OCR

Extracts a string and its information from an indicated UI element using the Microsoft Azure Computer Vision OCR engine.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.OCR.Engine
**Assembly:** `UiPath.UiAutomation.Activities`
**Class Name:** `UiPath.Core.Activities.MicrosoftAzureComputerVisionOCR`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `ApiKey` | ApiKey | InArgument | `string` |  |  |  | The API key used to provide you access to the Microsoft Azure Computer Vision OCR. |
| `Endpoint` | Endpoint | InArgument | `string` |  |  |  | The endpoint associated with your Microsoft Azure Computer Vision OCR API key. |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `HandwritingRecognition` | UseReadAPI | Property | `bool` |  |  | If selected, the new Azure Computer Vision API 2.0 with handwriting recognition capabilities is used. If not selected, the standard Azure Computer Vision API for printed text is used. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.Core.Activities.MicrosoftAzureComputerVisionOCR`.

## Notes

- This is an OCR engine activity meant to be used within other OCR activities such as `Click OCR Text`, `Get OCR Text`, or `Find OCR Text Position`.
- A valid Microsoft Azure Computer Vision API key and endpoint are required.
- Enable `UseReadAPI` to use the newer Azure Computer Vision API 2.0 with handwriting recognition support.
- When `UseReadAPI` is disabled, the standard Azure Computer Vision API for printed text is used.
