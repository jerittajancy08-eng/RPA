# SelectItem Usage — Deciding and Driving Option-List Controls

Determine whether dropdown/list/combo supports one select action (`SelectItem` or `uip rpa uia interact select`), read options, confirm result.

Technology, tag, and role do **not** determine selectability: native `<select>`, WinForms/WPF combo, Java/SAP list, div/ARIA combobox all qualify. Query control; never infer from selector or snapshot node type.

## 1. Detect — read the `items` attribute

```bash
uip rpa uia interact get <e-ref> items
```

- **`items` lists options** → select-drivable. Pass listed value verbatim to `select` or `SelectItem` activity's `Item`. No list opening or option capture: `items` returns full closed-control list.
- **`items` empty/absent** → not option-list control. Use click-to-open + click option, or type into type-ahead/filter combo.

`uip rpa uia interact get-all <e-ref>` returns `items`, `selecteditem`, `selecteditems`, and all attributes; use when other clues matter.

`[items deferred]` snapshot marker means options omitted there but usually available through `get`/`get-all`. If empty, expand control and retry or re-snapshot window.

## 2. Select

```bash
uip rpa uia interact select <e-ref> "<value>"
```

`<value>` must exactly match `items` text and casing. `Cannot select item. It was not found among existing items` means no matching option, not unsupported control: re-read `items`; pass one verbatim.

## 3. Confirm

```bash
uip rpa uia interact get <e-ref> selecteditem    # single-select
uip rpa uia interact get <e-ref> selecteditems   # multi-select
```

## Full parameter reference

`cli-reference.md` § Interact › Select, Get, Get All in this `references/` folder.
