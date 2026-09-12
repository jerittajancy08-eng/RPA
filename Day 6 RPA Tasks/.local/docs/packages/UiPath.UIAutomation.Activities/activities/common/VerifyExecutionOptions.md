# VerifyExecutionOptions

`UiPath.UIAutomationNext.Activities.VerifyExecutionOptions`

Define activity execution verification step. Used by activities such as **Click**, **Hover**, and **Keyboard Shortcuts** to confirm the action produced the expected outcome at runtime.

For **Type Into**, see `VerifyExecutionTypeIntoOptions` (documented inline in `TypeInto.md`), which extends this class with an additional `ExpectedText` property.

## Properties

| Property | Display Name | Type | Description |
|----------|-------------|------|-------------|
| `Target` | Target | [`TargetAnchorable`](Target.md#targetanchorable) | Target information that defines the UI element used for verification. |
| `Mode` | Verify mode | [`NVerifyMode`](NVerifyMode.md) | Defines whether to check if the verification target appears or disappears. |
| `Retry` | Retry | `InArgument<bool>` | When selected, the action is retried for the duration of the activity timeout, if the expected outcome was not achieved. |
| `Timeout` | Timeout | `InArgument<double>` | The amount of time (in seconds) to wait for the verification element to appear, disappear, or change. |

## XAML Syntax

```xml
<uix:VerifyExecutionOptions>
  <uix:VerifyExecutionOptions.Target>
    <uix:TargetAnchorable Version="V6" />
  </uix:VerifyExecutionOptions.Target>
  <uix:VerifyExecutionOptions.Mode>Appears</uix:VerifyExecutionOptions.Mode>
  <uix:VerifyExecutionOptions.Retry>
    <InArgument x:TypeArguments="x:Boolean">True</InArgument>
  </uix:VerifyExecutionOptions.Retry>
  <uix:VerifyExecutionOptions.Timeout>
    <InArgument x:TypeArguments="x:Double">10</InArgument>
  </uix:VerifyExecutionOptions.Timeout>
</uix:VerifyExecutionOptions>
```
