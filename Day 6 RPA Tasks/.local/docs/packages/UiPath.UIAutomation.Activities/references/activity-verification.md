# Activity Execution Verification

Post-action verification confirms appearance, disappearance, text/image change, or Type Into result. Configure `VerifyOptions` only on four supported activities. Failed check with `Retry=True` reruns activity until activity `Timeout`; each check waits verify `Timeout`. Final failure errors; activity `ContinueOnError=True` swallows it.

## Which activities support verification

| Activity | XAML class | `VerifyOptions` type |
|----------|------------|----------------------|
| Click | `NClick` | `VerifyExecutionOptions` |
| Hover | `NHover` | `VerifyExecutionOptions` |
| Keyboard Shortcuts | `NKeyboardShortcuts` | `VerifyExecutionOptions` |
| **Type Into** | `NTypeInto` | **`VerifyExecutionTypeIntoOptions`** — see [Type Into (special)](#type-into-special-cases) |

## Settable members

Set inside `VerifyOptions`. Full type: [`VerifyExecutionOptions.md`](../activities/common/VerifyExecutionOptions.md).

| Member | Type | Notes |
|--------|------|-------|
| `Mode` | [`NVerifyMode`](../activities/common/NVerifyMode.md) | Check to perform (target modes only — see below). |
| `Target` | `TargetAnchorable` | Element *observed* to confirm outcome. Required for all `NVerifyMode` checks (omit only for Type Into Auto / Expected text). |
| `Retry` | `InArgument<bool>` | Re-run activity until check passes or activity `Timeout` elapses. **Default `True`** when unset. |
| `Timeout` | `InArgument<double>` | Seconds to wait for expected outcome during verification. **Default `10`** when unset. |
| `ExpectedText` | `InArgument<string>` | **Type Into only** — expected typed result; see [below](#type-into-special-cases). |

## Verify modes (`NVerifyMode`)

Every mode requires verify `Target`.

| `Mode` | Use when expected outcome is… |
|--------|-----------------------------------|
| `Appears` | New element shows up (e.g. success banner after clicking Submit). |
| `Disappears` | Element goes away (e.g. row vanishes after clicking Delete, spinner clears). |
| `TextChanges` | Target's text changes from its pre-action value. |
| `AspectChanges` | Target's image/appearance changes. **Not available on portable / Studio Web projects.** |

## Choosing the verify target: reuse or create

Verify target is observed, never acted on.

| Situation | Action |
|---|---|
| Target already in OR | Reuse its `referenceId` per [`uia-target-attachment-guide.md`](uia-target-attachment-guide.md); repeat ID when needed. |
| Target absent from OR | Create with [`uia-configure-target`](../skills/uia-configure-target/SKILL.md). Describe observed state (for example, "confirmation banner that appears"), not action, so inferred activity type defaults to neutral `None`; no `--activity-type` flag. See [`selection-activity-types.md`](selection-activity-types.md). |

## Adding verification (target modes)

For Click, Hover, Keyboard Shortcuts, or Type Into target mode:

1. Pick `Mode`.
2. Reuse/create verify target.
3. Add `VerifyOptions` with `Mode`/`Retry`/`Timeout`.
4. Attach target at `VerifyOptions.Target` with nested `targetProperty` per [`uia-target-attachment-guide.md`](uia-target-attachment-guide.md):

   ```bash
   uip rpa uia object-repository link-elements \
     --elements '[{"workflowFilePath":"Workflows/Main.xaml","activityId":"NClick_1","referenceId":"<VERIFY_ELEMENT_REF>","targetProperty":"VerifyOptions.Target"}]' \
     --project-dir "<PROJECT_DIR>"
   ```

`<VerifyExecutionOptions>` must exist before linking; linker fills `.Target`, not options object. On link error, get XAML via `get-elements-xaml` and embed `<uix:TargetAnchorable>` inside `VerifyExecutionOptions.Target` per attachment guide.

### Example — Click, `Appears` with a verify target

```xml
<uix:NClick DisplayName="Click Submit" sap2010:WorkflowViewState.IdRef="NClick_1">
  <uix:NClick.Target>
    <uix:TargetAnchorable Version="V6" />
  </uix:NClick.Target>
  <uix:NClick.VerifyOptions>
    <uix:VerifyExecutionOptions Mode="Appears" Retry="True" Timeout="10">
      <uix:VerifyExecutionOptions.Target>
        <uix:TargetAnchorable Version="V6" />
      </uix:VerifyExecutionOptions.Target>
    </uix:VerifyExecutionOptions>
  </uix:NClick.VerifyOptions>
</uix:NClick>
```

`Mode`, literal `Retry`, and `Timeout` are attributes. `link-elements` fills target placeholders; fallback embeds resolved selector.

## Type Into (special cases)

Type Into also supports two target-less modes; neither uses configure-target/link steps. `Mode` is irrelevant and may be omitted. See [`TypeInto.md`](../activities/TypeInto.md).

| Mode | Configuration |
|---|---|
| Auto | Add `VerifyExecutionTypeIntoOptions` without `Target` or `ExpectedText`; verifies typed text landed. |
| Expected text | Set `ExpectedText` to compare actual typed result; wildcards supported (for example, `*@example.com`); omit `Target`. |
| None | Omit `VerifyOptions`. Presence of target-less `VerifyExecutionTypeIntoOptions` enables Auto. |

### Example — Type Into, Auto (verifies the typed text landed)

```xml
<uix:NTypeInto DisplayName="Type email" sap2010:WorkflowViewState.IdRef="NTypeInto_1">
  <uix:NTypeInto.Target>
    <uix:TargetAnchorable Version="V6" />
  </uix:NTypeInto.Target>
  <uix:NTypeInto.VerifyOptions>
    <uix:VerifyExecutionTypeIntoOptions Retry="True" Timeout="10" />
  </uix:NTypeInto.VerifyOptions>
</uix:NTypeInto>
```

### Example — Type Into, Expected text

```xml
<uix:NTypeInto.VerifyOptions>
  <uix:VerifyExecutionTypeIntoOptions ExpectedText="john.doe@example.com" Retry="True" Timeout="10" />
</uix:NTypeInto.VerifyOptions>
```

## Notes

Configure new verify targets as `None`, not `CheckElement`; `CheckElement` tuning belongs to Check Element / Check App State activities.
