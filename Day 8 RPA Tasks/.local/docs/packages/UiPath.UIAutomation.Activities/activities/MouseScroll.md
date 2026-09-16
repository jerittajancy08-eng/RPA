# Mouse Scroll

Sends mouse scroll events to the specified UI element.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NMouseScroll`
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  | The UI element to perform the action on. |
| `CursorMotionType` | Cursor motion type | InArgument | [`CursorMotionType`](common/CursorMotionType.md) |  |  |  | Specifies the type of motion performed by the mouse cursor. There are two options: Instant - the cursor jumps to the destination, and Smooth - the cursor moves in increments. Setting has effect only if input method Hardware Events is used. The default option is Instant. |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |
| `SearchedElement.InUiElement` | Input searched element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | An existing UI element reference to use as the searched element. The mouse scroll continues until this element becomes visible. See [Scroll modes](#scroll-modes). |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `Direction` | Direction | InArgument | [`NScrollDirection`](common/NScrollDirection.md) | `Left` |  | Specifies the type of scroll to be performed with the mouse wheel. |
| `MovementUnits` | # of scrolls | InArgument | `int` |  | Yes | The number of scrolls. If scrolling to a specific element, the element is searched after all rotations are executed. See [Scroll modes](#scroll-modes). |
| `SearchedElement` | Searched element | Property | [`SearchedElement`](common/SearchedElement.md) |  |  | Define the element that must found and visible on screen while scrolling. See [Scroll modes](#scroll-modes). |
| `KeyModifiers` | Key modifiers | InArgument | [`NKeyModifiers`](common/NKeyModifiers.md) |  |  | One or more key modifiers to use in combination with the mouse scroll action. |
| `InteractionMode` | Input mode | InArgument | [`NChildInteractionMode`](common/NChildInteractionMode.md) |  |  | The method used to execute the scroll action. |
| `HealingAgentBehavior` | Healing Agent mode | InArgument | [`NChildHealingAgentBehavior`](common/NChildHealingAgentBehavior.md) |  |  | Configures the Healing Agent actions if they are allowed by Governance or Orchestrator process/job/trigger level settings |
| `ScopeIdentifier` | Scope identifier | Property | `string` |  |  | Attaches this activity to a specific Application Card. Use the Application Card's `ScopeGuid` property. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |
| `SearchedElement.OutUiElement` | Output searched element | [`UiElement`](common/UiElement.md) | The searched UI element reference, to use in other activities as in Input UI Element. |

### Common

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` |  |  | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `Timeout` | Timeout | InArgument | `double` |  |  | The amount of time (in seconds) to wait for the operation to be performed before generating an error. The default value is 30 seconds. |
| `SearchedElement.Timeout` | Searched element timeout | InArgument | `double` | `0.2` |  | The amount of time (in seconds) to wait for the element to appear after each scroll action. |
| `DelayAfter` | Delay after | InArgument | `double` |  |  | Delay (in seconds) after this activity is completed, before next activity starts. The default amount of time is 0.3 seconds. |
| `DelayBefore` | Delay before | InArgument | `double` |  |  | Delay (in seconds) to wait before executing this activity. The default amount of time is 0.2 seconds. |

## Sub-Objects

See [`SearchedElement`](common/SearchedElement.md) for the full property table and XAML nested-element syntax. `SearchedElement.Target`, `SearchedElement.InUiElement`, `SearchedElement.OutUiElement` and `SearchedElement.Timeout` are nested inside `NMouseScroll.SearchedElement`, not attributes of the activity.

## Scroll modes

The activity has two modes, selected by whether `SearchedElement` is set.

### SearchedElement not set — scroll by distance

The activity scrolls `Target` exactly `MovementUnits` times in `Direction` and completes. Only `OutUiElement` is produced.

### SearchedElement set — scroll to element

`SearchedElement.Target` or `SearchedElement.InUiElement` must be set. An empty `SearchedElement` block still selects this mode and fails validation with `Searched element is required when "Scroll type" is "To element".`, then at runtime with `Searched element Target or Input UI Element must be set when Scroll type is set to To element.` "Scroll type" in both messages refers to whether `SearchedElement` is set; it is not a XAML member. For distance mode, remove the whole `NMouseScroll.SearchedElement` block rather than leaving it empty.

The searched element is looked up once before any scrolling. When it is found on that first check it is scrolled into view and the activity completes, so `Direction` and `MovementUnits` are not used. Otherwise the activity scrolls `MovementUnits` times in `Direction`, waits up to `SearchedElement.Timeout` for the element, and repeats until it is found or `Timeout` expires, in which case it fails with `Unable to find the searched element.`

The element that was found is written to `SearchedElement.OutUiElement`.

## XAML Examples

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NMouseScroll`.

### Scroll by distance

Omit `SearchedElement` entirely for distance mode:

```xml
<uix:NMouseScroll Direction="Down"
                  InteractionMode="HardwareEvents"
                  MovementUnits="10" />
```

### Scroll to an input element

Setting `SearchedElement` selects to-element mode. This example assumes `searchedElement` is an in-scope `UiElement` variable:

```xml
<uix:NMouseScroll Direction="Down"
                  InteractionMode="HardwareEvents"
                  MovementUnits="10">
  <uix:NMouseScroll.SearchedElement>
    <uix:SearchedElement InUiElement="[searchedElement]" />
  </uix:NMouseScroll.SearchedElement>
</uix:NMouseScroll>
```

### Scroll to a target element

`SearchedElement.Target` is a [`TargetAnchorable`](common/Target.md#targetanchorable) nested inside the `SearchedElement` block — not an attribute of the activity. Do not hand-write it. Author the activity with a unique `sap2010:WorkflowViewState.IdRef` and no `SearchedElement.Target` child, then attach the Object Repository element with `link-elements` using `"targetProperty": "SearchedElement.Target"` — see [Target Attachment Guide](../references/uia-target-attachment-guide.md).

```xml
<uix:NMouseScroll Direction="Down"
                  MovementUnits="10"
                  sap2010:WorkflowViewState.IdRef="NMouseScroll_1">
  <uix:NMouseScroll.SearchedElement>
    <uix:SearchedElement Timeout="1">
      <!-- SearchedElement.Target is attached by link-elements. Do not hand-write it. -->
    </uix:SearchedElement>
  </uix:NMouseScroll.SearchedElement>
</uix:NMouseScroll>
```

## Notes

- This activity must be placed inside a **Use Application/Browser** (`NApplicationCard`) scope.
- `MovementUnits` must be `1` or greater in both modes. When it is not set it evaluates to `0`, and validation and execution report `Value for property [# of scrolls] can not be lower than 1.`
- When `Direction` is not set, `Left` is used.
- `KeyModifiers` (for example, Ctrl or Shift) affect scrolling only with `HardwareEvents`, and are ignored with `DebuggerApi`.
- `Simulate` and `WindowMessages` are not supported by this activity. With `Simulate`, execution fails with `Action not supported by this type of element. Please use another type of scroll.` — this also applies when `SameAsCard` resolves to `Simulate`.
