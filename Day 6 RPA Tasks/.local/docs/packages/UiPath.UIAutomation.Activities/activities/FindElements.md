# Find Elements

Returns a collection of UI elements that match a fuzzy partial selector (`Filter`). The search can be scoped to a target, an input element, or an enclosing application/browser scope.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NFindElements`
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard` *(only for `FindChildren` / `FindDescendants` when neither `Target` nor `InUiElement` is set)*

Use **Find Elements** when a workflow needs to collect multiple matching UI elements, such as rows, buttons, links, inputs, windows, or browser tabs.

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Description |
|------|--------------|------|------|----------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  | The container under which children are searched. See [Search context](#search-context). |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  | The Input UI Element defines the screen element that the activity will be executed on. See [Search context](#search-context). |
| `FilterTarget.FuzzySelectorArgument` | Filter | InArgument | `string` | **Yes** | Fuzzy partial selector that describes the elements to return. See [Authoring XAML](#authoring-xaml). |

### Configuration

| Name | Display Name | Kind | Type | Description |
|------|--------------|------|------|-------------|
| `Mode` | Mode | InArgument | [`NFindMode`](#modes) | Enables you to set the find mode of the UI elements in the collection. The following options are available: elements, descendants, top level. See [Modes](#modes). |
| `Timeout` | Timeout (seconds) | InArgument | `double` | The amount of time to wait for the element to appear or disappear, before executing one of the two activity blocks. Default value: `5` seconds. |
| `HealingAgentBehavior` | Healing Agent mode | InArgument | [`NChildHealingAgentBehavior`](common/NChildHealingAgentBehavior.md) | Configures the Healing Agent actions if they are allowed by Governance or Orchestrator process/job/trigger level settings. |
| `ScopeIdentifier` | Scope identifier | Property | `string` | Attaches this activity to a specific Application Card. Use the Application Card's `ScopeGuid` property. |

### Output

| Name | Display Name | Type | Description |
|------|--------------|------|-------------|
| `Children` | Children | `IEnumerable<UiElement>` | All UI children found according to the filter and scope set. The field supports only `IEnumerable<UiElement>` variables. See [Using the output](#using-the-output). |
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. See [Using the output](#using-the-output). |

### Common

| Name | Display Name | Kind | Type | Description |
|------|--------------|------|------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` | Continue executing the activities in the automation if this activity fails. The default value is `False`. |
| `DelayBefore` | Delay before | InArgument | `double` | Delay (in seconds) to wait before executing this activity. The default amount of time is `0.2` seconds. |

## Search context

`NFindElements` searches under a container resolved from one of:

- `Target` — set this when **Find Elements** should resolve the container itself.
- `InUiElement` — set this when a previous activity already returned the container (typically wired from its `OutUiElement`).
- The enclosing `NApplicationCard` scope — only used for `FindChildren` / `FindDescendants` and only when neither `Target` nor `InUiElement` is set.

`Target` defines *where* to search; `FilterTarget` defines *what* to return.

`FindTopLevels` does not use a search context. Setting `InUiElement` in this mode produces a design-time warning and is ignored at runtime. See each mode under [Modes](#modes) for the rules that apply.

## Using the output

`Children` returns the matching elements. Bind it to an `IEnumerable<UiElement>` variable and iterate with **For Each UI Element** (preferred — `CurrentItem` is typed as `UiElement`) or `ForEach<UiElement>`. Each element can be passed to other UI Automation activities through their `InUiElement`.

`OutUiElement` returns the resolved search context (the element resolved from `Target` / `InUiElement` under which children were searched), **not** one of the collected child elements. Capture it into a `UiElement` variable and feed it as `InUiElement` of the next activity to chain from the same target/scope.

## Modes

`Mode` controls where the fuzzy selector is evaluated.

### `FindChildren`

Searches first-level elements directly inside the indicated `Target`, `InUiElement`, or enclosing `NApplicationCard` scope. This is also the runtime behavior when `Mode` is unset.

**Search context.** Requires at least one of `Target`, `InUiElement`, or an enclosing `NApplicationCard`. Prefer the narrowest available — being inside an `NApplicationCard` alone passes validation but can make the search run across the whole application/browser surface. For lists, tables, menus, forms, or repeated page regions, set `NFindElements.Target` to the container and let `FilterTarget` describe the repeated children. Rely on the `NApplicationCard` scope by itself only when the intended search area really is the whole app/browser surface.

**Filter.** Must describe a single nesting level inside the search context.

### `FindDescendants`

Searches matching elements at any depth inside the indicated `Target`, `InUiElement`, or enclosing scope. Use this only when wrappers or nesting depth can vary — otherwise `FindChildren` against a tighter container is more predictable.

**Search context.** Same rules as `FindChildren`.

**Filter.** Matched at any depth inside the search context.

### `FindTopLevels`

Searches top-level windows or browser surfaces across all open applications.

**Search context.** Does not use a target or input element. Can run without an `NApplicationCard`, `Target`, or `InUiElement`. Setting `InUiElement` produces a design-time warning and is ignored at runtime.

**Filter.** Must describe a top-level container (a `<wnd>` or `<html>`), not an inner element.

## Validation

| Error | Cause | Fix |
|-------|-------|-----|
| `"Filter" field is required.` | `FilterTarget` is missing, or `FilterTarget.FuzzySelectorArgument` resolves to an empty value. | Add a `FilterTarget` with `SearchSteps="FuzzySelector"` and a non-empty `FuzzySelectorArgument`. |
| `Target or Input UI Element must be set.` | `Mode` is `FindChildren`, `FindDescendants`, or null, but there is no `NApplicationCard`, `Target`, or `InUiElement`. | Add a target, input element, or application/browser scope; or use `FindTopLevels` if the intent is to search top-level windows. |
| `For 'Find top levels' mode, the Input element will be ignored` | `Mode="FindTopLevels"` has `InUiElement` set. | Remove `InUiElement`. |
| No elements are returned | The filter does not match the tree level searched by the mode, or the target/scope is too narrow. | Recheck `Mode`, target/scope, and selector scope. For top-level search, use a top-level `<wnd ... />` or `<html ... />` selector. |

## Authoring XAML

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NFindElements`.

The generated XAML must still be completed with a valid `FilterTarget`, and for `FindChildren` / `FindDescendants` it must have a target, input element, or application/browser scope.

### Binding the Filter

The `Filter` is stored in `FilterTarget.FuzzySelectorArgument`. The `TargetAnchorable` must use `SearchSteps="FuzzySelector"`.

| Filter value | C# project | VB project |
|--------------|------------|------------|
| Literal selector | Use the XML-escaped selector as the `FuzzySelectorArgument` attribute. | Same as C#. |
| Variable selector | Use a `CSharpValue` inside the `FuzzySelectorArgument` property element. | Use VB bracket syntax in the `FuzzySelectorArgument` attribute, for example `[filter]`. |

Do not use VB bracket syntax such as `[filter]` in a C# project; bind a variable filter through a `CSharpValue` element under `FuzzySelectorArgument` instead.

### FilterTarget examples

Literal selector, valid for both C# and VB projects:

```xml
<uix:TargetAnchorable FuzzySelectorArgument="&lt;wnd app='*' /&gt;"
                      SearchSteps="FuzzySelector" />
```

C# variable selector:

```xml
<uix:TargetAnchorable SearchSteps="FuzzySelector">
  <uix:TargetAnchorable.FuzzySelectorArgument>
    <InArgument x:TypeArguments="x:String">
      <CSharpValue x:TypeArguments="x:String">filter</CSharpValue>
    </InArgument>
  </uix:TargetAnchorable.FuzzySelectorArgument>
</uix:TargetAnchorable>
```

VB variable selector:

```xml
<uix:TargetAnchorable FuzzySelectorArgument="[filter]"
                      SearchSteps="FuzzySelector" />
```

### Mode filter examples

Selector strings below are illustrative — use values that match the elements you want to collect.

`FindChildren`:

```xml
<!-- All <li> children directly inside an indicated <ul> list -->
<webctrl tag='LI' />
```

```xml
<!-- All Edit child controls of an indicated dialog window -->
<wnd cls='Edit' />
```

`FindDescendants`:

```xml
<!-- Every link anywhere inside an indicated section/page -->
<webctrl tag='A' />
```

```xml
<!-- Every text input inside an indicated form, regardless of wrappers -->
<webctrl tag='INPUT' type='text' />
```

`FindTopLevels`:

```xml
<!-- All open Notepad top-level windows -->
<wnd app='notepad.exe' />
```

```xml
<!-- All Chrome browser tabs whose title matches "*Gmail*" -->
<html app='chrome.exe' title='*Gmail*' />
```

```xml
<!-- All standard Win32 dialog top-level windows (#32770) -->
<wnd cls='#32770' />
```