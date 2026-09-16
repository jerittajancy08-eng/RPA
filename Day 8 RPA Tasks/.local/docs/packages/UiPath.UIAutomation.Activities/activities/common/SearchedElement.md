# SearchedElement

Defines an element that must be found and visible on screen during a scrolling or polling action. Used by **Mouse Scroll** (the scroll loop runs until the searched element appears, or the timeout expires).

The same shape exists in two forms — the activity (XAML) form wraps each value in `InArgument<T>` and exposes additional element handles, while the coded API form is minimal:

- **Activity:** `UiPath.UIAutomationNext.Activities.SearchedElement`
- **Coded API:** `UiPath.UIAutomationNext.API.Models.SearchedElementOptions`

## Properties

| Property | Activity Type | Coded API Type | Description |
|----------|--------------|----------------|-------------|
| `Target` | [`TargetAnchorable`](Target.md#targetanchorable) | `TargetAnchorableModel` | Target information that defines the searched UI element. |
| `Timeout` | `InArgument<double>` | `double` | The amount of time (in seconds) to wait for the element to appear after each scroll action. Default: `0.2`. |
| `OutUiElement` | `OutArgument<`[`UiElement`](UiElement.md)`>` | — | (Activity only) Output handle to the searched UI element, usable as input to other activities. |
| `InUiElement` | `InArgument<`[`UiElement`](UiElement.md)`>` | — | (Activity only) Existing UI element reference to use as the searched element instead of resolving the target. |

## XAML (activity)

```xml
<uix:NMouseScroll.SearchedElement>
  <uix:SearchedElement>
    <uix:SearchedElement.Target>
      <uix:TargetAnchorable Version="V6" />
    </uix:SearchedElement.Target>
    <uix:SearchedElement.Timeout>
      <InArgument x:TypeArguments="x:Double">5</InArgument>
    </uix:SearchedElement.Timeout>
  </uix:SearchedElement>
</uix:NMouseScroll.SearchedElement>
```

## Coded API

```csharp
var searched = new SearchedElementOptions
{
    Target = uiAutomation["loadMoreButton"], // TargetAnchorableModel
    Timeout = 5
};
```
