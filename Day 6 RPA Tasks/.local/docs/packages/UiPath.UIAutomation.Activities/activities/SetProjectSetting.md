# Set Project Setting

Overrides the project setting with the indicated key and given value. The value takes effect from the moment the activity is executed. Only UIAutomation activities (Classic, Modern and CV) are affected.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NSetProjectSetting`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Key` | Key | InArgument | `string` | **Yes** |  |  | The project setting key to override. Must match an existing key in the project settings schema. |
| `Value` | Value | InArgument | `object` |  |  |  | The value to apply. Must be assignable to or coercible into the declared type of the setting. Leave empty to clear a previously-set override and revert to the base project setting value. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NSetProjectSetting`.

## Notes

- No mandatory parent scope is required for this activity, and it does not target a UI element.
- `Key` is mandatory — leaving it empty produces a design-time validation error and a runtime exception.
- Keys are fully qualified, in the form `UiPath.UIAutomationNext.Activities.<Section>.<Setting>` — the same keys Studio and the Robot use when they read the project settings (for example `UiPath.UIAutomationNext.Activities.Generic.DelayBefore`). A key that is not part of the schema is stored but never read by any activity.
- Leaving `Value` empty clears a previously applied override and restores the value configured in the project settings.
- The override stays in effect for the lifetime of the current process, or until the same key is overridden again.
- Workflows started with **Invoke Workflow File** in isolated mode inherit the override through an environment variable; already-running isolated children are not affected.
- When `Key` is a literal containing a `.`, the activity's display name gets its last segment appended (e.g. `Set Project Setting - DelayBefore`).
- Coded workflow equivalent: `uiAutomation.SetProjectSetting(key, value)` — see [coded-api.md](../coded/coded-api.md#setprojectsetting).
