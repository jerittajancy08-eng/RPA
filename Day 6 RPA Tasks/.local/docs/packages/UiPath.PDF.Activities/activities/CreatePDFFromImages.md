# Create PDF from Images

`UiPath.PDF.Activities.PDF.CreatePDFFromImages`

Creates a single PDF document from an array of images, one image per page. Each page is sized to its source image's natural dimensions at 96 DPI. Supports two input modes: providing files as a collection (array of paths or resource array) or providing files individually one by one. The new file is written to `OutputFileName` and returned via `OutputFile`. Cross-platform (Windows and Linux).

**Package:** `UiPath.PDF.Activities`
**Category:** App Integration > PDF

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `FileList` | Images (local paths) | InArgument | `string[]` | Yes† | | | An array of local image file paths. Visible only when `InputMode` is `Collection`. Mutually exclusive with `ResourceFileList`. Supported extensions: `.png`, `.jpg`, `.jpeg`, `.jpe`, `.tif`, `.tiff`, `.bmp`. |
| `ResourceFileList` | Images | InArgument | `IResource[]` | Yes† | | | An array of image file resources. Visible only when `InputMode` is `Collection`. Mutually exclusive with `FileList`. |
| `IndividualResourceFileList` | Images | Property | `IEnumerable<InArgument<IResource>>` | | | | Individual image resource arguments added one by one. Visible only when `InputMode` is `Individual`. |
| `OutputFileName` | Output file path | InArgument | `string` | | | | Full path where the new PDF will be saved (must end with `.pdf`). If not specified, a default `<guid>.pdf` filename is generated automatically. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `InputMode` | Input Mode | `ItemsInputMode` | `Collection` | Controls how input images are specified. `Collection` uses `FileList`/`ResourceFileList`; `Individual` uses `IndividualResourceFileList`. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `OutputFile` | Output file | `ILocalResource` | The generated PDF file as a local resource. |

## Conditional Properties

These properties appear or change behavior based on other property values:

- **`FileList`** (InArgument\<string[]\>) — Visible only when `InputMode` is `Collection`. Provide either `FileList` or `ResourceFileList`, not both.
- **`ResourceFileList`** (InArgument\<IResource[]\>) — Visible only when `InputMode` is `Collection`. Provide either `ResourceFileList` or `FileList`, not both.
- **`IndividualResourceFileList`** (IEnumerable\<InArgument\<IResource\>\>) — Visible only when `InputMode` is `Individual`.

## Enum Reference

**`ItemsInputMode`**: `Collection` (Use a collection of image files), `Individual` (Use individual image files)

## Notes

- At least one image must be provided regardless of input mode.
- Supported image formats: PNG, JPG, JPEG, JPE, TIF, TIFF, BMP. **GIF and other formats are explicitly rejected** at runtime.
- Each output page is sized to its source image's natural pixel dimensions interpreted at 96 DPI (e.g., a 1920×1080 image produces a 1440pt × 810pt page = 20" × 11.25").
- Multi-frame TIF/TIFF files are split into their individual frames, and each frame becomes its own page (a 3-frame TIFF contributes 3 pages). Frames are transcoded to PNG when embedded.
- Non-TIFF image bytes are embedded as-is — JPEGs remain JPEGs in the output PDF, with no extra re-encoding.
- When `InputMode` is `Collection`, setting both `FileList` and `ResourceFileList` is a validation error — use one or the other.
- Output folder is auto-created if it doesn't exist; default output goes to the working directory.
- Cross-platform: runs on both Windows and Linux (Studio Desktop and Studio Web).

## XAML Example

### Collection mode (local paths)

```xml
<pdf:CreatePDFFromImages
    DisplayName="Create PDF from Images"
    InputMode="Collection"
    FileList="[new String() {&quot;C:\photos\page1.png&quot;, &quot;C:\photos\page2.jpg&quot;, &quot;C:\photos\page3.tif&quot;}]"
    OutputFileName="[outputFilePath]"
    OutputFile="[createdFile]" />
```

### Collection mode (file resources)

```xml
<pdf:CreatePDFFromImages
    DisplayName="Create PDF from Images"
    InputMode="Collection"
    ResourceFileList="[imageResourceArray]"
    OutputFileName="[outputFilePath]"
    OutputFile="[createdFile]" />
```

> Namespace prefix `pdf` maps to `clr-namespace:UiPath.PDF.Activities.PDF;assembly=UiPath.PDF.Activities`.
