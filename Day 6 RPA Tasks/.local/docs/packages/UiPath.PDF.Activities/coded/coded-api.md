# UiPath PDF Activities — Coded Workflow API

`UiPath.PDF.Activities`

Provides PDF and XPS document operations for coded workflows: read text (with or without OCR), get page counts, join, extract page ranges, extract images, export pages as images, manage passwords, and convert HTML / plain text / email messages to PDF.

**Service accessor:** `pdf` (type `IPdfService`)
**Required package:** `"UiPath.PDF.Activities": "[{Version}]"` in `project.json` dependencies.

## Auto-Imported Namespaces

The coded-workflow runtime auto-imports the following namespaces when the `UiPath.PDF.Activities` package is referenced. You do **not** need `using` directives for the types they contain:

```
UiPath.PDF.Activities.Api
UiPath.PDF
UiPath.PDF.Activities.PDF.Enums
UiPath.Platform.ResourceHandling
System.Net.Mail
```

The `pdf` accessor is auto-injected by the coded-workflow runtime — just call `pdf.<Method>(…)`.

Per-type → namespace lookup (every type below except `IOCRActivity` lives in one of the auto-imported namespaces above):

| Type | Namespace |
|---|---|
| `ConvertToPdfOptions` | `UiPath.PDF.Activities.Api` |
| `PaperSize` | `UiPath.PDF.Activities.Api` |
| `ImageDpi`, `ImageExtension` | `UiPath.PDF` |
| `TextAlignment` | `UiPath.PDF.Activities.PDF.Enums` |
| `IResource`, `ILocalResource` | `UiPath.Platform.ResourceHandling` |
| `MailMessage` | `System.Net.Mail` |
| `IOCRActivity` | `UiPath.OCR.Contracts.Activities` — ships in the `UiPath.OCR.Activities` package; auto-imported once that package is referenced |

## Service Overview

The `pdf` service follows the **direct service method** pattern: every operation is a single method call that returns the result synchronously. No `IDisposable` wrappers, no `using` blocks — just call the method and use the return value.

Most operations expose two overloads: a `string fileName` form for plain file paths and an `IResource file` form for working with files retrieved via `PathExists` / "Get Local File or Folder" activities. Methods are grouped into four sections below:

- **Read Operations** — extract text from PDFs (native or OCR-driven) and XPS files; get page counts
- **Page-Level Operations** — extract a page range as a new PDF, export a single page as an image, pull all embedded images out of a PDF, extract file attachments embedded in a PDF
- **Combine & Modify** — join multiple PDFs into one; set, change, or remove user/owner passwords
- **Conversion to PDF** — turn HTML, plain text, or `MailMessage` objects into PDF files

Methods that produce a file return an `ILocalResource` — use `.LocalPath` for the absolute path, or pass the resource back into other PDF / OCR / DU activities. Read/inspect methods return `string` or `int` directly.

> **`.LocalPath`, not `.FullName`.** On `ILocalResource`, `FullName` is the file *name* only (`Path.GetFileName(LocalPath)`). Chaining `resource.FullName` into a path-taking method resolves against the robot's current working directory and throws `FileNotFoundException` unless the file happens to live there. Always read the produced file's path from `.LocalPath`.

### Conditional availability

The XPS methods (`ReadXpsText`, `ReadXpsWithOcr`) and `ReadPdfWithOcr` are available on **Windows only**. All other methods are available on both Windows and cross-platform.

---

## Read Operations

### `string ReadPdfText(string fileName, string password = null, string range = "All", bool preserveFormatting = false)`
### `string ReadPdfText(IResource file, string password = null, string range = "All", bool preserveFormatting = false)`

Reads all characters from a PDF file. Two overloads: by path string, or by `IResource` (e.g. returned from a `PathExists` activity).

**Parameters:**
- `fileName` (`string`) — The path of the PDF file to read.
- `file` (`IResource`) — The PDF resource to read.
- `password` (`string`) — Password to open the PDF, if protected (default: `null`).
- `range` (`string`) — Page range to read. Use `"All"` for every page, or comma-separated ranges like `"1-3,5"` (default: `"All"`).
- `preserveFormatting` (`bool`) — If `true`, preserves the visual whitespace and column structure (default: `false`).

**Returns:** `string` — The extracted text. Returns an empty string for pages with no text content.

---

### `string ReadPdfWithOcr(string fileName, IOCRActivity ocrEngine, string range = "All", int degreeOfParallelism = 1, string password = null, ImageDpi imageDpi = ImageDpi.Medium)`

