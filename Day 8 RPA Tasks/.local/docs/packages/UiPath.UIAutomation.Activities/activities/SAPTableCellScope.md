# Table Cell Scope

A container that enables you to attach to an existing Table UI element and perform multiple actions within it.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.SAP
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NSAPTableCellScope`
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  | The UI element to perform the action on. |
| `ColumnName` | Column Name / Filter | InArgument | `string` |  |  |  | Specifies Column name from the table. After indicating the table cell, the list with all available Column names is displayed in the activity. |
| `RowIndex` | Row index | InArgument | `uint` |  |  |  | Specifies the row index |
| `RowSelector` | Row selector | InArgument | `string` |  |  |  | Specifies the row selector/filter to use |
| `RowType` | Row input type | InArgument | [`NSAPTableCellScopeRowType`](#nsaptablecellscoperowtype) |  |  |  | Specifies how the row should be searched: by index, by selector/filter or first empty row |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `HealingAgentBehavior` | Healing Agent mode | InArgument | [`NChildHealingAgentBehavior`](common/NChildHealingAgentBehavior.md) |  |  | Configures the Healing Agent actions if they are allowed by Governance or Orchestrator process/job/trigger level settings |
| `ScopeIdentifier` | Scope identifier | Property | `string` |  |  | Attaches this activity to a specific Application Card. Use the Application Card's `ScopeGuid` property. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `TableRow` | Table Row Index | `uint` | The found row index in case "First Empty Row" is used. This is equal to the Row Number input otherwise. This field only supports uint variables. |
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` |  | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `Timeout` | Timeout | InArgument | `double` |  | The amount of time (in seconds) to wait for the operation to be performed before generating an error. The default value is 30 seconds. |
| `DelayAfter` | Delay after | InArgument | `double` |  | Delay (in seconds) after this activity is completed, before next activity starts. The default amount of time is 0.3 seconds. |
| `DelayBefore` | Delay before | InArgument | `double` |  | Delay (in seconds) to wait before executing this activity. The default amount of time is 0.2 seconds. |

## Enums

### NSAPTableCellScopeRowType

`UiPath.UIAutomationNext.Enums.NSAPTableCellScopeRowType`

How a row is identified by **SAP Table Cell Scope**.

| Value | Description |
|-------|-------------|
| `NSAPTableCellScopeRowType.SelectorFilter` | Use a selector/filter for the row. |
| `NSAPTableCellScopeRowType.Index` | Use a row index. |
| `NSAPTableCellScopeRowType.FirstEmptyRow` | Select the first empty row. |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name `UiPath.UIAutomationNext.Activities.NSAPTableCellScope`.

## Notes

- Before automating SAP, read [`sap-guide.md`](../references/sap-guide.md).
- This activity must be placed inside a `UiPath.UIAutomationNext.Activities.NApplicationCard` scope.
- This activity is a scope/container. Place child activities inside the body to perform actions on the specified table cell.
