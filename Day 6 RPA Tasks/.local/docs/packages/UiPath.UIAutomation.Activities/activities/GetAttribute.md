# Get Attribute

Retrieves the value of a specified attribute of the indicated UI element.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Assembly:** `UiPath.UIAutomationNext.Activities`
**Class Name:** `UiPath.UIAutomationNext.Activities.NGetAttributeGeneric`1`
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  | The UI element to perform the action on. |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `Attribute` | Attribute | InArgument | `string` |  |  | The name of the attribute to be retrieved. |
| `HealingAgentBehavior` | Healing Agent mode | InArgument | [`NChildHealingAgentBehavior`](common/NChildHealingAgentBehavior.md) |  |  | Configures the Healing Agent actions if they are allowed by Governance or Orchestrator process/job/trigger level settings |
| `ScopeIdentifier` | Scope identifier | Property | `string` |  |  | Attaches this activity to a specific Application Card. Use the Application Card's `ScopeGuid` property. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `Result` | Save to | `OutArgument<T>` | Where to save the current attribute value. |
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |

### Common

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` |  |  | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `Timeout` | Timeout | InArgument | `double` |  |  | The amount of time (in seconds) to wait for the operation to be performed before generating an error. The default value is 30 seconds. |
| `DelayAfter` | Delay after | InArgument | `double` |  |  | Delay (in seconds) after this activity is completed, before next activity starts. The default amount of time is 0.3 seconds. |
| `DelayBefore` | Delay before | InArgument | `double` |  |  | Delay (in seconds) to wait before executing this activity. The default amount of time is 0.2 seconds. |

## Attributes

The following attributes can be retrieved through the `Attribute` property. The **Availability** column indicates the platforms the attribute is supported on.