Reads all characters from a PDF file using OCR. Use this for scanned PDFs or PDFs where text is rasterized rather than embedded.

**Parameters:**
- `fileName` (`string`) — The path of the PDF file to read.
- `ocrEngine` (`IOCRActivity`) — The OCR engine to use. Obtain one via the `ocr` service: `ocr.GetUiPathDocumentOcr(apiKey)` or `ocr.GetExtendedLanguagesOcr(apiKey)`. Requires the `UiPath.OCR.Activities` package as a dependency.
- `range` (`string`) — Page range, same syntax as `ReadPdfText` (default: `"All"`).
- `degreeOfParallelism` (`int`) — How many pages to OCR in parallel (default: `1`). Increase for throughput on multi-core hardware, but watch for OCR-engine rate limits.
- `password` (`string`) — PDF password if protected (default: `null`).
- `imageDpi` (`ImageDpi`) — DPI at which pages are rasterized before OCR. Higher DPI improves accuracy but increases processing time and memory use (default: `ImageDpi.Medium` = 150 DPI).

**Returns:** `string` — The OCR-extracted text, concatenated across all processed pages.

---

### `string ReadXpsText(string fileName, string range = "All")`
### `string ReadXpsText(IResource file, string range = "All")`

Reads all characters from an XPS file. This overload has no `password` parameter — use `ReadXpsWithOcr` if the XPS is password-protected.

**Parameters:**
- `fileName` (`string`) — The path of the XPS file.
- `file` (`IResource`) — The XPS resource.
- `range` (`string`) — Page range (default: `"All"`).

**Returns:** `string` — The extracted text.

---

### `string ReadXpsWithOcr(string fileName, IOCRActivity ocrEngine, string range = "All", int degreeOfParallelism = 1, string password = null)`

Reads all characters from an XPS file using OCR. Same OCR-engine sourcing as `ReadPdfWithOcr`.

**Parameters:**
- `fileName` (`string`) — The path of the XPS file.
- `ocrEngine` (`IOCRActivity`) — The OCR engine to use. See `ReadPdfWithOcr` for sourcing.
- `range` (`string`) — Page range (default: `"All"`).
- `degreeOfParallelism` (`int`) — Pages OCRed in parallel (default: `1`).
- `password` (`string`) — Password to open the XPS file, if protected (default: `null`).

**Returns:** `string` — The OCR-extracted text.

---

### `int GetPdfPageCount(string fileName, string password = null)`
### `int GetPdfPageCount(IResource file, string password = null)`

Returns the number of pages in a PDF file.

**Parameters:**
- `fileName` (`string`) — The path of the PDF file.
- `file` (`IResource`) — The PDF resource.
- `password` (`string`) — PDF password if protected (default: `null`).

**Returns:** `int` — The page count (≥ 1 for any valid PDF).

---

## Page-Level Operations

### `ILocalResource ExtractPdfPageRange(string fileName, string range, string password = null, string outputFileName = null)`
### `ILocalResource ExtractPdfPageRange(IResource file, string range, string password = null, string outputFileName = null)`

Creates a new PDF containing only the specified page range.

**Parameters:**
- `fileName` / `file` — Source PDF (path or resource).
- `range` (`string`) — Page range to extract, e.g. `"1-3,5"`. **Required** — no default.
- `password` (`string`) — Source PDF password if protected (default: `null`).
- `outputFileName` (`string`) — Output path. If `null`, an auto-generated filename is used (default: `null`).

**Returns:** `ILocalResource` — Resource pointing to the newly-created PDF. Use `.LocalPath` for the absolute path.

---

### `ILocalResource ExportPdfPageAsImage(string fileName, int pageNumber, string outputFileName, string password = null, ImageDpi imageDpi = ImageDpi.Medium)`
### `ILocalResource ExportPdfPageAsImage(IResource file, int pageNumber, string outputFileName, string password = null, ImageDpi imageDpi = ImageDpi.Medium)`

Renders one page of a PDF as a raster image. Image format is inferred from `outputFileName`'s extension (`.png`, `.jpg`, `.bmp`, etc.).

**Parameters:**
- `fileName` / `file` — Source PDF.
- `pageNumber` (`int`) — **1-based** page index. Passing `0` or a number greater than the page count will throw.
- `outputFileName` (`string`) — Output image path. **Required** — no default.
- `password` (`string`) — PDF password if protected (default: `null`).
- `imageDpi` (`ImageDpi`) — Rendering DPI (default: `ImageDpi.Medium` = 150 DPI).

