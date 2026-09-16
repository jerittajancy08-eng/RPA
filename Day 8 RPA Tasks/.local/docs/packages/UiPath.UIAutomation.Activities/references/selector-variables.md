# Selector Variables

Inject variable/argument values into configured selector values.

| Location | Authoring form | Result |
|---|---|---|
| Hand-authored XAML `InArgument<string>` | Write `string.Format` | Stored/runtime form |
| Definition (`window.xaml`, `target-n.xaml`) | Pass `{{variableName}}` to CLI `update-definition`; never hand-edit | CLI writes equivalent `string.Format` |

`{{variableName}}` is CLI input only, never stored. Every referenced variable must be declared as an XAML argument or enclosing activity variable (for example, parent `Sequence`); unresolved runtime token becomes literal text. Definition files belong to [`uia-configure-target`](../skills/uia-configure-target/SKILL.md): mutate windows with `target-app update-definition`, elements with `target-anchorable update-definition`.

## When to parametrize: per-item actions in a loop

**Rule:** selector matching more than one runtime item must carry the current item's distinguishing value via the loop variable, else it resolves to an arbitrary or stale match.

Applies when an activity runs once per item in a loop (`ForEach` over rows/results/cards/entries) acting on the current item's element. Pin the varying value on the **item/row selector**, not only the leaf control.

Do NOT drop the per-item value because an upstream step (search, filter, prior navigation) narrowed the UI to one match. Fails at runtime:

- Panes/lists don't clear between iterations — non-pinned selector matches a **stale** prior-item node.
- One query returns several qualifying nodes (variants, duplicates) — non-pinned selector is ambiguous.
- Lists repopulate asynchronously — selector resolves before the current item is present.

A `selector-intelligence evaluate` distinctness warning ("not sufficiently distinct" or equivalent `ToolingFeedback`) is **blocking** for a looped target, not advisory — even when every candidate carries it and `IsValid` is `true`.