| Name | Description | Type | Availability |
|------|-------------|------|--------------|
| `aaname` | The Active Accessibility name. It represent the text that is displayed, such as 'Submit' on a button of 'First Name' for a edit box. (String). | `String` | Windows, CrossPlatform |
| `aastate` | The Active Accessibility state. Identifies the current condition of the control, such as checked for a checkbox. State advises whether a control can be selected, focused, and/or other types of changeable functionality. (String). | `String` | Windows, CrossPlatform |
| `accCtxInfo` | The accessibility context information (String). | `String` | Windows |
| `accessibleClass` | The accessible Java class of the element (String). | `String` | Windows |
| `accessibleFullClass` | The fully qualified accessible Java class of the element (String). | `String` | Windows |
| `allColTooltips` |  | `List<String>` | Windows, CrossPlatform |
| `alt` | The alternate text for an image, if the image cannot be displayed (String). | `String` | Windows |
| `alterIfDisabled` |  | `Boolean` | Windows, CrossPlatform |
| `app` | The process name such as chrome.exe, msedge.exe, java.exe (String). | `String` | Windows, CrossPlatform |
| `AppPath` | The fully qualified path of the process' folder (String). E.g. C:\Program Files (x86)\Google\Chrome\Application. | `String` | Windows, CrossPlatform |
| `aria-label` | The accessibility (ARIA) string that labels the current element (String). Used mostly in cases where a text label is not visible on the screen. | `String` | Windows, CrossPlatform |
| `aria-role` | The accessibility (ARIA) element role (String).  E.g. alert, button, option, radio, scrollbar. | `String` | Windows, CrossPlatform |
| `automationId` | The UI Automation property of supported elements (String). | `String` | Windows |
| `backgroundColor` | The background color of the element formatted as `(Red, Green, Blue, Alpha)` (String). | `String` | Windows, CrossPlatform |
| `baseClassName` | The underlying base Java class of the element (String). | `String` | Windows |
| `browserName` |  | `Int32` | Windows, CrossPlatform |
| `canAccessElementProcess` |  | `Boolean` | Windows, CrossPlatform |
| `canLoadDriverInBrowserExtension` |  | `Boolean` | Windows, CrossPlatform |
| `checked` | Indicates whether the UI element is in the checked state (Boolean). | `Boolean` | Windows, CrossPlatform |
| `class` | The class name(s) for an HTML element (String). | `String` | Windows, CrossPlatform |
| `cls` | It specifies one or more CSS classnames for an element. Or the class name of a native window handle (String). | `String` | Windows, CrossPlatform |
| `colName` |  | `String` | CrossPlatform |
| `colorIndex` |  | `Boolean` | Windows, CrossPlatform |
| `colorIntensified` |  | `Boolean` | Windows, CrossPlatform |
| `colorInverse` |  | `Boolean` | Windows, CrossPlatform |
| `columnCount` |  | `Int32` | Windows, CrossPlatform |
| `componentName` | The underlying Java class of the element (String). | `String` | Windows |
| `cookie` | The cookies associated with the current DOM. It returns all name/value pairs of cookies in the current document (String). | `String` | Windows, CrossPlatform |
| `css-selector` |  | `String` | CrossPlatform |
| `ctrlId` | The identifier of an element with a native window handle (String). | `String` | Windows |
| `ctrlName` | The name of an element with a native window handle (String). | `String` | Windows |
| `data-hide-on-smallscreens` |  | `String` | CrossPlatform |
| `disabled` |  | `Boolean` | Windows, CrossPlatform |
| `displayDpiScaleFactor` |  | `Single` | Windows, CrossPlatform |
| `driverVersionsEmbeddedInBrowserExtension` |  | `List<String>` | Windows, CrossPlatform |
| `editable` | Indicates if the element is editable (Boolean). | `Boolean` | Windows |
| `enabled` | Indicates if the element is enabled (Boolean). | `Boolean` | Windows, CrossPlatform |
| `expanded` | The expanded state of a tree node element. (Boolean). | `Boolean` | Windows, CrossPlatform |
| `focused` | Indicates whether the element has focus or not (Boolean). | `Boolean` | Windows, CrossPlatform |
| `foreground` | An attribute that indicates if the process or browser tab is in foreground or not (Boolean). | `Boolean` | Windows |
| `foregroundColor` | The foreground color of the element formatted as `(Red, Green, Blue, Alpha)` (String). | `String` | Windows, CrossPlatform |
| `fullClassName` | The fully qualified name of the Java class of the element (String). | `String` | Windows |
| `hasFocus` | Indicates whether the element has focus or not (Boolean). | `Boolean` | Windows, CrossPlatform |
| `hasTableAncestor` | Indicates whether the element has an ancestor with role `table` or not (Int32). | `Int32` | Windows, CrossPlatform |
| `helpText` | Identifies the HelpText UIA property (String). | `String` | Windows |
| `hwnd` | It retrieves the native window handle (Int32). | `Int32` | Windows |
| `id` |  | `String` | CrossPlatform |
| `idx` | The index of an element that is used to distinguish elements with the same selector, based on their order in the interface hierarchy. It is automatically calculated for selectors that use the "Find one match" method. | `Int32` | Windows, CrossPlatform |
| `imageColorData` | The image color data of the element (ImageColorData). | `ImageColorData` | Windows |
| `innerHtml` | A property of any HTML element which has for value the content found between the opening tag and ending tag (String). | `String` | Windows, CrossPlatform |
| `innerText` | It returns the text content of the specified element, and all its descendants (String). | `String` | Windows, CrossPlatform |
| `isappcontainer` |  | `Boolean` | Windows, CrossPlatform |
| `isBlockedIEModeTabInEdge` |  | `Boolean` | Windows, CrossPlatform |
| `isbrowser` |  | `Boolean` | Windows, CrossPlatform |
| `isBrowserExtensionInstalled` |  | `Boolean` | Windows, CrossPlatform |
| `isBrowserExtensionNeeded` |  | `Boolean` | Windows, CrossPlatform |
| `isBrowserFileUrlError` |  | `Boolean` | Windows, CrossPlatform |
| `isBrowserRunningWithMultipleProfiles` |  | `Boolean` | Windows, CrossPlatform |
| `isbrowsertabactive` |  | `Boolean` | Windows, CrossPlatform |
| `isBrowserTabContentInaccessible` |  | `Boolean` | Windows, CrossPlatform |
| `isCustomRemoteApp` |  | `Boolean` | Windows, CrossPlatform |
| `isExpanded` | The expanded state of a tree node element. (Boolean). | `Boolean` | Windows, CrossPlatform |
| `isExtensionWorkingForBrowserWindow` |  | `Boolean` | Windows, CrossPlatform |
| `isiewindow` |  | `Boolean` | Windows, CrossPlatform |
| `isIEWithProtectedMode` |  | `Boolean` | Windows, CrossPlatform |
| `isJavaBridgeEnabled` |  | `Boolean` | Windows, CrossPlatform |
| `isJavaWindow` | Checks if the given window implements the Java Accessibility API (Boolean). | `Boolean` | Windows |
| `isMDIChild` |  | `Boolean` | Windows, CrossPlatform |
| `isminimized` |  | `Boolean` | Windows, CrossPlatform |
| `isRemoteApp` |  | `Int32` | Windows, CrossPlatform |
| `isRemoteElement` |  | `Boolean` | Windows, CrossPlatform |
| `isRemotePluginInstalled` |  | `Boolean` | Windows, CrossPlatform |
| `isUiPathJavaBridgeEnabled` | An attribute that indicates if communication with the Java extension is running (Boolean). | `Boolean` | Windows |
| `isValidRemoteDriverVersion` |  | `Boolean` | Windows, CrossPlatform |
| `isValidRemoteRuntimeVersion` |  | `Boolean` | Windows, CrossPlatform |
| `items` | Retrieve a list with the names of all selectable items of a control (List). | `List<String>` | Windows, CrossPlatform |
| `javaState` | A list with all the available states of a java type of control (String). E.g. enabled, visible, editable. | `String` | Windows |
| `labeledBy` | The text label of the element (String). E.g. the static text label for a combo box. | `String` | Windows |
| `legacyAccHelp` | The Legacy Accessible Help value (String). | `String` | Windows |
| `name` | The accessible name of the element (String). | `String` | Windows |
| `outerHtml` | Gets the serialized HTML fragment describing the element including its descendants (String). | `String` | Windows, CrossPlatform |
| `outerText` | The text content of the specified element (String). | `String` | Windows, CrossPlatform |
| `parentClass` | The first non empty class attribute of the ancestors of an HTML element (String). | `String` | Windows, CrossPlatform |
| `parentId` | The first non empty id attribute of the ancestors of an HTML element (String). | `String` | Windows, CrossPlatform |
| `parentName` | The first non empty name attribute of the ancestors of an HTML element (String). | `String` | Windows |
| `PID` | The process identifier (Int32). | `Int32` | Windows |
| `position` | The element position (Rectangle). | `Rectangle` | Windows, CrossPlatform |
| `rdpid` |  | `Int32` | Windows, CrossPlatform |
| `readonly` |  | `Boolean` | Windows, CrossPlatform |
| `readyState` | The loading state of the document (String). | `String` | Windows, CrossPlatform |
| `relativeVisibility` | The (partial) visibility of an element inside all of its ancestors (Boolean) | `Boolean` | Windows |
| `role` | The role of an element (String). E.g. button, link | `String` | Windows, CrossPlatform |
| `rowCount` |  | `Int32` | Windows, CrossPlatform |
| `rowName` |  | `String` | CrossPlatform |
| `sapReadyState` |  | `Boolean` | Windows, CrossPlatform |
| `sapScreen` |  | `Int32` | Windows, CrossPlatform |
| `sapScriptingStatus` |  | `Int32` | Windows, CrossPlatform |
| `sapSession` |  | `Int32` | Windows, CrossPlatform |
| `sapSysNumber` |  | `Int32` | Windows, CrossPlatform |
| `scaleFactor` |  | `Single` | Windows, CrossPlatform |
| `scrollmax` |  | `Int32` | Windows, CrossPlatform |
| `scrollmin` |  | `Int32` | Windows, CrossPlatform |
| `scrollPageSize` |  | `Int32` | Windows, CrossPlatform |
| `scrollpos` |  | `Int32` | Windows, CrossPlatform |
| `selected` | Indicates whether the element is selected or not (Boolean). | `Boolean` | Windows, CrossPlatform |
| `selectedItem` | The selected item of a dropdown element (String). | `String` | Windows, CrossPlatform |
| `selectedItems` |  | `List<String>` | Windows, CrossPlatform |
| `size` |  | `Rectangle` | Windows, CrossPlatform |
| `src` | The location (URL) of the external resource (String). | `String` | Windows, CrossPlatform |
| `subsystem` | The subsystem used to generate the selector (String). The selector's second level tag: webctrl, ctrl, uia, java, silverlight, sap. | `String` | Windows, CrossPlatform |
| `supportsMultipageScraping` |  | `Boolean` | Windows, CrossPlatform |
| `tabindex` |  | `Int32` | CrossPlatform |
| `tableCol` |  | `Int32` | CrossPlatform |
| `tableRow` |  | `Int32` | CrossPlatform |
| `tag` | The starting part of an HTML element (String). E.g. `<input>`, `<p>`, `<a>` | `String` | Windows, CrossPlatform |
| `text` | It refers to all of the font, style, alignment, and other formatting associated with a given character or series of characters (String). | `String` | Windows, CrossPlatform |
| `TID` | The identifier of the thread that created the window containing the element | `Int32` | Windows |
| `title` | It  specifies extra information about an element (String). Most often shown as a tooltip text when the mouse moves over the element. | `String` | Windows, CrossPlatform |
| `treeDepth` | The depth inside the parent tree of a node element (Int32). | `Int32` | Windows, CrossPlatform |
| `type` |  | `String` | CrossPlatform |
| `ui5-allColLabels` |  | `List<String>` | Windows, CrossPlatform |
| `uipath-web-automation-framework` |  | `NDriverWebFramework` | Windows, CrossPlatform |
| `url` | It specifies the URL of the browser tab (String). | `String` | Windows, CrossPlatform |
| `virtualName` | The accessible virtual name of the element (String). E.g. the static text label for a combo box. | `String` | Windows |
| `visibility` | It specifies a value corresponding to the following visibility levels: 0 - Visible, 1 - Hidden, 2 - Offscreen, 3 - Partial (NVisibilityLevel). | `NVisibilityLevel` | Windows |
| `visible` | Indicates if the element is visible (Boolean). | `Boolean` | Windows |
| `visibleInnerText` |  | `String` | CrossPlatform |
| `wndExtStyles` |  | `Int64` | Windows, CrossPlatform |
| `wndStyles` |  | `Int64` | Windows, CrossPlatform |

## XAML Example

Run `uip rpa --help` to find the command that retrieves the default XAML for an activity, then run it with the class name ``UiPath.UIAutomationNext.Activities.NGetAttributeGeneric`1``.

## Notes

- This activity must be placed inside a **Use Application/Browser** (`NApplicationCard`) scope.
- This is a generic activity; the type argument determines the output type of the retrieved attribute value.
- Common attributes include `href`, `class`, `id`, `value`, `innerText`, `title`, etc.