**Returns:** `ILocalResource` — Resource pointing to the produced image.

---

### `IEnumerable<ILocalResource> ExtractImagesFromPdf(string fileName, string password = null, ImageExtension imageExtension = ImageExtension.PNG, string outputFolderName = null)`
### `IEnumerable<ILocalResource> ExtractImagesFromPdf(IResource file, string password = null, ImageExtension imageExtension = ImageExtension.PNG, string outputFolderName = null)`

Extracts every image embedded in the PDF and writes each to its own file in the chosen format. Note: this extracts **embedded images** (logos, photos, diagrams as raster objects) — it does **not** rasterize the full page. Use `ExportPdfPageAsImage` for that.

**Parameters:**
- `fileName` / `file` — Source PDF.
- `password` (`string`) — PDF password if protected (default: `null`).
- `imageExtension` (`ImageExtension`) — Output format (default: `ImageExtension.PNG`).
- `outputFolderName` (`string`) — Destination folder. If `null` or empty, files are written to `Environment.CurrentDirectory` (default: `null`).

**Returns:** `IEnumerable<ILocalResource>` — One resource per extracted image. Empty enumerable if the PDF has no embedded images.

---

### `IEnumerable<ILocalResource> ExtractAttachmentsFromPdf(string fileName, string password = null, string[] filter = null, string outputFolderName = null)`
### `IEnumerable<ILocalResource> ExtractAttachmentsFromPdf(IResource file, string password = null, string[] filter = null, string outputFolderName = null)`

Extracts every file attachment embedded in the PDF (e.g. supplementary `.docx`, `.xlsx`, `.pdf` files attached to the document) and writes each to disk. Attachments are distinct from embedded *images*; use `ExtractImagesFromPdf` for raster objects in the page content.

**Parameters:**
- `fileName` / `file` — Source PDF.
- `password` (`string`) — PDF password if protected (default: `null`).
- `filter` (`string[]`) — File extensions to include, e.g. `new[] { ".docx", ".xlsx" }`. If `null`, every attachment is extracted (default: `null`).
- `outputFolderName` (`string`) — Destination folder. If `null` or empty, files are written to `Environment.CurrentDirectory` (default: `null`).

**Returns:** `IEnumerable<ILocalResource>` — One resource per extracted attachment. Empty enumerable if the PDF has no attachments matching the filter.

---

## Combine & Modify

### `ILocalResource JoinPdf(string[] fileList, string outputFileName = null)`
### `ILocalResource JoinPdf(IResource[] fileList, string outputFileName = null)`

Concatenates multiple PDFs into a single PDF. Order matches the array order.

**Parameters:**
- `fileList` (`string[]` or `IResource[]`) — Source files in concatenation order. All must be valid, readable PDFs.
- `outputFileName` (`string`) — Output path. If `null`, an auto-generated filename is used (default: `null`).

**Returns:** `ILocalResource` — Resource pointing to the merged PDF.

---

### `ILocalResource ManagePdfPassword(string fileName, string oldUserPassword, string newUserPassword, string oldOwnerPassword, string newOwnerPassword, string outputFileName = null)`
### `ILocalResource ManagePdfPassword(IResource file, string oldUserPassword, string newUserPassword, string oldOwnerPassword, string newOwnerPassword, string outputFileName = null)`

Sets, changes, or removes the user and/or owner passwords on a PDF.

**Parameters:**
- `fileName` / `file` — Source PDF.
- `oldUserPassword` (`string`) — Current user password. Pass `null` or empty if the PDF has none.
- `newUserPassword` (`string`) — New user password. Pass `null` or empty to **remove** the user password.
- `oldOwnerPassword` (`string`) — Current owner password.
- `newOwnerPassword` (`string`) — New owner password. Pass `null` or empty to **remove** it.
- `outputFileName` (`string`) — Output path. If `null`, an auto-generated filename is used (default: `null`).

**Returns:** `ILocalResource` — Resource pointing to the password-updated PDF.

> **User vs. owner password:** the *user* password is required to open the file. The *owner* password unlocks permissions (printing, copying, modifying). A PDF can have either, both, or neither.

---

## Conversion to PDF

### `ILocalResource ConvertHtmlToPdf(string html, string outputFileName = null, bool keepBackground = true, ConvertToPdfOptions options = null)`