Parametrize only the per-item varying value; keep constant constraints (labels, roles, fixed qualifiers) literal. Mechanism: [§ Choosing how to parametrize](#choosing-how-to-parametrize).

### Worked example: per-item row in a results list

`ForEach` over `productName`; loop body clicks current result row's Add to Cart button. Several sellers list the same product; only the `Contoso` listing qualifies. `resolve-defaults` on first item returns:

```xml
<ctrl name='Results' role='table' /><ctrl name='View Wireless Mouse M185 by Contoso Wireless Mouse M185 Contoso Electronics *' role='row' rowName='Wireless Mouse M185  Contoso' /><ctrl name='Add to Cart' role='push button' />
```

Value substitution per attribute — tag list and attribute names unchanged ([§ Hard rule](#hard-rule-only-parametrize-a-configured-target--never-hand-add-an-attribute)):

```text
name:    'View Wireless Mouse M185 by Contoso Wireless Mouse M185 Contoso Electronics *' -> 'View {{productName}} by Contoso*'
rowName: 'Wireless Mouse M185  Contoso'                                                  -> '{{productName}}  Contoso'
```

- Parametrize only first product-name occurrence in `name`; trailing residue (repeated product name, seller, category) not derivable from loop variable -> `*`.
- `by Contoso` stays literal: discriminator excluding other sellers' rows.
- Keep `rowName` two-space gap verbatim; product occurrence varies, seller constant.

```bash
uip rpa uia target-anchorable update-definition \
  --definition-file-path "C:/path/to/target-3.xaml" \
  --full-selector "<ctrl name='Results' role='table' /><ctrl name='View {{productName}} by Contoso*' role='row' rowName='{{productName}}  Contoso' /><ctrl name='Add to Cart' role='push button' />"
```

Sequencing: confirm literal selector against live current item first (`target-anchorable validate --definition-file-path ...` prints highlighted screenshot); substitute only after every live check passes — stored `string.Format` selector no longer validates against the live app. Read `SearchSteps` to pick flag: strict `Selector` -> `--full-selector`; `FuzzySelector` -> `--fuzzy-selector`.

## Hard rule: only parametrize a configured target — never hand-add an attribute

Tags/attributes come only from `resolve-defaults` and `uia-improve-selector`. Parametrization only replaces an existing value; never compose selector text, change attribute set, add an attribute, or parametrize a hand-authored selector. Replacing a residue span of an already-present value with `*` is part of the same permitted edit — only for text not derivable from the loop variable, never over the discriminator.

### Choosing how to parametrize

Read configured selector; follow first match:

| Attribute to parametrize | Action |
|--------------------------|--------|
| **Already present** (strict or fuzzy) | Swap that literal value for `{{variable}}` token via `update-definition`, keeping exact attribute set. |
| **Missing from a strict selector** (`SearchSteps` contains `Selector`) | Run `uia-improve-selector` subagent; name attribute to add and parametrize in its `$ATTR_INFO`. |
| **Missing from a fuzzy selector** (`SearchSteps` is `FuzzySelector`) | Subagent can't act on fuzzy selector. Parametrize different already-present attribute expressing same variation, or **abort** — never hand-add attribute. |

For subagent, provide target context as `$NODE_INFO` and requested attribute/parameter as `$ATTR_INFO`. See [`uia-configure-target`](../skills/uia-configure-target/SKILL.md) TARGET-7, section 7.2.

## In XAML

Variable-capable selector `InArgument<string>` properties:

- `Selector` — selector on `TargetApp` (Use Application/Browser). Only `TargetApp` property supporting variables.
- `ScopeSelectorArgument` — window selector on `TargetAnchorable`.
- `FullSelectorArgument` — strict element selector on `TargetAnchorable`.
- `FuzzySelectorArgument` — fuzzy element selector on `TargetAnchorable`.

Rules:

- Positional placeholders: `{0}`, `{1}`, …
- Each placeholder binds, in order, to variables listed after format string.
- Escape literal `{` and `}` as `{{` and `}}`.

VB expression:

```text
String.Format("<webctrl id='{0}' tag='{1}' />", elementId, tagName)
```

C# expression:

```text
string.Format("<webctrl id='{0}' tag='{1}' />", elementId, tagName)
```

Entire selector may be bare variable (no `string.Format`):

```text
mySelector
```

### XAML example

`TargetAnchorable` example:

```xml
<uix:TargetAnchorable Version="V6">
  <uix:TargetAnchorable.ScopeSelectorArgument>
    <InArgument x:TypeArguments="x:String">[string.Format("&lt;wnd app='chrome.exe' title='{0} - Google Chrome' /&gt;", pageTitle)]</InArgument>
  </uix:TargetAnchorable.ScopeSelectorArgument>
  <uix:TargetAnchorable.FullSelectorArgument>
    <InArgument x:TypeArguments="x:String">[string.Format("&lt;webctrl id='{0}' tag='BUTTON' /&gt;", elementId)]</InArgument>
  </uix:TargetAnchorable.FullSelectorArgument>
  <uix:TargetAnchorable.FuzzySelectorArgument>
    <InArgument x:TypeArguments="x:String">[string.Format("&lt;webctrl id='{0}' /&gt;", elementId)]</InArgument>
  </uix:TargetAnchorable.FuzzySelectorArgument>
</uix:TargetAnchorable>
```

`TargetApp.Selector` example:

```xml
<uix:TargetApp Version="V3">
  <uix:TargetApp.Selector>
    <InArgument x:TypeArguments="x:String">[string.Format("&lt;wnd app='chrome.exe' title='{0} - Google Chrome' /&gt;", pageTitle)]</InArgument>
  </uix:TargetApp.Selector>
</uix:TargetApp>
```

## In definition files

Definition is serialized `TargetApp`/`TargetAnchorable` plus sibling `*.xaml.metadata`. CLI atomically rewrites both. Pass `{{variableName}}` to `update-definition`; never hand-edit or pass handwritten `string.Format`.

`{{...}}` input syntax:

- Token must be valid identifier; otherwise literal.
- Repeated variable uses same placeholder (`{0}`) and one trailing argument per distinct variable.
- Bare entire-selector variable (for example, `mySelector`) stores direct variable expression.

### Element selector (`target-anchorable`)

`--full-selector` (strict), `--fuzzy-selector`, `--scope-selector` (window), and `--semantic-selector` accept `{{variable}}`; `--full-selector` and `--fuzzy-selector` are mutually exclusive.

```bash
uip rpa uia target-anchorable update-definition \
  --definition-file-path "C:/path/to/target-1.xaml" \
  --full-selector "<webctrl data-test='{{testName}}' data-field='result' tag='INPUT' />"
```

CLI stores:

```xml
<uix:TargetAnchorable.FullSelectorArgument>
  <InArgument x:TypeArguments="x:String">[string.Format("&lt;webctrl data-test='{0}' data-field='result' tag='INPUT' /&gt;", testName)]</InArgument>
</uix:TargetAnchorable.FullSelectorArgument>
```

### Window selector (`target-app`)

`--selector` is only variable-capable `TargetApp` property.

```bash
uip rpa uia target-app update-definition \
  --definition-file-path "C:/path/to/window.xaml" \
  --selector "<wnd app='chrome.exe' title='{{pageTitle}} - Google Chrome' />"
```

CLI stores:

```xml
<uix:TargetApp.Selector>
  <InArgument x:TypeArguments="x:String">[string.Format("&lt;wnd app='chrome.exe' title='{0} - Google Chrome' /&gt;", pageTitle)]</InArgument>
</uix:TargetApp.Selector>
```

## Conversion between the two forms

| Form | Where it appears | Example |
|------|------------------|---------|
| `{{variable}}` | input to `update-definition` (CLI/UI "string view") | `<webctrl id='{{x}}' />` |
| `string.Format` | `InArgument` stored in XAML / definition file | `string.Format("<webctrl id='{0}' />", x)` |
