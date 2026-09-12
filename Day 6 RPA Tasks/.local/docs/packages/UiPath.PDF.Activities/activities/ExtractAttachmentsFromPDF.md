# Extract Attachments from PDF

`UiPath.PDF.Activities.PDF.ExtractAttachmentsFromPDF`

Extract files embedded in a PDF document and save them to a local folder. The activity returns the list of saved files as `ILocalResource` references.

**Package:** `UiPath.PDF.Activities`
**Category:** App Integration > PDF

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `FileName` | File (local path) | `InArgument` | `string` | Yes* | | | The local file path of the PDF to extract attachments from. Mutually exclusive with `ResourceFile`. |
| `ResourceFile` | File | `InArgument` | `IResource` | Yes* | | | A file resource representing the PDF to extract attachments from. Mutually exclusive with `FileName`. |
| `Password` | Password | `InArgument` | `string` | | `null` | | The password used to open a password-protected PDF. Leave empty if the file is not encrypted. |
| `OutputFolderName` | Output folder | `InArgument` | `string` | | `null` | | The folder path where the extracted files are saved. If omitted, files are saved to the current working directory. |
| `FileExtensionFilter` | File extension filter | `InArgument` | `string[]` | | `null` | | Filter attachments by extension (e.g. `.xml` or `xml`). Leave empty to extract all attachments. |

> *\*Exactly one of `FileName` or `ResourceFile` is required (`[OverloadGroup]`).*

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `ExtractedFiles` | Extracted files | `IEnumerable<ILocalResource>` | A collection of local resource references pointing to each extracted file. |

## Valid Configurations

This activity supports two mutually exclusive file inputs:

**FileName overload** — supply `FileName` as a local file path string. Do not set `ResourceFile`.  
**ResourceFile overload** — supply `ResourceFile` as an `IResource` reference. Do not set `FileName`.

Exactly one must be set. Providing both or neither causes a validation error at runtime.

## XAML Example

**Using a local file path (`FileName` overload):**

```xml
<!-- xmlns:upap="clr-namespace:UiPath.PDF.Activities.PDF;assembly=UiPath.PDF.Activities" -->

<upap:ExtractAttachmentsFromPDF
    DisplayName="Extract Attachments from PDF"
    FileName="[pdfFilePath]"
    FileExtensionFilter="[extensionFilter]"
    OutputFolderName="[outputFolder]"
    ExtractedFiles="[extractedFiles]" />
```

**Using a file resource (`ResourceFile` overload):**

```xml
<!-- xmlns:upap="clr-namespace:UiPath.PDF.Activities.PDF;assembly=UiPath.PDF.Activities" -->
<!-- xmlns:upr="clr-namespace:UiPath.Platform.ResourceHandling;assembly=UiPath.Platform" -->

<upap:ExtractAttachmentsFromPDF
    DisplayName="Extract Attachments from PDF"
    ExtractedFiles="[extractedFiles]">
  <upap:ExtractAttachmentsFromPDF.Item>
    <upap:ItemArgument x:TypeArguments="upr:IResource"
        ResourceFile="[resourceFile]" />
  </upap:ExtractAttachmentsFromPDF.Item>
</upap:ExtractAttachmentsFromPDF>
```

## Notes

- `FileName` and `ResourceFile` are mutually exclusive (`[OverloadGroup]`). Supply exactly one. In the designer, a context menu lets you switch between **Use File Path** and **Use a File Resource**. In XAML the `ResourceFile` overload requires a nested `<upap:ExtractAttachmentsFromPDF.Item>` child element containing an `<upap:ItemArgument x:TypeArguments="upr:IResource" ResourceFile="[...]" />` element (see the `ResourceFile` XAML example above). `IResource` is from `clr-namespace:UiPath.Platform.ResourceHandling;assembly=UiPath.Platform`.
- `FileExtensionFilter` normalizes extensions: a leading dot is added if missing (e.g. `xml` becomes `.xml`), and matching is case-insensitive. Leave the argument empty or unset to extract all attachments regardless of type.
- The `OutputFolderName` path must already exist; the activity does not create directories.
- If `OutputFolderName` is omitted, files are saved to the current working directory.
- Attachment filenames are sanitized before writing. A filename that is empty or contains invalid path characters causes an `ArgumentException`.