Renders an HTML string into a PDF.

**Parameters:**
- `html` (`string`) — The HTML source to render. Can include inline CSS; external references must use absolute URLs.
- `outputFileName` (`string`) — Output path. If `null`, auto-generated (default: `null`).
- `keepBackground` (`bool`) — Preserve background colors and images in the rendered PDF (default: `true`).
- `options` (`ConvertToPdfOptions`) — Layout/content options. If `null`, defaults apply (A4, no header/footer, margin 0, scale 1.0).

**Returns:** `ILocalResource` — Resource pointing to the produced PDF.

---

### `ILocalResource ConvertEmailToPdf(MailMessage email, string outputFileName = null, bool keepBackground = true, ConvertToPdfOptions options = null)`

Renders a `System.Net.Mail.MailMessage` (headers, body, inline content) into a PDF.

**Parameters:**
- `email` (`MailMessage`) — The email message to convert. The body is rendered honoring `IsBodyHtml`.
- `outputFileName` (`string`) — Output path or `null` for auto-generated.
- `keepBackground` (`bool`) — Preserve background styling (default: `true`).
- `options` (`ConvertToPdfOptions`) — Layout options or `null` for defaults.

**Returns:** `ILocalResource` — Resource pointing to the produced PDF.

---

### `ILocalResource ConvertTextToPdf(string text, string outputFileName = null, int fontSize = 12, TextAlignment textAlignment = TextAlignment.Justify, ConvertToPdfOptions options = null)`

Wraps plain text in a simple PDF document with configurable font size and alignment.

**Parameters:**
- `text` (`string`) — The text content to write.
- `outputFileName` (`string`) — Output path or `null` for auto-generated.
- `fontSize` (`int`) — Font size in points (default: `12`).
- `textAlignment` (`TextAlignment`) — Paragraph alignment (default: `TextAlignment.Justify`).
- `options` (`ConvertToPdfOptions`) — Layout options or `null` for defaults.

**Returns:** `ILocalResource` — Resource pointing to the produced PDF.

---

## Options & Configuration Classes

### `ConvertToPdfOptions` (namespace `UiPath.PDF.Activities.Api`)

Common layout and content options shared across `ConvertHtmlToPdf`, `ConvertEmailToPdf`, and `ConvertTextToPdf`. All properties are optional.

| Property | Type | Default | Description |
|---|---|---|---|
| `HeaderHtml` | `string` | `null` | HTML snippet rendered as the page header on every page. `null` = no header. |
| `FooterHtml` | `string` | `null` | HTML snippet rendered as the page footer on every page. `null` = no footer. |
| `PaperSize` | `PaperSize` | `A4` | Page paper size. See enum reference below. |
| `Margin` | `int` | `0` | Page margin in points (1 point = 1/72 in). |
| `Scale` | `double` | `1.0` | Rendering scale. **Must be between `0.1` and `2.0` inclusive** — values outside this range throw on use. |

Construct it with object-initializer syntax:

```csharp
var options = new ConvertToPdfOptions
{
    PaperSize = PaperSize.Letter,
    Margin = 36,                 // half-inch margin
    HeaderHtml = "<h4>Invoice #2026-05</h4>",
    FooterHtml = "<small>Page {pageNumber}</small>"
};
```

---

## Enum Reference

**`ImageDpi`** (namespace `UiPath.PDF`) — pixel density for raster operations:
- `Low` = 96 DPI
- `Medium` = 150 DPI *(default for `ReadPdfWithOcr` and `ExportPdfPageAsImage`)*
- `High` = 270 DPI

**`ImageExtension`** (namespace `UiPath.PDF`) — output image format for `ExtractImagesFromPdf`:
- `PNG` *(default)*, `JPEG`, `TIFF`, `BMP`
- `GIF` — **deprecated**; the value exists for XAML back-compat but workflows that select it fail design-time validation. Do not use.

**`TextAlignment`** (namespace `UiPath.PDF.Activities.PDF.Enums`) — paragraph alignment for `ConvertTextToPdf`:
- `Justify` *(default)*, `Left`, `Right`, `Center`

**`PaperSize`** (namespace `UiPath.PDF.Activities.Api`) — paper size for `ConvertToPdfOptions.PaperSize`:
- `A4` *(default)*, `A3`, `A5`, `A2`, `A6`
- `Letter`, `Legal`, `Tabloid`, `Ledger`, `Executive`, `Statement`
- `B4`, `B5`
- `Number10Envelope`, `DLEnvelope`, `C5Envelope`, `C4Envelope`

