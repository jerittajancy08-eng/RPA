# OR Target Attachment

Attach Object Repository screens/elements to XAML activities.

> **Inline JSON/Windows paths:** `--elements`/`--targets` take JSON arrays. Use forward slashes (`C:/Users/...`); backslashes become JSON escapes and cause `Bad JSON escape`.

## IdRef Contract

Linker addresses `sap2010:WorkflowViewState.IdRef`. Every linked activity MUST have file-unique `<ClassName>_<N>` IdRef, e.g. `NApplicationCard_1`, `NClick_1`, `NClick_2`, `NTypeInto_1`. Number per class; continue highest existing `N` at `N+1`.

This matches Studio naming and preserves clean reopen.

## Fast Path: Linking OR Entries to Activities

Write activities with unique IdRefs and no `.Target` child (nor nested target such as `.SearchedElement.Target`); then attach via `link-screen` and `link-elements`.

> **Never parallelize `link-screen` and `link-elements`:** both mutate same `.xaml` and corrupt concurrent writes. Link screen, then all elements in one `link-elements` call.

### 1. Link a screen to an ApplicationCard

```bash
uip rpa uia object-repository link-screen \
  --workflow-file-path "<RELATIVE_XAML_PATH>" \
  --activity-id "<ACTIVITY_REF_ID>" \
  --reference-id "<SCREEN_REFERENCE_ID>" \
  --project-dir "<PROJECT_DIR>"
```

| Flag | Required | Description |
|------|----------|-------------|
| `--workflow-file-path` | Yes | Path to `.xaml` file, relative to project directory (e.g., `Workflows/Main.xaml`). |
| `--activity-id` | Yes | `sap2010:WorkflowViewState.IdRef` on target activity — typically `NApplicationCard_1`. |
| `--reference-id` | Yes | OR screen reference from `uia-configure-target` or `indicate-application`. |

### 2. Link elements to UI activities

Batch all workflow `(activity, element)` pairs. `--elements` JSON contains one object per pair, each with `workflowFilePath` and optional `targetProperty`.

```bash
uip rpa uia object-repository link-elements \
  --elements '[{"workflowFilePath":"<RELATIVE_XAML_PATH>","activityId":"<ACTIVITY_REF_ID_1>","referenceId":"<ELEMENT_REFERENCE_ID_1>"},{"workflowFilePath":"<RELATIVE_XAML_PATH>","activityId":"<ACTIVITY_REF_ID_2>","referenceId":"<ELEMENT_REFERENCE_ID_2>","targetProperty":"SearchedElement.Target"}]' \
  --project-dir "<PROJECT_DIR>"
```

| Flag | Required | Description |
|------|----------|-------------|
| `--elements` | Yes | Each entry: `workflowFilePath` — `.xaml` path (relative to project directory or absolute inside it); `activityId` — `sap2010:WorkflowViewState.IdRef` on target activity (e.g., `NClick_3`); `referenceId` — OR element reference from `uia-configure-target` or `indicate-element`; `targetProperty` — optional, defaults to `"Target"` (dotted paths for nested properties, e.g., `"SearchedElement.Target"`). |

**Set `targetProperty`** only when target is not default `.Target`. `NClick`, `NTypeInto`, `NGetText` use default; `NMouseScroll` uses `SearchedElement.Target`.

**Element reuse:** when same element referenced by multiple activities (e.g., same field clicked then typed into), include one JSON entry per activity, repeating `referenceId` in each.

## Linking Failed: Check the File Deserializes

Both commands load the `.xaml` through the workflow designer, so a file that cannot be deserialized blocks linking. You have this case when the command prints `The property '<Name>' was not found on the activity.` followed by `This can happen when the workflow contains validation errors that prevent its activities from loading correctly.` A load-blocking error anywhere in the file aborts deserialization of the whole activity tree, so the requested activity resolves without its real properties — the offending XAML is often in an unrelated activity. Causes: malformed XAML, an unescaped `{` starting an attribute value, a missing `x:TypeArguments`, an unknown type or member (including one whose package is missing or version-mismatched), or a property setter that throws. Fix the XAML, then retry.

Do not try to clear the ordinary validation errors first. `Target or Input UI Element must be set.` and `The Use Application/Browser activity is not yet configured.` are the expected pre-link state and disappear once you link. Linking also succeeds with unrelated semantic errors outstanding (missing required fields, scope violations, design-time-discovery errors), so there is no ordering deadlock.

> Both commands print their failures (`link-elements` per entry) but can still exit `0`, and the failure may be absent from `--output json`: read the printed message, not the exit code.

## Fallback: Embedding OR Entries When Linking Fails

Use only after ruling out the load failure above and `link-screen`/`link-elements` still error for specific reference ID. Embed OR XAML in matching activity for failed reference only, not whole screen.

### 1. Get the screen XAML for the ApplicationCard

```bash
uip rpa uia object-repository get-screen-xaml \
  --reference-id "<SCREEN_REFERENCE_ID>" \
  --project-dir "<PROJECT_DIR>"
```

Returns `<TargetApp>`; embed inside ApplicationCard:

```xml
<uix:NApplicationCard.TargetApp>
  <uix:TargetApp .../>
</uix:NApplicationCard.TargetApp>
```

### 2. Get element XAML for UI activities

```bash
uip rpa uia object-repository get-elements-xaml \
  --reference-ids "<REF_1>,<REF_2>,<REF_3>" \
  --project-dir "<PROJECT_DIR>"
```

Returns one `<TargetAnchorable>` per reference ID. Each entry prints a `ReferenceId: <id>` line, an `XAML:` line, then the `<TargetAnchorable>` element, blank-line-separated (a failed entry prints `ReferenceId: <id>` then `Error: <message>`). Embed each `<TargetAnchorable>` in activity `.Target` or named nested property such as `SearchedElement.Target`:

```xml
<uix:NClick ...>
  <uix:NClick.Target>
    <uix:TargetAnchorable .../>
  </uix:NClick.Target>
</uix:NClick>

<uix:NTypeInto ...>
  <uix:NTypeInto.Target>
    <uix:TargetAnchorable .../>
  </uix:NTypeInto.Target>
</uix:NTypeInto>

<uix:NGetText ...>
  <uix:NGetText.Target>
    <uix:TargetAnchorable .../>
  </uix:NGetText.Target>
</uix:NGetText>
```

| Parameter | Source |
|-----------|--------|
| `<SCREEN_REFERENCE_ID>` | OR screen reference from `uia-configure-target` or `indicate-application` |
| `<REF_1>,<REF_2>,...` | Comma-separated OR element references from `uia-configure-target` or `indicate-element` |

For reused element (e.g., field clicked then typed), place same `<TargetAnchorable>` in each activity `.Target`.
