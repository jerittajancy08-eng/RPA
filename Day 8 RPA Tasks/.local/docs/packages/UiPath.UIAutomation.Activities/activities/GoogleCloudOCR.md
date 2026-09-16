# Google Cloud Vision OCR

Extracts a string and its information from an indicated UI element using the Google Cloud Vision OCR engine. It can be used with other OCR activities (Click OCR Text, Hover OCR Text, Double Click OCR Text, Get OCR Text, Find OCR Text Position).

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.OCR.Engine
**Assembly:** `UiPath.UiAutomation.Activities`
**Class Name:** `UiPath.Core.Activities.GoogleCloudOCR`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `ApiKey` | ApiKey | InArgument | `string` |  |  |  | The API key used to provide you access to the Google Cloud Vision OCR. |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `DetectionMode` | DetectionMode | Property | [`GoogleCloudDetectionMode`](#googleclouddetectionmode) |  |  | Indicates what detection flag to send to the Google Cloud Vision OCR. |
| `Region` | Region | InArgument | [`GoogleCloudRegion`](#googlecloudregion) |  |  | The geographical region for the servers to be used for OCR |
| `ResizeToApiLimits` | ResizeToMaxLimitIfNecessary | Property | `bool` |  |  | When checked, the engine will attempt downsizing the image so as to have an acceptable size according to the Google Cloud Vision imposed limits |

## Enums

### GoogleCloudDetectionMode

`UiPath.Vision.OCR.GoogleCloudDetectionMode`

The detection mode used by Google Cloud OCR.

| Value | Description |
|-------|-------------|
| `GoogleCloudDetectionMode.TextDetection` | Text detection is optimized for areas of text within a larger image. |
| `GoogleCloudDetectionMode.DocumentTextDetection` | Run dense text document OCR. Takes precedence when both `DOCUMENT_TEXT_DETECTION` and `TEXT_DETECTION` are present. |

### GoogleCloudRegion

`UiPath.Vision.OCR.GoogleCloudRegion`

The Google Cloud region used by the Google Cloud OCR activity.

| Value | Description |
|-------|-------------|
| `GoogleCloudRegion.Global` | Global endpoint. |
| `GoogleCloudRegion.EU` | EU regional endpoint. |
| `GoogleCloudRegion.US` | US regional endpoint. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.Core.Activities.GoogleCloudOCR`.

## Notes

- This is an OCR engine activity meant to be used within other OCR activities such as `Click OCR Text`, `Get OCR Text`, or `Find OCR Text Position`.
- A valid Google Cloud Vision API key is required.
- The `DetectionMode` property configures the type of text detection performed by Google Cloud Vision.
- The `Region` property allows you to specify which geographic region's servers to use for OCR processing.
- Enable `ResizeToMaxLimitIfNecessary` to automatically downsize large images to meet Google Cloud Vision API size limits.
