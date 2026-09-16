# Inject Js Script

Executes JavaScript code in the context of the web page corresponding to a UiElement.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Browser
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NInjectJsScript`
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  | The UI element to perform the action on. |
| `InputParameter` | Input parameter | InArgument | `string` |  |  |  | Input data for the JavaScript code. See [Script format](#script-format). |
| `ScriptCode` | Script code | InArgument | `string` |  |  |  | The JavaScript code you want to run. You can write it here as a string, or add the full path of a .js file. See [Script format](#script-format). |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `ExecutionWorld` | Execution world | InArgument | [`NExecutionWorld`](common/NExecutionWorld.md) |  |  | The JavaScript environment for the script execution. Isolated option allows access to the HTML elements, but prevents access to page variables and code. Use this option to ensure that the script execution does not conflict with the page. Page option allows access to the HTML elements, page variables and code. Use this option if you need to access page variables (e.g. jQuery $) or to interact with page code (e.g. window.alert). |
| `ScopeIdentifier` | Scope identifier | Property | `string` |  |  | Attaches this activity to a specific Application Card. Use the Application Card's `ScopeGuid` property. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `ScriptOutput` | Script output | `OutArgument` | String result returned from JavaScript code. See [Script format](#script-format). |
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |

### Common

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` |  |  | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `Timeout` | Timeout | InArgument | `double` |  |  | The amount of time (in seconds) to wait for the operation to be performed before generating an error. The default value is 30 seconds. |
| `DelayAfter` | Delay after | InArgument | `double` |  |  | Delay (in seconds) after this activity is completed, before next activity starts. The default amount of time is 0.3 seconds. |
| `DelayBefore` | Delay before | InArgument | `double` |  |  | Delay (in seconds) to wait before executing this activity. The default amount of time is 0.2 seconds. |

## Script format

The value supplied to `ScriptCode` must be a JavaScript **function expression**. The activity invokes the function with two arguments — `element` (the resolved target/input UI element in the page context) and `input` (the value of `InputParameter`) — and writes the function's return value into the `ScriptOutput` `OutArgument`.

### Script code

- `ScriptCode` must be a JavaScript function expression — not a script body, not a statement list. The activity invokes the function and uses its return value.
- The function expression can be supplied either inline as a string or as the full path of a `.js` file whose contents are a function expression.
- The function receives two parameters, in this order:
  - `element` — the resolved UI element the activity is bound to (the target, or `InUiElement` when set).
  - `input` — the value of `InputParameter`, always a string (see below).
- The function should return a `string`. Returning nothing is allowed and yields an empty string in `ScriptOutput` (see [Output](#output-scriptoutput)).

This matches the default template the activity ships with: `function (element, input) { … }`.

```js
function (element, input) {
    const sec = element || document.querySelector('[data-automation-id="pex-team-highlights-section"]');
    if (!sec) return '';
    const t = sec.querySelector('button[aria-label="View More"], button[aria-label="View Less"]');
    return t ? (t.getAttribute('aria-label') || '') : '';
}
```

### Input (`InputParameter`)

- The type is `string`. Always.
- To pass an **object** or **array**, send it as a JSON string and call `JSON.parse(input)` as the first step inside the function.
- To pass a **primitive** (number, boolean, etc.), convert it to its string representation; parse it inside the function if you need the original type back (`Number(input)`, `input === "true"`, …).
- If `InputParameter` is left empty (or null), the runtime normalizes it to an empty string before invoking the script — `input` will always be a `string`, never `null`/`undefined`.

```js
// Object input — caller passes JSON string '{"id":42,"name":"row"}'
function (element, input) {
    const data = JSON.parse(input);
    const row = document.querySelector(`[data-id="${data.id}"]`);
    return row ? row.textContent : '';
}
```

```js
// Primitive input — caller passes "42"
function (element, input) {
    const index = Number(input);
    const items = document.querySelectorAll('.item');
    return items[index]?.textContent ?? '';
}
```

```js
// No input — InputParameter is not set on the activity (input is "")
function (element, input) {
    const items = document.querySelectorAll('.item');
    return String(items.length);
}
```

### Output (`ScriptOutput`)

- The function should return a `string`; that value is written into the `ScriptOutput` `OutArgument`.
- If the function returns nothing (`undefined` / no `return`), `ScriptOutput` is set to an empty string (`""`).

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NInjectJsScript`.

## Notes

- This activity must be placed inside a **Use Application/Browser** (`NApplicationCard`) scope.
- The `ExecutionWorld` property controls the JavaScript execution environment: **Isolated** prevents conflicts with page code, while **Page** allows access to page variables and functions.
- Use `InputParameter` to pass data into the JavaScript code, accessible via the first function argument.
- The script must return a value using `return` to populate the `ScriptOutput`.
