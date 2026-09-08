# Upload Storage File

`UiPath.Core.Activities.Storage.UploadStorageFile`

Uploads a local file to the Orchestrator storage, in a certain bucket and at a certain path.

**Package:** `UiPath.System.Activities`
**Category:** Storage

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `StorageBucketName` | Storage bucket name | InArgument | `string` | Yes (inherited) | — | The name of the Orchestrator storage bucket. Inherited from the storage activity base class — declared on the activity element, not as a child binding. |
| `FolderPath` | Folder path | InArgument | `string` | No (inherited) | — | The Orchestrator folder containing the storage bucket. Inherited from the storage activity base class. Leave empty to use the robot's current folder. |
| `FileResource` | File Resource | InArgument | `IResource` | Yes (File overload) | — | A file resource object representing the local file to upload. Use this or `Path`, not both. |
| `Path` | Path | InArgument | `string` | Yes (Path overload) | — | The local file system path of the file to upload. Use this or `FileResource`, not both. |
| `Destination` | Destination | InArgument | `string` | No | — | The target path within the storage bucket. A value ending in `/` or `\` (e.g., `reports/2024/`) is a directory prefix the file name is appended to; a value without a trailing separator is used as the full upload path unless `FileName` is also set, in which case it is treated as a directory. Leave empty to upload to the bucket root. |
| `FileName` | File Name | InArgument | `string` | No | — | The name to use for the file in the storage bucket. If omitted, the name is inferred at runtime from `FileResource` or `Path`. |

## Valid Configurations

`FileResource` and `Path` are mutually exclusive overload groups:

| Configuration | Required properties | Optional properties |
|---|---|---|
| **File** | `FileResource` | `Destination`, `FileName` |
| **Path** | `Path` | `Destination`, `FileName` |

## XAML Example

```xml
<!-- Upload using a local file path -->
<ui:UploadStorageFile
    xmlns:ui="clr-namespace:UiPath.Core.Activities.Storage;assembly=UiPath.System.Activities"
    DisplayName="Upload Storage File"
    Path="[&quot;C:\Temp\report.pdf&quot;]"
    Destination="[&quot;reports/2024&quot;]"
    FileName="[&quot;report.pdf&quot;]" />
```

```xml
<!-- Upload using a file resource -->
<ui:UploadStorageFile
    xmlns:ui="clr-namespace:UiPath.Core.Activities.Storage;assembly=UiPath.System.Activities"
    DisplayName="Upload Storage File"
    FileResource="[myFileResource]"
    Destination="[&quot;reports/2024&quot;]" />
```

```xml
<!-- Upload to the bucket root: no Destination, file name inferred from the source path -->
<ui:UploadStorageFile
    xmlns:ui="clr-namespace:UiPath.Core.Activities.Storage;assembly=UiPath.System.Activities"
    DisplayName="Upload Storage File"
    Path="[&quot;C:\Temp\report.pdf&quot;]" />
```

## Notes

- Requires an active Orchestrator connection with access to storage buckets.
- The storage bucket name and folder path are configured in the Orchestrator connection context (via `StorageBucketName` and `FolderPath` inherited from the base storage activity class).
- The upload path is composed at runtime from `Destination`, `FileName`, and the source file:
  - Empty `Destination`: the file is uploaded to the bucket root, named by `FileName` or, if that is empty, by the name inferred from `FileResource`/`Path`.
  - `Destination` ending in `/` or `\` marks a directory prefix; the file name (`FileName`, or inferred from the source) is appended with `/`. A bare separator (`/` or `\`) means the bucket root.
  - `Destination` without a trailing separator: used verbatim as the full upload path when `FileName` is empty, or treated as a directory that `FileName` is appended to.
- Legacy workflows where `Destination` includes the file name keep working unchanged: the value is used as the full upload path at runtime; the XAML is not rewritten.
- If a file with the same name already exists at the destination path in the bucket, it is overwritten.
