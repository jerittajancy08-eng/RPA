# Insert Image Workbook

`UiPath.Excel.Activities.InsertImage`

Inserts an image into a workbook sheet, anchored at a cell.

**Package:** `UiPath.Excel.Activities`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `WorkbookPath` | File (local path) | InArgument | `string` | | | The full local path of the workbook. Alternative to `WorkbookPathResource` — use one or the other |
| `WorkbookPathResource` | File | InArgument | `IResource` | | | A resource reference (e.g. a local file, remote file, or abstract resource). Alternative to `WorkbookPath` — use one or the other |
| `ImagePath` | Image (local path) | InArgument | `string` | Yes* | | Local path of the image to insert. Alternative to `ImagePathResource` — use one or the other |
| `ImagePathResource` | Image file | InArgument | `IResource` | Yes* | | A resource reference to the image. Alternative to `ImagePath` — use one or the other |
| `SheetName` | SheetName | InArgument | `string` | Yes | `"Sheet1"` | The sheet to insert the image into. Created automatically if it doesn't exist |
| `Cell` | Cell | InArgument | `string` | Yes | `"A1"` | Anchor cell for the top-left corner of the image |
| `Left` | Image left | InArgument | `int` | Yes | `0` | Horizontal pixel offset from the anchor cell. Must be non-negative |
| `Top` | Image top | InArgument | `int` | Yes | `0` | Vertical pixel offset from the anchor cell. Must be non-negative |
| `Width` | Image width | InArgument | `int?` | | | Image width in pixels. If unset or `0`, scales proportionally from `Height`, or keeps the original size if `Height` is also unset/`0`. Must be non-negative |
| `Height` | Image height | InArgument | `int?` | | | Image height in pixels. If unset or `0`, scales proportionally from `Width`, or keeps the original size if `Width` is also unset/`0`. Must be non-negative |

\* `ImagePath` and `ImagePathResource` are mutually exclusive — exactly one must be set. This is validated at design time (`CacheMetadata`).

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `Password` | Password | `InArgument<string>` | | The password of the workbook, if necessary |
| `Workbook` | Workbook | `InArgument<Workbook>` | | Existing workbook object to use instead of a file path |

## Valid Configurations

**Image source** (mutually exclusive, validated at design time):
- **Local path**: set `ImagePath`.
- **Resource**: set `ImagePathResource`.

**Workbook source** (mutually exclusive):
- **Local path**: set `WorkbookPath`.
- **Resource**: set `WorkbookPathResource`.

### Sizing behavior (`Width` / `Height`)

- **Both unset or `0`** → image keeps its original dimensions.
- **Only `Width` set** → `Height` scales proportionally to preserve aspect ratio.
- **Only `Height` set** → `Width` scales proportionally to preserve aspect ratio.
- **Both set** → exact dimensions, aspect ratio not preserved.

## XAML Example

```xml
<excel:InsertImage
  DisplayName="Insert Image Workbook"
  WorkbookPath="[&quot;report.xlsx&quot;]"
  ImagePath="[&quot;logo.png&quot;]"
  SheetName="[&quot;Sheet1&quot;]"
  Cell="[&quot;B2&quot;]"
  Left="[0]"
  Top="[0]"
  Width="[200]"
  Height="[100]" />
```

## Notes

- Uses the file-based workbook engine (not Excel Interop). Works cross-platform.
- `.xlsx` only — `.xls` workbooks throw `NotSupportedException`.
- Supported image formats: PNG, JPEG, GIF, BMP, TIFF, WebP, PCX.
- `WorkbookPath` / `WorkbookPathResource` are mutually exclusive, as are `ImagePath` / `ImagePathResource`.
- `Left`, `Top`, `Width`, and `Height` must be non-negative; negative values throw `ArgumentOutOfRangeException`.
