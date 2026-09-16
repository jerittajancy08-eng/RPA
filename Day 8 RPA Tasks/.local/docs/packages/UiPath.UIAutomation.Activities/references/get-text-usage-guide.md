# GetText Usage — Extracting Information from an Application

Decide whether to read a value with `Get Text` (`NGetText`) or `Get Attribute`, and confirm the extracted value before wiring it into the workflow.

## 1. Prefer `Get Text` on a visible element

If the information is visible on the screen, `Get Text` on that element is the default choice — try it first. Reach for `Get Attribute` only when `Get Text` doesn't return a clean, complete, or structured enough value (see § 4).

## 2. Mandatory: preview the extraction before authoring

Preview is a **precondition for authoring**, not a reaction to a bad result. `target-anchorable validate` and a highlight screenshot establish *identity* — that the selector finds the intended element — and say nothing about what it would *extract*. Confirm the value itself:

```bash
uip rpa uia target-anchorable preview-text --definition-file-path "path/to/target.xaml"
```

This runs the same scraping logic `Get Text` uses (the "Preview Extraction" option in Studio) against the live application and prints exactly what the activity would return — works on any captured target-anchorable definition, before it's wired into a workflow. If the default scraping method returns nothing useful, retry with an explicit `--scraping-method` (`TextAttribute`, `Native`, `Fulltext`, `OCR` — see [`NScrapingMethod`](../activities/common/NScrapingMethod.md)):

```bash
uip rpa uia target-anchorable preview-text --definition-file-path "path/to/target.xaml" --scraping-method "Fulltext"
```

An element holding no text extracts an empty string, which is a result and not a failure: the command succeeds and prints `The target was found and extracted an empty string. If text was expected, try a different scraping method.` A nonzero exit means the target was not found or the scrape itself failed.

**`Get Text` does not simply "return whatever is currently rendered."** The scraping method decides how much text is captured, and they don't all read the same thing: `TextAttribute` reads a single control property, `Fulltext` walks the element **and its children** (so it can surface text no single element visibly displays on its own), `Native` draws/reads via Windows-specific APIs, and `OCR` reads pixels. An empty or partial result on a visible element can mean the wrong scraping method is selected, not a broken selector — confirm with `preview-text` and try another `--scraping-method` before concluding the target itself is wrong.

## 3. Preview an empty instance whenever control flow branches on emptiness

`Get Text` may fall back to the element's **accessible name**, which is identity, not content — so it can return a plausible non-empty value for an element with no content. For grid and cell controls the accessible name is frequently the cell address: an empty Excel `A10` yields `A10`, not `""`, while `interact get-all` shows an empty `text` attribute.

Any control flow that stops at the first blank row, cell, or result depends entirely on the empty case, so that is the case to preview — a populated instance proves nothing about it. Previewed text equal to the node's accessible name or `automationid` means you are reading identity: read the content attribute (`text` for a grid cell) with `Get Attribute` instead (§ 4), or terminate on a row count or explicit sentinel.

## 4. When the value you want isn't (cleanly) in the text — use `Get Attribute`

A control's attributes frequently hold a cleaner, more structured form of the same data (canonical value, `aria-*`, tooltip, formatted number) than the on-screen text. Dump the element's attributes to see what's available:

```bash
uip rpa uia interact get-all <e-ref>          # all attributes
uip rpa uia interact get <e-ref> <attribute>  # one attribute
```

Full attribute list: [`GetAttribute.md`](../activities/GetAttribute.md). When a better attribute exists, read it with `Get Attribute` instead of `Get Text`.

## Full parameter reference

`cli-reference.md` § Target Anchorable › Preview Text, Interact › Get, Get All (sibling file in this `references/` folder).
