# DialogHandling

Configure auto-dismissal of JavaScript dialogs (`alert()`, `confirm()`, `prompt()`) raised by the page during automation. Used by **Use Application/Browser** (`NApplicationCard`) and **SAP Logon**.

The same shape exists in two forms — the activity (XAML) form wraps each value in `InArgument<T>`, while the coded API uses raw values:

- **Activity:** `UiPath.UIAutomationNext.DialogHandling`
- **Coded API:** `UiPath.UIAutomationNext.API.Models.DialogHandlingOptions`

## Properties

| Property | Activity Type | Coded API Type | Description |
|----------|--------------|----------------|-------------|
| `DismissAlerts` | `InArgument<bool>` | `bool` | Enable auto-dismissal of JavaScript alert dialogs. Default: `false`. |
| `DismissConfirms` | `InArgument<bool>` | `bool` | Enable auto-dismissal of JavaScript confirm dialogs (OK/Cancel). Closed using `ConfirmDialogResponse`. Default: `false`. |
| `DismissPrompts` | `InArgument<bool>` | `bool` | Enable auto-dismissal of JavaScript prompt dialogs (OK/Cancel + text input). Closed using `PromptDialogResponse`; `PromptDialogResponseText` provides the input text. Default: `false`. |
| `ConfirmDialogResponse` | `InArgument<`[`NBrowserDialogResponse`](NBrowserDialogResponse.md)`>` | [`NBrowserDialogResponse`](NBrowserDialogResponse.md) | Response sent for confirm dialogs. Used only when `DismissConfirms` is `true`. Default: `Cancel`. |
| `PromptDialogResponse` | `InArgument<`[`NBrowserDialogResponse`](NBrowserDialogResponse.md)`>` | [`NBrowserDialogResponse`](NBrowserDialogResponse.md) | Response sent for prompt dialogs. Used only when `DismissPrompts` is `true`. Default: `Cancel`. |
| `PromptDialogResponseText` | `InArgument<string>` | `string` | Text response entered into the prompt dialog's input field. Used only when `DismissPrompts` is `true`. Default: empty string. |

## XAML (activity)

```xml
<uix:NApplicationCard.DialogHandling>
  <uix:DialogHandling
      DismissAlerts="True"
      DismissConfirms="True"
      DismissPrompts="True"
      ConfirmDialogResponse="OK"
      PromptDialogResponse="OK"
      PromptDialogResponseText="auto-response" />
</uix:NApplicationCard.DialogHandling>
```

## Coded API

```csharp
var dialogHandling = new DialogHandlingOptions
{
    DismissAlerts = true,
    DismissConfirms = true,
    PromptDialogResponse = NBrowserDialogResponse.OK,
    PromptDialogResponseText = "auto-response"
};
```