---

## Common Patterns

### 1. Read text from a single PDF

```csharp
public class ReadInvoice : CodedWorkflow
{
    [Workflow]
    public void Execute()
    {
        string text = pdf.ReadPdfText("C:\\invoices\\march.pdf");
        Log(text);
    }
}
```

### 2. Read a scanned PDF via OCR (cross-package, requires `UiPath.OCR.Activities`)

Add `"UiPath.OCR.Activities": "*"` to `project.json` and use both services together. The `ocr` accessor comes from the OCR package's registry.

```csharp
public class OcrInvoice : CodedWorkflow
{
    [Workflow]
    public void Execute()
    {
        // Resolve an OCR engine from the OCR service
        var ocrEngine = ocr.GetUiPathDocumentOcr(
            apiKey: Config["DocumentOcrApiKey"].ToString());

        // Hand the engine to the PDF service
        string text = pdf.ReadPdfWithOcr(
            fileName: "C:\\scans\\contract.pdf",
            ocrEngine: ocrEngine,
            range: "All",
            degreeOfParallelism: 2,
            imageDpi: ImageDpi.High);

        Log($"Extracted {text.Length} characters");
    }
}
```

### 3. Merge a folder of PDFs and emit one combined file

```csharp
public class MonthlyRollup : CodedWorkflow
{
    [Workflow]
    public void Execute()
    {
        string[] inputs = Directory.GetFiles(
            @"C:\monthly", "*.pdf", SearchOption.TopDirectoryOnly);

        ILocalResource merged = pdf.JoinPdf(
            fileList: inputs,
            outputFileName: @"C:\monthly\rollup.pdf");

        Log($"Merged {inputs.Length} files → {merged.LocalPath}");
    }
}
```

### 4. Iterate every page, exporting each as a PNG

`GetPdfPageCount` plus `ExportPdfPageAsImage` is the standard pattern for per-page processing.

```csharp
public class PdfToImages : CodedWorkflow
{
    [Workflow]
    public void Execute()
    {
        const string source = @"C:\reports\quarterly.pdf";
        int pages = pdf.GetPdfPageCount(source);

        for (int p = 1; p <= pages; p++)
        {
            ILocalResource img = pdf.ExportPdfPageAsImage(
                fileName: source,
                pageNumber: p,
                outputFileName: $@"C:\reports\page-{p:D3}.png",
                imageDpi: ImageDpi.High);

            Log($"Page {p} → {img.LocalPath}");
        }
    }
}
```

### 5. Convert HTML to PDF with a custom header and Letter paper

```csharp
public class GenerateReport : CodedWorkflow
{
    [Workflow]
    public void Execute()
    {
        string html = "<h1>Q1 Results</h1><p>Revenue: $1.2M</p>";

        var options = new ConvertToPdfOptions
        {
            PaperSize = PaperSize.Letter,
            Margin = 36,
            HeaderHtml = "<div style='font-size:10px'>Confidential</div>",
            FooterHtml = "<div style='font-size:10px'>Page {pageNumber}</div>",
            Scale = 1.0
        };

        ILocalResource report = pdf.ConvertHtmlToPdf(
            html: html,
            outputFileName: @"C:\reports\q1.pdf",
            keepBackground: true,
            options: options);

        Log($"Report ready: {report.LocalPath}");
    }
}
```

---

## Notes for Coding Agents

- **No `using` blocks needed.** No method on the `pdf` service returns `IDisposable`. Just call and use.
- **Capture results before chaining.** When you need to feed an `ILocalResource` from one method into another that takes `IResource`, store it in a variable first — passing the method call inline works too, but a named local makes telemetry/logging easier.
- **Availability gates apply at compile time.** If your project targets cross-platform, the Windows-only methods listed in "Conditional availability" are not present on the `pdf` service — calls to them won't compile, not just fail.
- **Clean up files you create.** Most methods produce real files on disk — joined PDFs, extracted page ranges, extracted images, exported page images, password-modified PDFs, and converted PDFs all land on the file system. Be disciplined about this: if a file you produced is only an intermediate or scratch result that is no longer needed at the end of the workflow, delete it (`File.Delete(resource.LocalPath)` for a single file, `Directory.Delete(folder, recursive: true)` for an extracted-images folder). Leaving stale outputs around bloats robot disks, leaks information across workflow runs, and confuses subsequent executions that scan the same folder.
