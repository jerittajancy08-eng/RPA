# Application Event Trigger

Setup a trigger on a given event on the indicated UI Element.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NNativeEventTrigger`1`
**Where to use:** see [XAML Example](#xaml-example) — two valid XAML shapes. `Use Application/Browser` scope is optional.

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  | The UI element to perform the action on. |
| `EventType` | Event type | Property | [`NNativeEventType`](#nnativeeventtype) | Yes | `None` |  | Indicates the event type that triggers the activity. See [NNativeEventType](#nnativeeventtype). |
| `MatchSync` | Match sync | Property | `bool` |  | `false` |  | Indicates whether the matching of the target element selector is done synchronously or asynchronously. |
| `Selectors` | Selectors | InArgument | `IEnumerable<string>` |  |  |  | Optional collection of selectors to monitor for the indicated event; these selectors will be monitored alongside the indicated target. |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `IncludeChildren` | Include children | InArgument | `bool` |  |  | When selected, the children of the specified UI element are also monitored. By default, this check box is selected. |
| `SchedulingMode` | Scheduling mode | Property | [`TriggerActionSchedulingMode`](common/TriggerActionSchedulingMode.md) |  |  | It specifies how to execute the actions when a trigger is fired. Sequential: actions are executed one after another; Concurrent: actions execution can overlap; OneTime: executes one action and exits monitoring. For Sequential and Concurrent modes the monitoring continues until either the user stops the execution or a Break activity is met. |

## Enums

### NNativeEventType

`UiPath.UIAutomationNext.Triggers.NNativeEventType`

The native event type that fires the **Application Event Trigger**. Values are grouped by target technology — pick the prefix that matches the technology of your `Target` (Web/UIA/Java/AA/Window). `None` is invalid (validation error).

#### Values

| Value | Description |
|-------|-------------|
| `NNativeEventType.ElementAppeared` | Fires when the target element appears. Works with any target technology. |
| `NNativeEventType.ElementDisappeared` | Fires when the target element disappears. Works with any target technology. |
| `NNativeEventType.KeyPress` | Fires on a key press on the target. Works with any target technology. |
| `NNativeEventType.MouseClick` | Fires on a mouse click on the target. Works with any target technology. |
| `NNativeEventType.JavaTextChange` | Fires when text changes on a Java AWT/Swing target. |
| `NNativeEventType.JavaKeyPress` | Fires on a key press on a Java target. |
| `NNativeEventType.JavaMouseClick` | Fires on a mouse click on a Java target. |
| `NNativeEventType.JavaMouseMove` | Fires when the mouse moves over a Java target. |
| `NNativeEventType.JavaFocusGained` | Fires when a Java target gains focus. |
| `NNativeEventType.JavaFocusLost` | Fires when a Java target loses focus. |
| `NNativeEventType.JavaEnabled` | Fires when a Java target becomes enabled. |
| `NNativeEventType.JavaDisabled` | Fires when a Java target becomes disabled. |
| `NNativeEventType.JavaSelectionChanged` | Fires when the selection changes on a Java target. |
| `NNativeEventType.JavaMouseEntered` | Fires when the mouse enters a Java target. |
| `NNativeEventType.JavaMouseExited` | Fires when the mouse exits a Java target. |
| `NNativeEventType.JavaCellSelected` | Fires when a Java table/list cell is selected. |
| `NNativeEventType.JavaCellValueChanged` | Fires when a Java table/list cell value changes. |
| `NNativeEventType.JavaMousePressed` | Fires on mouse button press over a Java target. |
| `NNativeEventType.JavaMouseReleased` | Fires on mouse button release over a Java target. |
| `NNativeEventType.JavaMenuSelected` | Fires when a Java menu is selected. |
| `NNativeEventType.JavaMenuDeselected` | Fires when a Java menu is deselected. |
| `NNativeEventType.JavaMenuCanceled` | Fires when a Java menu is canceled. |
| `NNativeEventType.JavaPopupMenuVisible` | Fires when a Java popup menu becomes visible. |
| `NNativeEventType.JavaPopupMenuInvisible` | Fires when a Java popup menu becomes invisible. |
| `NNativeEventType.JavaPopupMenuCanceled` | Fires when a Java popup menu is canceled. |
| `NNativeEventType.CtrlTextChange` | Fires when text changes on an AA / generic control. |
| `NNativeEventType.CtrlTitleChange` | Fires when the title changes on an AA / generic control. |
| `NNativeEventType.CtrlFocusGained` | Fires when an AA / generic control gains focus. |
| `NNativeEventType.CtrlFocusLost` | Fires when an AA / generic control loses focus. |
| `NNativeEventType.CtrlStateChanged` | Fires when the state of an AA / generic control changes. |
| `NNativeEventType.CtrlSelectionChanged` | Fires when the selection changes on an AA / generic control. |
| `NNativeEventType.CtrlLocationChanged` | Fires when the location of an AA / generic control changes. |
| `NNativeEventType.ForegroundGained` | Fires when the target window gains foreground. |
| `NNativeEventType.ForegroundLost` | Fires when the target window loses foreground. |
| `NNativeEventType.WndMinimized` | Fires when the target window is minimized. |
| `NNativeEventType.WndRestored` | Fires when the target window is restored. |
| `NNativeEventType.WebctrlTextChange` | Fires when text changes on a web target. |
| `NNativeEventType.WebctrlTextSelectionChanged` | Fires when the text selection changes on a web target. |
| `NNativeEventType.WebctrlKeyPress` | Fires on a key press on a web target. |
| `NNativeEventType.WebctrlMouseClick` | Fires on a mouse click on a web target. |
| `NNativeEventType.WebctrlMouseDoubleClick` | Fires on a mouse double-click on a web target. |
| `NNativeEventType.WebctrlMouseRightClick` | Fires on a right-click on a web target. |
| `NNativeEventType.WebctrlMouseMove` | Fires when the mouse moves over a web target. |
| `NNativeEventType.WebctrlMousePressed` | Fires on mouse button press on a web target. |
| `NNativeEventType.WebctrlMouseReleased` | Fires on mouse button release on a web target. |
| `NNativeEventType.WebctrlMouseEntered` | Fires when the mouse enters a web target. |
| `NNativeEventType.WebctrlMouseExited` | Fires when the mouse exits a web target. |
| `NNativeEventType.WebctrlFocusGained` | Fires when a web target gains focus. |
| `NNativeEventType.WebctrlFocusLost` | Fires when a web target loses focus. |
| `NNativeEventType.WebctrlDownloadChanged` | Fires when a download state changes in the browser. |
| `NNativeEventType.WebctrlTabActivated` | Fires when a browser tab is activated. |
| `NNativeEventType.WebctrlTabCreated` | Fires when a browser tab is created. |
| `NNativeEventType.WebctrlTabRemoved` | Fires when a browser tab is removed. |
| `NNativeEventType.WebctrlTabUpdated` | Fires when a browser tab is updated. |
| `NNativeEventType.WebctrlTabDialogOpening` | Fires when a dialog opens in a browser tab. |
| `NNativeEventType.WebctrlTabDialogClosed` | Fires when a dialog closes in a browser tab. |
| `NNativeEventType.WebctrlTabNavigationStarted` | Fires when navigation starts in a browser tab. |
| `NNativeEventType.WebctrlTabNavigationCompleted` | Fires when navigation completes in a browser tab. |
| `NNativeEventType.WebctrlWindowFocusChanged` | Fires when the focused browser window changes. |
| `NNativeEventType.WebctrlWindowBoundsChanged` | Fires when the browser window bounds change. |
| `NNativeEventType.WebctrlWindowCreated` | Fires when a browser window is created. |
| `NNativeEventType.WebctrlWindowRemoved` | Fires when a browser window is removed. |
| `NNativeEventType.WebctrlWebRequestBeforeRedirect` | Fires before a web request is redirected. |
| `NNativeEventType.WebctrlWebRequestBeforeRequest` | Fires before a web request is sent. |
| `NNativeEventType.WebctrlWebRequestBeforeSendHeaders` | Fires before request headers are sent. |
| `NNativeEventType.WebctrlWebRequestCompleted` | Fires when a web request completes. |
| `NNativeEventType.WebctrlWebRequestErrorOccurred` | Fires when a web request errors. |
| `NNativeEventType.WebctrlWebRequestHeadersReceived` | Fires when response headers are received. |
| `NNativeEventType.WebctrlWebRequestResponseStarted` | Fires when the web response starts. |
| `NNativeEventType.WebctrlWebRequestSendHeaders` | Fires when request headers are sent. |
| `NNativeEventType.UiaFocusGained` | Fires when a UIA desktop target gains focus. |
| `NNativeEventType.UiaFocusLost` | Fires when a UIA desktop target loses focus. |
| `NNativeEventType.UiaInvoked` | Fires when a UIA desktop target is invoked. |
| `NNativeEventType.UiaTextChanged` | Fires when text changes on a UIA desktop target. |
| `NNativeEventType.UiaTextSelectionChanged` | Fires when the text selection changes on a UIA desktop target. |
| `NNativeEventType.UiaSelected` | Fires when a UIA desktop target is selected. |
| `NNativeEventType.UiaDeselected` | Fires when a UIA desktop target is deselected. |
| `NNativeEventType.UiaToggled` | Fires when a UIA desktop target is toggled. |
| `NNativeEventType.SapWebPageMonitor` | Fires when a SAP web page event is monitored. |

#### Usage

Reference values as `NNativeEventType.<Value>`, e.g. `NNativeEventType.WebctrlMouseClick`.

Display names overlap across technologies ("Mouse released", "Text changed", "Focus gained" each exist for Java/Ctrl/Webctrl/UIA). Always pick by the prefix that matches your target's technology, not by the display name:

| Target technology      | Prefix      | Example: "Mouse released" → |
|------------------------|-------------|------------------------------|
| Browser / web page     | `Webctrl*`  | `WebctrlMouseReleased`       |
| UIA desktop            | `Uia*`      | — (no Released event; use `UiaInvoked`) |
| Java AWT/Swing         | `Java*`     | `JavaMouseReleased`          |
| AA / generic control   | `Ctrl*`     | — (no Released event)        |
| Windows / foreground   | `Wnd*`      | — (window-level events only) |
| Any / agnostic         | *(none)*    | — (only `KeyPress`, `MouseClick`, `ElementAppeared`, `ElementDisappeared`) |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name ``UiPath.UIAutomationNext.Activities.NNativeEventTrigger`1``.

> **IMPORTANT:** `EventType` is required. Leaving it at `None` fails validation with `"Field 'Event type' must have a value."` Match the value to the target's technology (see [NNativeEventType](#nnativeeventtype) above).

### Valid placements (pick one)

The platform trigger framework rejects any other placement with *"Trigger must be the first activity in the workflow."*

**1. First child of a top-level `Sequence`** — no wrapper needed, but the trigger must be the first activity:

```xml
<Sequence>
  <uix:NNativeEventTrigger x:TypeArguments="uix:EmptyArgs"
                          EventType="WebctrlMouseClick"
                          sap2010:WorkflowViewState.IdRef="NNativeEventTrigger_1"> ... </uix:NNativeEventTrigger>
  <!-- subsequent activities run after the trigger fires -->
</Sequence>
```

**2. Inside `TriggerScope.Triggers`** — required when the trigger is *not* the first activity, or when you want a dedicated handler body. The `TriggerScope` itself can be nested anywhere:

```xml
<ui:TriggerScope DisplayName="Trigger Scope" SchedulingMode="OneTime"
                sap2010:WorkflowViewState.IdRef="TriggerScope_1">
  <ui:TriggerScope.Action>
    <ActivityAction x:TypeArguments="uix:EmptyArgs">
      <ActivityAction.Argument>
        <DelegateInArgument x:TypeArguments="uix:EmptyArgs" Name="args" />
      </ActivityAction.Argument>
      <Sequence>
        <!-- handler activities run here when the trigger fires -->
      </Sequence>
    </ActivityAction>
  </ui:TriggerScope.Action>
  <ui:TriggerScope.Triggers>
    <scg:List x:TypeArguments="Activity" Capacity="1">
      <uix:NNativeEventTrigger x:TypeArguments="uix:EmptyArgs"
                              EventType="WebctrlMouseClick"
                              sap2010:WorkflowViewState.IdRef="NNativeEventTrigger_1"> ... </uix:NNativeEventTrigger>
    </scg:List>
  </ui:TriggerScope.Triggers>
</ui:TriggerScope>
```

> **IMPORTANT:** `TriggerScope` is `UiPath.Core.Activities.TriggerScope`, so it must use the `ui:` prefix (`xmlns:ui="http://schemas.uipath.com/workflow/activities"`).

The generic argument is the trigger's `TArgs` type — `uix:EmptyArgs` for the default factory. It must be the same type on the trigger, on `ActivityAction`, and on `DelegateInArgument`. `TriggerScope.SchedulingMode` takes `ActionSchedulingMode` values (`Sequential`, `Concurrent`, `OneTime`, `SequentialCollapse`, `SequentialDrop`), which is a different enum from the trigger's own `Scheduling mode`.

## Notes

- Use this activity to set up triggers based on native application events on a specified UI element.
- The `Match sync` property controls whether target element matching occurs synchronously or asynchronously.
- `MatchSync` and `IncludeChildren` are only effective for event types whose feature flags declare them.
- Additional selectors can be provided via the `Selectors` property to monitor multiple elements for the same event.
- The `Scheduling mode` controls how multiple trigger firings are handled (sequentially, concurrently, or one-time). When inside a `TriggerScope`, `Scheduling mode` and `Enabled` are controlled by the scope and hidden on the trigger.
- `Use Application/Browser` scope is **optional**.
