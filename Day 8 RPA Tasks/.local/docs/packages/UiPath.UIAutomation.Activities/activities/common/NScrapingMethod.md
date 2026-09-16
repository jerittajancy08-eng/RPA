# NScrapingMethod

`UiPath.UIAutomationNext.Enums.NScrapingMethod`

The text scraping method used to extract text from UI elements (used by Get Text and similar activities).

## Values

| Value | Description |
|-------|-------------|
| `NScrapingMethod.Default` | Goes through possible scraping methods (`TextAttribute`, `Fulltext`, in that order) and stops at the first method that returns data. |
| `NScrapingMethod.TextAttribute` | Extracts the text attribute value for the specified UI element. |
| `NScrapingMethod.Native` | Uses Windows specific APIs to draw and get the text of the element. Works mostly on legacy applications. |
| `NScrapingMethod.Fulltext` | Extracts the full text of the element including its child elements. Useful for container elements. |
| `NScrapingMethod.OCR` | Extracts the text using the OCR engine. |

## Usage

Reference values as `NScrapingMethod.<Value>`, e.g. `NScrapingMethod.Default`.
