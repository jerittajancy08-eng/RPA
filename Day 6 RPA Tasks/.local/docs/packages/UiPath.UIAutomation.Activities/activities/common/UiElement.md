# UiElement

`UiPath.Core.UiElement`

A runtime handle to a UI element. Almost every UI Automation activity accepts a `UiElement` as input (`InUiElement`) and returns one as output (`OutUiElement`), so an element resolved by one activity can be passed to subsequent activities without re-targeting.

## Usage

`UiElement` is an opaque type — its public surface is intentionally minimal. You don't construct it directly; you obtain one from an activity output (or as the result of a coded API call) and feed it into the next activity.

### As input

When you set the `InUiElement` property of an activity, the activity skips its own targeting and operates on the supplied element directly. The activity's `Target` is then ignored.

### As output

`OutUiElement` returns the element the activity acted on. Pipe it into the `InUiElement` of another activity to reuse the same element handle and avoid re-resolving the selector.

### XAML

```xml
<!-- Producer: Click resolves a button and outputs the element -->
<uix:NClick InUiElement="{x:Null}" OutUiElement="[btnElement]" Version="V5">
  <uix:NClick.Target>
    <uix:TargetAnchorable Version="V6" />
  </uix:NClick.Target>
</uix:NClick>

<!-- Consumer: another activity uses the element directly -->
<uix:NHover InUiElement="[btnElement]" Version="V5" />
```

### Coded API

```csharp
UiElement element = uiAutomation.Click(uiAutomation.GetUiElement("Login Button"));
// reuse the element for further actions
uiAutomation.Hover(element);
```

## Notes

- A `UiElement` is tied to its originating runtime and process; it cannot be serialized across job boundaries.
- Treat `UiElement` references as short-lived. If the underlying UI changes (page navigation, control re-render), the handle may become stale and the next activity will throw.
- The `UiElement` type lives in the `UiPath.Core` namespace (legacy) and is shared across UIA activity versions.
