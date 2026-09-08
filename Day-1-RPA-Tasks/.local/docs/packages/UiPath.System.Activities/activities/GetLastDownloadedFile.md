# Wait for Download

`UiPath.Core.Activities.GetLastDownloadedFile`

Detects a file download from any application and waits for the download to complete before any further processing of the file in the automation.

**Package:** `UiPath.System.Activities`
**Category:** System > File
**Platform:** Windows only

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `DownloadFolder` | Downloads folder | `InArgument` | `string` | Yes | `GetDownloadsFolder()` | Path of the folder to monitor for new downloads. Defaults to the system Downloads folder. |
| `Timeout` | Timeout | `InArgument` | `int` | No | — | Maximum time in milliseconds to wait for a download to complete before raising an error. |
| `IgnoreFiles` | Ignore file extensions | `InArgument` | `string` | No | — | Comma-separated list of file extensions to ignore while waiting for the download to complete, entered **without** the leading dot (e.g. `crdownload, tmp`). By default, browser partial-download files are ignored (`crdownload`, `partial`, `part`, `opdownload`). Providing a value replaces these defaults, so include them if you want them kept. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `Body` | Body | `ActivityAction` | — | Optional container for activities to execute while monitoring for the download (e.g. clicking a download button). Rendered as a visual container in Studio when supported. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `File` | Downloaded file | `OutArgument` | `FileInfo` | `FileInfo` object for the downloaded file. |
| `FileResource` | File Resource | `OutArgument` | `ILocalResource` | `ILocalResource` reference to the downloaded file. |

## XAML Example

```xml
<ui:GetLastDownloadedFile
    DisplayName="Wait for Download"
    DownloadFolder="C:\Users\user\Downloads"
    Timeout="30000"
    File="[downloadedFileInfo]"
    FileResource="[downloadedResource]">
  <ui:GetLastDownloadedFile.Body>
    <ActivityAction>
      <!-- Place activities that trigger the download here -->
    </ActivityAction>
  </ui:GetLastDownloadedFile.Body>
</ui:GetLastDownloadedFile>
```

`xmlns:ui="clr-namespace:UiPath.Core.Activities;assembly=UiPath.System.Activities"`

## Notes

- Place the activity that triggers the file download (e.g. a **Click** activity on a download button) inside the `Body` container so the monitoring starts before the download is initiated.
- The activity monitors the `DownloadFolder` for new files; it waits until an in-progress download is fully completed. Browser partial-download files are ignored by default (`.crdownload`, `.partial`, `.part`, `.opdownload`), including Chrome's newer `Unconfirmed N.crdownload.<id>.<ext>` naming.
- Use `IgnoreFiles` to filter out temporary files created by browsers during the download process. Enter extensions **without** the leading dot (e.g. `crdownload, tmp`). Note that supplying a value **replaces** the default set above — include those extensions if you still want them ignored.
- Both `File` (legacy) and `FileResource` (recommended) outputs reference the same downloaded file.
