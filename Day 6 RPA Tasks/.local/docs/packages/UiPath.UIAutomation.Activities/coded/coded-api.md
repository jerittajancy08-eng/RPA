# UI Automation Coded API

- **Service accessor:** `uiAutomation`
- **Type:** `IUiAutomationAppService`
- **Package:** `UiPath.UIAutomation.Activities`

## Architecture

Two layers:

1. **Service layer (`IUiAutomationAppService`)** -- accessed via `uiAutomation`. Top-level operations: `Attach`/`Open` (return `UiTargetApp` handle), element-level actions accepting [`UiElement`](../activities/common/UiElement.md) directly (e.g., `Click(UiElement)`, `TypeInto(UiElement)`), plus global utilities `SetRuntimeBrowser`, `SetProjectSetting`, `SetCVServer`, `KeyboardShortcut`, `TakeScreenshot`, `GetClipboard`/`SetClipboard`, `WaitState`.

2. **Handle layer (`UiTargetApp`)** -- returned by `Attach`/`Open`; represents attached/opened application window. Methods accept targets by `string` name (Object Repository target name), `IElementDescriptor`, `TargetAnchorableModel`, or `RuntimeTarget`. Implements `IDisposable` — dispose when done. Indexers convert target name or element descriptor into `TargetAnchorableModel`.

Every action method has synchronous plus `Async`-suffixed variant returning `Task` or `Task<T>`.

### External descriptors

`IElementDescriptor` and `IScreenDescriptor` belong to Coded Workflows framework (`UiPath.CodedWorkflows.DescriptorIntegration` package), not this API. Many methods accept them so Coded Workflows tooling descriptors pass directly. Schema owned by that package; treat as opaque inputs.

### Runtime target models

`TargetAnchorableModel` — runtime-model equivalent of [`TargetAnchorable`](../activities/common/Target.md#targetanchorable): same selector, anchor, image, native text, semantic, and CV data as activity-side `TargetAnchorable`, but plain runtime object, not XAML sub-object with `InArgument<T>` properties. Most coded-API methods accept it via `UiTargetApp` indexers (`uiTargetApp["target name"]` or `uiTargetApp[descriptor]`). Full property surface: [`Target.md`](../activities/common/Target.md).

`TargetAppModel` — runtime-model equivalent of [`TargetApp`](../activities/common/Target.md#targetapp), used by `Attach(TargetAppModel)` / `Open(TargetAppModel)` for advanced Object Repository scenarios. `RuntimeTarget` — opaque runtime handle (`Guid` reference) returned by `GetRuntimeTarget`, consumed by other methods.

## Auto-imported Namespaces

None beyond defaults.

## Enum Types

Most enums referenced by signatures and option classes below are documented under [`../activities/common/`](../activities/common/) — each enum's md file lists values with descriptions. Examples: [`NClickType`](../activities/common/NClickType.md), [`NMouseButton`](../activities/common/NMouseButton.md), [`NWaitForReady`](../activities/common/NWaitForReady.md).

Enums used only by this coded API (not by any activity) documented inline at end of file: [`NScrapingOptions`](#nscrapingoptions).

Runtime element handle [`UiElement`](../activities/common/UiElement.md) and verification options shape [`VerifyExecutionOptions`](../activities/common/VerifyExecutionOptions.md) documented under `../activities/common/` — also referenced by activities.

## IUiAutomationAppService Methods

### Attach

```csharp
UiTargetApp Attach(string screenName, string appName = "***", string appVersion = "***", NAppAttachMode appAttachMode = NAppAttachMode.ByInstance, NWindowResize windowResize = NWindowResize.None, NInteractionMode interactionMode = NInteractionMode.HardwareEvents);
Task<UiTargetApp> AttachAsync(string screenName, string appName = "***", string appVersion = "***", NAppAttachMode appAttachMode = NAppAttachMode.ByInstance, NWindowResize windowResize = NWindowResize.None, NInteractionMode interactionMode = NInteractionMode.HardwareEvents);
UiTargetApp Attach(string screenName, TargetAppOptions targetAppOptions, string appName = "***", string appVersion = "***");
Task<UiTargetApp> AttachAsync(string screenName, TargetAppOptions targetAppOptions, string appName = "***", string appVersion = "***");
UiTargetApp Attach(IScreenDescriptor screenDescriptor, NAppAttachMode appAttachMode = NAppAttachMode.ByInstance, NWindowResize windowResize = NWindowResize.None, NInteractionMode interactionMode = NInteractionMode.HardwareEvents);
Task<UiTargetApp> AttachAsync(IScreenDescriptor screenDescriptor, NAppAttachMode appAttachMode = NAppAttachMode.ByInstance, NWindowResize windowResize = NWindowResize.None, NInteractionMode interactionMode = NInteractionMode.HardwareEvents);
UiTargetApp Attach(IScreenDescriptor screenDescriptor, TargetAppOptions targetAppOptions);
Task<UiTargetApp> AttachAsync(IScreenDescriptor screenDescriptor, TargetAppOptions targetAppOptions);
UiTargetApp Attach(TargetAppModel targetApp, TargetAppOptions targetAppOptions = null);
Task<UiTargetApp> AttachAsync(TargetAppModel targetApp, TargetAppOptions targetAppOptions = null);
```

### Open

```csharp
UiTargetApp Open(string screenName, string appName = "***", string appVersion = "***", NAppOpenMode openMode = NAppOpenMode.IfNotOpen, NWindowResize windowResize = NWindowResize.None, NInteractionMode interactionMode = NInteractionMode.HardwareEvents);
Task<UiTargetApp> OpenAsync(string screenName, string appName = "***", string appVersion = "***", NAppOpenMode openMode = NAppOpenMode.IfNotOpen, NWindowResize windowResize = NWindowResize.None, NInteractionMode interactionMode = NInteractionMode.HardwareEvents);
UiTargetApp Open(string screenName, TargetAppOptions targetAppOptions, string appName = "***", string appVersion = "***");
Task<UiTargetApp> OpenAsync(string screenName, TargetAppOptions targetAppOptions, string appName = "***", string appVersion = "***");
UiTargetApp Open(IScreenDescriptor screenDescriptor, NAppOpenMode openMode = NAppOpenMode.IfNotOpen, NWindowResize windowResize = NWindowResize.None, NInteractionMode interactionMode = NInteractionMode.HardwareEvents);
Task<UiTargetApp> OpenAsync(IScreenDescriptor screenDescriptor, NAppOpenMode openMode = NAppOpenMode.IfNotOpen, NWindowResize windowResize = NWindowResize.None, NInteractionMode interactionMode = NInteractionMode.HardwareEvents);
UiTargetApp Open(IScreenDescriptor screenDescriptor, TargetAppOptions targetAppOptions);
Task<UiTargetApp> OpenAsync(IScreenDescriptor screenDescriptor, TargetAppOptions targetAppOptions);
UiTargetApp Open(TargetAppModel targetApp, TargetAppOptions targetAppOptions = null);
Task<UiTargetApp> OpenAsync(TargetAppModel targetApp, TargetAppOptions targetAppOptions = null);
```

### KeyboardShortcut

```csharp
void KeyboardShortcut(KeyboardShortcutOptions keyboardShortcutOptions);
Task KeyboardShortcutAsync(KeyboardShortcutOptions keyboardShortcutOptions);
```

### TakeScreenshot

```csharp
void TakeScreenshot(TakeScreenshotOptions takeScreenshotOptions);
Task TakeScreenshotAsync(TakeScreenshotOptions takeScreenshotOptions);
```

### Click

```csharp
void Click(UiElement uiElement, ClickOptions clickOptions = null);
Task ClickAsync(UiElement uiElement, ClickOptions clickOptions = null);
```

### Highlight

```csharp
void Highlight(UiElement uiElement, HighlightOptions highlightOptions = null);
Task HighlightAsync(UiElement uiElement, HighlightOptions highlightOptions = null);
```

### TypeInto

```csharp
void TypeInto(UiElement uiElement, TypeIntoOptions typeIntoOptions);
Task TypeIntoAsync(UiElement uiElement, TypeIntoOptions typeIntoOptions);
```

### Hover

```csharp
void Hover(UiElement uiElement, HoverOptions hoverOptions = null);
Task HoverAsync(UiElement uiElement, HoverOptions hoverOptions = null);
```

### GetText

```csharp
string GetText(UiElement uiElement, GetTextOptions options = null);
Task<string> GetTextAsync(UiElement uiElement, GetTextOptions options = null);
```

### SelectItem

```csharp
void SelectItem(UiElement uiElement, SelectItemOptions options);
Task SelectItemAsync(UiElement uiElement, SelectItemOptions options);
```

### GetUiElement

```csharp
UiElement GetUiElement(RuntimeTarget target);
Task<UiElement> GetUiElementAsync(RuntimeTarget target);
```

### SetDefaultRuntimeBrowser

```csharp
void SetDefaultRuntimeBrowser();
Task SetDefaultRuntimeBrowserAsync();
```

### SetRuntimeBrowser

```csharp
void SetRuntimeBrowser(NBrowserType browserType);
Task SetRuntimeBrowserAsync(NBrowserType browserType);
void SetRuntimeBrowser(SetRuntimeBrowserOptions setRuntimeBrowserOptions);
Task SetRuntimeBrowserAsync(SetRuntimeBrowserOptions setRuntimeBrowserOptions);
```

### SetProjectSetting

Overrides a UI Automation project setting for the lifetime of the current process (or until overridden again). Only UI Automation activities (Classic, Modern and CV) are affected. Keys are fully qualified, e.g. `UiPath.UIAutomationNext.Activities.Generic.DelayBefore`. A `null` value clears a previous override and restores the project setting's base value. XAML equivalent: [Set Project Setting](../activities/SetProjectSetting.md).

```csharp
void SetProjectSetting(string key, object value);
Task SetProjectSettingAsync(string key, object value);
```

### SetCVServer

Replaces the Computer Vision server configuration at runtime, taking precedence over the project settings, for the lifetime of the current process (or until overridden again). The three values are applied as a set — an empty `server` means no remote server and an empty `apiKey` means no API key. XAML equivalent: [Set CV Server](../activities/SetCVServer.md).

```csharp
void SetCVServer(string server, string apiKey, bool useLocalServer);
Task SetCVServerAsync(string server, string apiKey, bool useLocalServer);
```

### GetClipboard

```csharp
string GetClipboard(GetClipboardOptions options);
Task<string> GetClipboardAsync(GetClipboardOptions options);
```

### SetClipboard

```csharp
void SetClipboard(SetClipboardOptions setClipboardOptions);
Task SetClipboardAsync(SetClipboardOptions setClipboardOptions);
```

### WaitState

```csharp
bool WaitState(IElementDescriptor elementDescriptor, CheckStateOptions checkStateOptions);
Task<bool> WaitStateAsync(IElementDescriptor elementDescriptor, CheckStateOptions checkStateOptions);
```

## UiTargetApp Methods

`UiTargetApp` implements `IDisposable` — use `using` blocks or call `Dispose()` when finished.

### Indexers

```csharp
TargetAnchorableModel this[string target] { get; }
TargetAnchorableModel this[IElementDescriptor elementDescriptor] { get; }
```

### Click

```csharp
void Click(string target, NClickType clickType = NClickType.Single, NMouseButton mouseButton = NMouseButton.Left);
Task ClickAsync(string target, NClickType clickType = NClickType.Single, NMouseButton mouseButton = NMouseButton.Left);
void Click(string target, ClickOptions clickOptions);
Task ClickAsync(string target, ClickOptions clickOptions);
void Click(IElementDescriptor elementDescriptor, NClickType clickType = NClickType.Single, NMouseButton mouseButton = NMouseButton.Left);
Task ClickAsync(IElementDescriptor elementDescriptor, NClickType clickType = NClickType.Single, NMouseButton mouseButton = NMouseButton.Left);
void Click(IElementDescriptor elementDescriptor, ClickOptions clickOptions);
Task ClickAsync(IElementDescriptor elementDescriptor, ClickOptions clickOptions);
void Click(TargetAnchorableModel target, ClickOptions clickOptions = null);
Task ClickAsync(TargetAnchorableModel target, ClickOptions clickOptions = null);
void Click(RuntimeTarget target, ClickOptions clickOptions = null);
Task ClickAsync(RuntimeTarget target, ClickOptions clickOptions = null);
```

### MouseScroll

```csharp
void MouseScroll(string target, NScrollDirection direction = NScrollDirection.Down, int movementUnits = Constants.MouseScrollDefaultDistanceMovementUnits);
Task MouseScrollAsync(string target, NScrollDirection direction = NScrollDirection.Down, int movementUnits = Constants.MouseScrollDefaultDistanceMovementUnits);
void MouseScroll(string target, string searchedTarget, NScrollDirection direction = NScrollDirection.Down, int movementUnits = Constants.MouseScrollDefaultToElementMovementUnits);
Task MouseScrollAsync(string target, string searchedTarget, NScrollDirection direction = NScrollDirection.Down, int movementUnits = Constants.MouseScrollDefaultToElementMovementUnits);
void MouseScroll(string target, MouseScrollOptions mouseScrollOptions);
Task MouseScrollAsync(string target, MouseScrollOptions mouseScrollOptions);
void MouseScroll(IElementDescriptor elementDescriptor, NScrollDirection direction = NScrollDirection.Down, int movementUnits = Constants.MouseScrollDefaultDistanceMovementUnits);
Task MouseScrollAsync(IElementDescriptor elementDescriptor, NScrollDirection direction = NScrollDirection.Down, int movementUnits = Constants.MouseScrollDefaultDistanceMovementUnits);
void MouseScroll(IElementDescriptor elementDescriptor, IElementDescriptor searchedElementDescriptor, NScrollDirection direction = NScrollDirection.Down, int movementUnits = Constants.MouseScrollDefaultToElementMovementUnits);
Task MouseScrollAsync(IElementDescriptor elementDescriptor, IElementDescriptor searchedElementDescriptor, NScrollDirection direction = NScrollDirection.Down, int movementUnits = Constants.MouseScrollDefaultToElementMovementUnits);
void MouseScroll(IElementDescriptor elementDescriptor, MouseScrollOptions mouseScrollOptions);
Task MouseScrollAsync(IElementDescriptor elementDescriptor, MouseScrollOptions mouseScrollOptions);
void MouseScroll(TargetAnchorableModel target);
Task MouseScrollAsync(TargetAnchorableModel target);
void MouseScroll(TargetAnchorableModel target, string searchedTarget);
Task MouseScrollAsync(TargetAnchorableModel target, string searchedTarget);
void MouseScroll(TargetAnchorableModel target, MouseScrollOptions mouseScrollOptions);
Task MouseScrollAsync(TargetAnchorableModel target, MouseScrollOptions mouseScrollOptions);
void MouseScroll(RuntimeTarget target, MouseScrollOptions mouseScrollOptions);
Task MouseScrollAsync(RuntimeTarget target, MouseScrollOptions mouseScrollOptions);
```

### GetText

```csharp
string GetText(string target);
Task<string> GetTextAsync(string target);
string GetText(string target, GetTextOptions getTextOptions);
Task<string> GetTextAsync(string target, GetTextOptions getTextOptions);
string GetText(IElementDescriptor elementDescriptor);
Task<string> GetTextAsync(IElementDescriptor elementDescriptor);
string GetText(IElementDescriptor elementDescriptor, GetTextOptions getTextOptions);
Task<string> GetTextAsync(IElementDescriptor elementDescriptor, GetTextOptions getTextOptions);
string GetText(TargetAnchorableModel target, GetTextOptions getTextOptions = null);
Task<string> GetTextAsync(TargetAnchorableModel target, GetTextOptions getTextOptions = null);
string GetText(RuntimeTarget target, GetTextOptions getTextOptions = null);
Task<string> GetTextAsync(RuntimeTarget target, GetTextOptions getTextOptions = null);
```

### TypeInto

```csharp
void TypeInto(string target, string text);
Task TypeIntoAsync(string target, string text);
void TypeInto(string target, SecureString secureText);
Task TypeIntoAsync(string target, SecureString secureText);
void TypeInto(string target, TypeIntoOptions typeIntoOptions);
Task TypeIntoAsync(string target, TypeIntoOptions typeIntoOptions);
void TypeInto(IElementDescriptor elementDescriptor, string text);
Task TypeIntoAsync(IElementDescriptor elementDescriptor, string text);
void TypeInto(IElementDescriptor elementDescriptor, SecureString secureText);
Task TypeIntoAsync(IElementDescriptor elementDescriptor, SecureString secureText);
void TypeInto(IElementDescriptor elementDescriptor, TypeIntoOptions typeIntoOptions);
Task TypeIntoAsync(IElementDescriptor elementDescriptor, TypeIntoOptions typeIntoOptions);
void TypeInto(TargetAnchorableModel target, string text);
Task TypeIntoAsync(TargetAnchorableModel target, string text);
void TypeInto(TargetAnchorableModel target, SecureString secureText);
Task TypeIntoAsync(TargetAnchorableModel target, SecureString secureText);
void TypeInto(TargetAnchorableModel target, TypeIntoOptions typeIntoOptions);
Task TypeIntoAsync(TargetAnchorableModel target, TypeIntoOptions typeIntoOptions);
void TypeInto(RuntimeTarget target, TypeIntoOptions typeIntoOptions);
Task TypeIntoAsync(RuntimeTarget target, TypeIntoOptions typeIntoOptions);
```

### TakeScreenshot

```csharp
void TakeScreenshot(string target, string fileName);
Task TakeScreenshotAsync(string target, string fileName);
void TakeScreenshot(string target, TakeScreenshotOptions takeScreenshotOptions);
Task TakeScreenshotAsync(string target, TakeScreenshotOptions takeScreenshotOptions);
void TakeScreenshot(IElementDescriptor elementDescriptor, string fileName);
Task TakeScreenshotAsync(IElementDescriptor elementDescriptor, string fileName);
void TakeScreenshot(IElementDescriptor elementDescriptor, TakeScreenshotOptions takeScreenshotOptions);
Task TakeScreenshotAsync(IElementDescriptor elementDescriptor, TakeScreenshotOptions takeScreenshotOptions);
void TakeScreenshot(TargetAnchorableModel target, string fileName);
Task TakeScreenshotAsync(TargetAnchorableModel target, string fileName);
void TakeScreenshot(TargetAnchorableModel target, TakeScreenshotOptions takeScreenshotOptions);
Task TakeScreenshotAsync(TargetAnchorableModel target, TakeScreenshotOptions takeScreenshotOptions);
void TakeScreenshot(RuntimeTarget target, TakeScreenshotOptions takeScreenshotOptions);
Task TakeScreenshotAsync(RuntimeTarget target, TakeScreenshotOptions takeScreenshotOptions);
```

### SelectItem

```csharp
void SelectItem(string target, string item);
Task SelectItemAsync(string target, string item);
void SelectItem(string target, SelectItemOptions selectItemOptions);
Task SelectItemAsync(string target, SelectItemOptions selectItemOptions);
void SelectItem(IElementDescriptor elementDescriptor, string item);
Task SelectItemAsync(IElementDescriptor elementDescriptor, string item);
void SelectItem(IElementDescriptor elementDescriptor, SelectItemOptions selectItemOptions);
Task SelectItemAsync(IElementDescriptor elementDescriptor, SelectItemOptions selectItemOptions);
void SelectItem(TargetAnchorableModel target, string item);
Task SelectItemAsync(TargetAnchorableModel target, string item);
void SelectItem(TargetAnchorableModel target, SelectItemOptions selectItemOptions);
Task SelectItemAsync(TargetAnchorableModel target, SelectItemOptions selectItemOptions);
void SelectItem(RuntimeTarget target, SelectItemOptions selectItemOptions);
Task SelectItemAsync(RuntimeTarget target, SelectItemOptions selectItemOptions);
```

### SAPClickToolbarButton

```csharp
void SAPClickToolbarButton(string target, string button);
Task SAPClickToolbarButtonAsync(string target, string button);
void SAPClickToolbarButton(string target, SelectItemOptions selectItemOptions);
Task SAPClickToolbarButtonAsync(string target, SelectItemOptions selectItemOptions);
void SAPClickToolbarButton(IElementDescriptor elementDescriptor, string button);
Task SAPClickToolbarButtonAsync(IElementDescriptor elementDescriptor, string button);
void SAPClickToolbarButton(IElementDescriptor elementDescriptor, SelectItemOptions selectItemOptions);
Task SAPClickToolbarButtonAsync(IElementDescriptor elementDescriptor, SelectItemOptions selectItemOptions);
void SAPClickToolbarButton(TargetAnchorableModel target, string item);
Task SAPClickToolbarButtonAsync(TargetAnchorableModel target, string item);
void SAPClickToolbarButton(TargetAnchorableModel target, SelectItemOptions selectItemOptions);
Task SAPClickToolbarButtonAsync(TargetAnchorableModel target, SelectItemOptions selectItemOptions);
void SAPClickToolbarButton(RuntimeTarget target, SelectItemOptions selectItemOptions);
Task SAPClickToolbarButtonAsync(RuntimeTarget target, SelectItemOptions selectItemOptions);
```

### Check

```csharp
void Check(string target, NCheckType checkType = NCheckType.Check);
Task CheckAsync(string target, NCheckType checkType = NCheckType.Check);
void Check(string target, CheckOptions checkOptions);
Task CheckAsync(string target, CheckOptions checkOptions);
void Check(IElementDescriptor elementDescriptor, NCheckType checkType = NCheckType.Check);
Task CheckAsync(IElementDescriptor elementDescriptor, NCheckType checkType = NCheckType.Check);
void Check(IElementDescriptor elementDescriptor, CheckOptions checkOptions);
Task CheckAsync(IElementDescriptor elementDescriptor, CheckOptions checkOptions);
void Check(TargetAnchorableModel target, CheckOptions checkOptions = null);
Task CheckAsync(TargetAnchorableModel target, CheckOptions checkOptions = null);
void Check(RuntimeTarget target, CheckOptions checkOptions = null);
Task CheckAsync(RuntimeTarget target, CheckOptions checkOptions = null);
```

### IsEnabled

```csharp
bool IsEnabled(string target);
Task<bool> IsEnabledAsync(string target);
bool IsEnabled(string target, IsEnabledOptions isEnabledOptions);
Task<bool> IsEnabledAsync(string target, IsEnabledOptions isEnabledOptions);
bool IsEnabled(IElementDescriptor elementDescriptor);
Task<bool> IsEnabledAsync(IElementDescriptor elementDescriptor);
bool IsEnabled(IElementDescriptor elementDescriptor, IsEnabledOptions isEnabledOptions);
Task<bool> IsEnabledAsync(IElementDescriptor elementDescriptor, IsEnabledOptions isEnabledOptions);
bool IsEnabled(TargetAnchorableModel target, IsEnabledOptions isEnabledOptions = null);
Task<bool> IsEnabledAsync(TargetAnchorableModel target, IsEnabledOptions isEnabledOptions = null);
bool IsEnabled(RuntimeTarget target, IsEnabledOptions isEnabledOptions = null);
Task<bool> IsEnabledAsync(RuntimeTarget target, IsEnabledOptions isEnabledOptions = null);
```

### WaitState

```csharp
bool WaitState(string target, NCheckStateMode checkStateMode, double timeout);
Task<bool> WaitStateAsync(string target, NCheckStateMode checkStateMode, double timeout);
bool WaitState(string target, CheckStateOptions checkStateOptions);
Task<bool> WaitStateAsync(string target, CheckStateOptions checkStateOptions);
bool WaitState(IElementDescriptor elementDescriptor, NCheckStateMode checkStateMode, double timeout);
Task<bool> WaitStateAsync(IElementDescriptor elementDescriptor, NCheckStateMode checkStateMode, double timeout);
bool WaitState(IElementDescriptor elementDescriptor, CheckStateOptions checkStateOptions);
Task<bool> WaitStateAsync(IElementDescriptor elementDescriptor, CheckStateOptions checkStateOptions);
bool WaitState(TargetAnchorableModel target, NCheckStateMode checkStateMode, double timeout = 5);
Task<bool> WaitStateAsync(TargetAnchorableModel target, NCheckStateMode checkStateMode, double timeout = 5);
bool WaitState(TargetAnchorableModel target, CheckStateOptions checkStateOptions);
Task<bool> WaitStateAsync(TargetAnchorableModel target, CheckStateOptions checkStateOptions);
bool WaitState(RuntimeTarget target, CheckStateOptions checkStateOptions);
Task<bool> WaitStateAsync(RuntimeTarget target, CheckStateOptions checkStateOptions);
```

### KeyboardShortcut

```csharp
void KeyboardShortcut(string target, string shortcuts, double delayBetweenShortcuts = 0.5);
Task KeyboardShortcutAsync(string target, string shortcuts, double delayBetweenShortcuts = 0.5);
void KeyboardShortcut(string target, KeyboardShortcutOptions keyboardShortcutOptions);
Task KeyboardShortcutAsync(string target, KeyboardShortcutOptions keyboardShortcutOptions);
void KeyboardShortcut(IElementDescriptor elementDescriptor, string shortcuts, double delayBetweenShortcuts = 0.5);
Task KeyboardShortcutAsync(IElementDescriptor elementDescriptor, string shortcuts, double delayBetweenShortcuts = 0.5);
void KeyboardShortcut(IElementDescriptor elementDescriptor, KeyboardShortcutOptions keyboardShortcutOptions);
Task KeyboardShortcutAsync(IElementDescriptor elementDescriptor, KeyboardShortcutOptions keyboardShortcutOptions);
void KeyboardShortcut(string shortcuts, double delayBetweenShortcuts = 0.5);
Task KeyboardShortcutAsync(string shortcuts, double delayBetweenShortcuts = 0.5);
void KeyboardShortcut(KeyboardShortcutOptions keyboardShortcutOptions);
Task KeyboardShortcutAsync(KeyboardShortcutOptions keyboardShortcutOptions);
void KeyboardShortcut(TargetAnchorableModel target, string shortcuts, double delayBetweenShortcuts = 0.5);
Task KeyboardShortcutAsync(TargetAnchorableModel target, string shortcuts, double delayBetweenShortcuts = 0.5);
void KeyboardShortcut(TargetAnchorableModel target, KeyboardShortcutOptions keyboardShortcutOptions);
Task KeyboardShortcutAsync(TargetAnchorableModel target, KeyboardShortcutOptions keyboardShortcutOptions);
void KeyboardShortcut(RuntimeTarget target, KeyboardShortcutOptions keyboardShortcutOptions);
Task KeyboardShortcutAsync(RuntimeTarget target, KeyboardShortcutOptions keyboardShortcutOptions);
```

### Highlight

```csharp
void Highlight(string target, KnownColor color = KnownColor.Yellow, double duration = 3);
Task HighlightAsync(string target, KnownColor color = KnownColor.Yellow, double duration = 3);
void Highlight(string target, HighlightOptions highlightOptions);
Task HighlightAsync(string target, HighlightOptions highlightOptions);
void Highlight(IElementDescriptor elementDescriptor, KnownColor color = KnownColor.Yellow, double duration = 3);
Task HighlightAsync(IElementDescriptor elementDescriptor, KnownColor color = KnownColor.Yellow, double duration = 3);
void Highlight(IElementDescriptor elementDescriptor, HighlightOptions highlightOptions);
Task HighlightAsync(IElementDescriptor elementDescriptor, HighlightOptions highlightOptions);
void Highlight(TargetAnchorableModel target, HighlightOptions highlightOptions = null);
Task HighlightAsync(TargetAnchorableModel target, HighlightOptions highlightOptions = null);
void Highlight(RuntimeTarget target, HighlightOptions highlightOptions = null);
Task HighlightAsync(RuntimeTarget target, HighlightOptions highlightOptions = null);
```

### GetAttribute

```csharp
string GetAttribute(string target, string attribute);
Task<string> GetAttributeAsync(string target, string attribute);
string GetAttribute(string target, GetAttributeOptions getAttributeOptions);
Task<string> GetAttributeAsync(string target, GetAttributeOptions getAttributeOptions);
string GetAttribute(IElementDescriptor elementDescriptor, string attribute);
Task<string> GetAttributeAsync(IElementDescriptor elementDescriptor, string attribute);
string GetAttribute(IElementDescriptor elementDescriptor, GetAttributeOptions getAttributeOptions);
Task<string> GetAttributeAsync(IElementDescriptor elementDescriptor, GetAttributeOptions getAttributeOptions);
T GetAttribute<T>(string target, string attribute);
Task<T> GetAttributeAsync<T>(string target, string attribute);
T GetAttribute<T>(string target, GetAttributeOptions getAttributeOptions);
Task<T> GetAttributeAsync<T>(string target, GetAttributeOptions getAttributeOptions);
T GetAttribute<T>(IElementDescriptor elementDescriptor, string attribute);
Task<T> GetAttributeAsync<T>(IElementDescriptor elementDescriptor, string attribute);
T GetAttribute<T>(IElementDescriptor elementDescriptor, GetAttributeOptions getAttributeOptions);
Task<T> GetAttributeAsync<T>(IElementDescriptor elementDescriptor, GetAttributeOptions getAttributeOptions);
string GetAttribute(TargetAnchorableModel target, string attribute);
Task<string> GetAttributeAsync(TargetAnchorableModel target, string attribute);
string GetAttribute(TargetAnchorableModel target, GetAttributeOptions getAttributeOptions);
Task<string> GetAttributeAsync(TargetAnchorableModel target, GetAttributeOptions getAttributeOptions);
string GetAttribute(RuntimeTarget target, GetAttributeOptions getAttributeOptions);
Task<string> GetAttributeAsync(RuntimeTarget target, GetAttributeOptions getAttributeOptions);
T GetAttribute<T>(TargetAnchorableModel target, string attribute);
Task<T> GetAttributeAsync<T>(TargetAnchorableModel target, string attribute);
T GetAttribute<T>(TargetAnchorableModel target, GetAttributeOptions getAttributeOptions);
Task<T> GetAttributeAsync<T>(TargetAnchorableModel target, GetAttributeOptions getAttributeOptions);
T GetAttribute<T>(RuntimeTarget target, GetAttributeOptions getAttributeOptions);
Task<T> GetAttributeAsync<T>(RuntimeTarget target, GetAttributeOptions getAttributeOptions);
```

### GetAttributeAsJson

```csharp
string GetAttributeAsJson(string target, string attribute);
Task<string> GetAttributeAsJsonAsync(string target, string attribute);
string GetAttributeAsJson(string target, GetAttributeOptions getAttributeOptions);
Task<string> GetAttributeAsJsonAsync(string target, GetAttributeOptions getAttributeOptions);
string GetAttributeAsJson(IElementDescriptor elementDescriptor, string attribute);
Task<string> GetAttributeAsJsonAsync(IElementDescriptor elementDescriptor, string attribute);
string GetAttributeAsJson(IElementDescriptor elementDescriptor, GetAttributeOptions getAttributeOptions);
Task<string> GetAttributeAsJsonAsync(IElementDescriptor elementDescriptor, GetAttributeOptions getAttributeOptions);
string GetAttributeAsJson(TargetAnchorableModel target, string attribute);
Task<string> GetAttributeAsJsonAsync(TargetAnchorableModel target, string attribute);
string GetAttributeAsJson(TargetAnchorableModel target, GetAttributeOptions getAttributeOptions);
Task<string> GetAttributeAsJsonAsync(TargetAnchorableModel target, GetAttributeOptions getAttributeOptions);
string GetAttributeAsJson(RuntimeTarget target, GetAttributeOptions getAttributeOptions);
Task<string> GetAttributeAsJsonAsync(RuntimeTarget target, GetAttributeOptions getAttributeOptions);
```

### Hover

```csharp
void Hover(string target, double hoverTime, CursorMotionType cursorMotionType);
Task HoverAsync(string target, double hoverTime, CursorMotionType cursorMotionType);
void Hover(string target, HoverOptions hoverOptions);
Task HoverAsync(string target, HoverOptions hoverOptions);
void Hover(IElementDescriptor elementDescriptor, double? hoverTime = null, CursorMotionType? cursorMotionType = null);
Task HoverAsync(IElementDescriptor elementDescriptor, double? hoverTime = null, CursorMotionType? cursorMotionType = null);
void Hover(IElementDescriptor elementDescriptor, HoverOptions hoverOptions);
Task HoverAsync(IElementDescriptor elementDescriptor, HoverOptions hoverOptions);
void Hover(TargetAnchorableModel target, double hoverTime, CursorMotionType cursorMotionType = CursorMotionType.Instant);
Task HoverAsync(TargetAnchorableModel target, double hoverTime, CursorMotionType cursorMotionType = CursorMotionType.Instant);
void Hover(TargetAnchorableModel target, HoverOptions hoverOptions);
Task HoverAsync(TargetAnchorableModel target, HoverOptions hoverOptions);
void Hover(RuntimeTarget target, HoverOptions hoverOptions);
Task HoverAsync(RuntimeTarget target, HoverOptions hoverOptions);
```

### GetChildren

```csharp
IEnumerable<RuntimeTarget> GetChildren(string target, string selectorFilter, bool checkRootVisibility = false, bool recursive = false, double timeout = Constants.DefaultTimeout);
Task<IEnumerable<RuntimeTarget>> GetChildrenAsync(string target, string selectorFilter, bool checkRootVisibility = false, bool recursive = false, double timeout = Constants.DefaultTimeout);
IEnumerable<RuntimeTarget> GetChildren(string target, GetChildrenOptions getChildrenOptions);
Task<IEnumerable<RuntimeTarget>> GetChildrenAsync(string target, GetChildrenOptions getChildrenOptions);
IEnumerable<RuntimeTarget> GetChildren(IElementDescriptor elementDescriptor, string selectorFilter, bool checkRootVisibility = false, bool recursive = false, double timeout = Constants.DefaultTimeout);
Task<IEnumerable<RuntimeTarget>> GetChildrenAsync(IElementDescriptor elementDescriptor, string selectorFilter, bool checkRootVisibility = false, bool recursive = false, double timeout = Constants.DefaultTimeout);
IEnumerable<RuntimeTarget> GetChildren(IElementDescriptor elementDescriptor, GetChildrenOptions getChildrenOptions);
Task<IEnumerable<RuntimeTarget>> GetChildrenAsync(IElementDescriptor elementDescriptor, GetChildrenOptions getChildrenOptions);
IEnumerable<RuntimeTarget> GetChildren(TargetAnchorableModel target, GetChildrenOptions getChildrenOptions);
Task<IEnumerable<RuntimeTarget>> GetChildrenAsync(TargetAnchorableModel target, GetChildrenOptions getChildrenOptions);
IEnumerable<RuntimeTarget> GetChildren(RuntimeTarget target, GetChildrenOptions getChildrenOptions);
Task<IEnumerable<RuntimeTarget>> GetChildrenAsync(RuntimeTarget target, GetChildrenOptions getChildrenOptions);
```

### ExtractData

```csharp
DataTable ExtractData(string target, string extractMetadata, string tableSettings = "", NChildInteractionMode interactionMode = NChildInteractionMode.SameAsCard, LimitType limitExtractionTo = LimitType.Rows, int? numberOfItems = null);
DataTable ExtractData(string target, string nextPageTarget, string extractMetadata, string tableSettings = "", double delayBetweenPages = Constants.DefaultDelayBetweenPages, NChildInteractionMode interactionMode = NChildInteractionMode.SameAsCard, LimitType limitExtractionTo = LimitType.Rows, int? numberOfItems = null);
DataTable ExtractData(string target, ExtractDataOptions extractDataOptions);
DataTable ExtractData(IElementDescriptor elementDescriptor, string extractMetadata, string tableSettings = "", NChildInteractionMode interactionMode = NChildInteractionMode.SameAsCard, LimitType limitExtractionTo = LimitType.Rows, int? numberOfItems = null);
DataTable ExtractData(IElementDescriptor elementDescriptor, string nextPageTarget, string extractMetadata, string tableSettings = "", double delayBetweenPages = Constants.DefaultDelayBetweenPages, NChildInteractionMode interactionMode = NChildInteractionMode.SameAsCard, LimitType limitExtractionTo = LimitType.Rows, int? numberOfItems = null);
DataTable ExtractData(IElementDescriptor elementDescriptor, ExtractDataOptions extractDataOptions);
DataTable ExtractData(TargetAnchorableModel target, string extractMetadata, string tableSettings = "", NChildInteractionMode interactionMode = NChildInteractionMode.SameAsCard, LimitType limitExtractionTo = LimitType.Rows, int? numberOfItems = null);
DataTable ExtractData(TargetAnchorableModel target, TargetAnchorableModel nextPageTarget, string extractMetadata, string tableSettings = "", double delayBetweenPages = Constants.DefaultDelayBetweenPages, NChildInteractionMode interactionMode = NChildInteractionMode.SameAsCard, LimitType limitExtractionTo = LimitType.Rows, int? numberOfItems = null);
DataTable ExtractData(TargetAnchorableModel target, ExtractDataOptions extractDataOptions);
DataTable ExtractData(ExtractDataOptionsModel options);
```

### ExtractTableData

```csharp
DataTable ExtractTableData(IElementDescriptor elementDescriptor, ExtractTableDataOptions extractTableDataOptions = null);
Task<DataTable> ExtractTableDataAsync(IElementDescriptor elementDescriptor, ExtractTableDataOptions extractTableDataOptions = null);
DataTable ExtractTableData(string target, ExtractTableDataOptions extractTableDataOptions = null);
Task<DataTable> ExtractTableDataAsync(string target, ExtractTableDataOptions extractTableDataOptions = null);
DataTable ExtractTableData(TargetAnchorableModel target, string extractMetadata, string tableSettings, TargetAnchorableModel nextPageTarget = null, ExtractTableDataOptions extractDataOptions = null);
Task<DataTable> ExtractTableDataAsync(TargetAnchorableModel target, string extractMetadata, string tableSettings, TargetAnchorableModel nextPageTarget = null, ExtractTableDataOptions extractDataOptions = null);
```

### DragAndDrop

```csharp
void DragAndDrop(string target, TargetAnchorableModel destinationTarget, NKeyModifiers? keyModifiers = null, NMouseButton? mouseButton = null, CursorMotionType? cursorMotionType = null, bool? useSourceHover = null, double? delayBetweenActions = null);
Task DragAndDropAsync(string target, TargetAnchorableModel destinationTarget, NKeyModifiers? keyModifiers = null, NMouseButton? mouseButton = null, CursorMotionType? cursorMotionType = null, bool? useSourceHover = null, double? delayBetweenActions = null);
void DragAndDrop(string target, DragAndDropOptions dragAndDropOptions);
Task DragAndDropAsync(string target, DragAndDropOptions dragAndDropOptions);
void DragAndDrop(IElementDescriptor elementDescriptor, TargetAnchorableModel destinationTarget, NKeyModifiers? keyModifiers = null, NMouseButton? mouseButton = null, CursorMotionType? cursorMotionType = null, bool? useSourceHover = null, double? delayBetweenActions = null);
Task DragAndDropAsync(IElementDescriptor elementDescriptor, TargetAnchorableModel destinationTarget, NKeyModifiers? keyModifiers = null, NMouseButton? mouseButton = null, CursorMotionType? cursorMotionType = null, bool? useSourceHover = null, double? delayBetweenActions = null);
void DragAndDrop(IElementDescriptor elementDescriptor, IElementDescriptor destinationTarget, NKeyModifiers? keyModifiers = null, NMouseButton? mouseButton = null, CursorMotionType? cursorMotionType = null, bool? useSourceHover = null, double? delayBetweenActions = null);
Task DragAndDropAsync(IElementDescriptor elementDescriptor, IElementDescriptor destinationTarget, NKeyModifiers? keyModifiers = null, NMouseButton? mouseButton = null, CursorMotionType? cursorMotionType = null, bool? useSourceHover = null, double? delayBetweenActions = null);
void DragAndDrop(IElementDescriptor elementDescriptor, DragAndDropOptions dragAndDropOptions);
Task DragAndDropAsync(IElementDescriptor elementDescriptor, DragAndDropOptions dragAndDropOptions);
void DragAndDrop(TargetAnchorableModel target, DragAndDropOptions dragAndDropOptions = null);
Task DragAndDropAsync(TargetAnchorableModel target, DragAndDropOptions dragAndDropOptions = null);
void DragAndDrop(RuntimeTarget target, DragAndDropOptions dragAndDropOptions = null);
Task DragAndDropAsync(RuntimeTarget target, DragAndDropOptions dragAndDropOptions = null);
```

### GetRuntimeTarget

```csharp
RuntimeTarget GetRuntimeTarget(string target, GetRuntimeTargetOptions getRuntimeTargetOptions = null);
Task<RuntimeTarget> GetRuntimeTargetAsync(string target, GetRuntimeTargetOptions getRuntimeTargetOptions = null);
RuntimeTarget GetRuntimeTarget(TargetAnchorableModel target, GetRuntimeTargetOptions getRuntimeTargetOptions = null);
Task<RuntimeTarget> GetRuntimeTargetAsync(TargetAnchorableModel target, GetRuntimeTargetOptions getRuntimeTargetOptions = null);
```

### FillForm

```csharp
void FillForm(FillFormOptions fillFormOptions);
Task FillFormAsync(FillFormOptions fillFormOptions);
void FillForm(object dataSource);
Task FillFormAsync(object dataSource);
```

### ClosePopup

```csharp
void ClosePopup(ClosePopupOptions closePopupOptions = null);
Task ClosePopupAsync(ClosePopupOptions closePopupOptions = null);
```

### SAPSelectMenuItem

```csharp
void SAPSelectMenuItem(string item);
Task SAPSelectMenuItemAsync(string item);
void SAPSelectMenuItem(SelectItemOptions selectItemOptions);
Task SAPSelectMenuItemAsync(SelectItemOptions selectItemOptions);
```

### GoToUrl

```csharp
void GoToUrl(string url);
Task GoToUrlAsync(string url);
void GoToUrl(GoToUrlOptions urlOptions);
Task GoToUrlAsync(GoToUrlOptions urlOptions);
```

### GetUrl

```csharp
string GetUrl(GetUrlOptions getUrlOptions = null);
Task<string> GetUrlAsync(GetUrlOptions getUrlOptions = null);
```

### GetAccessibilityCheck

```csharp
string GetAccessibilityCheck(GetAccessibilityCheckOptions getAccessibilityCheckOptions = null);
Task<string> GetAccessibilityCheckAsync(GetAccessibilityCheckOptions getAccessibilityCheckOptions = null);
```

### GetUiElement

```csharp
UiElement GetUiElement(RuntimeTarget target);
Task<UiElement> GetUiElementAsync(RuntimeTarget target);
UiElement GetUiElement();
Task<UiElement> GetUiElementAsync();
UiElement GetUiElement(IElementDescriptor elementDescriptor, GetRuntimeTargetOptions getRuntimeTargetOptions = null);
Task<UiElement> GetUiElementAsync(IElementDescriptor elementDescriptor, GetRuntimeTargetOptions getRuntimeTargetOptions = null);
UiElement GetUiElement(TargetAnchorableModel target, GetRuntimeTargetOptions getRuntimeTargetOptions = null);
Task<UiElement> GetUiElementAsync(TargetAnchorableModel target, GetRuntimeTargetOptions getRuntimeTargetOptions = null);
```

### SetValue

```csharp
void SetValue(string target, string value);
Task SetValueAsync(string target, string value);
void SetValue(string target, SetValueOptions setValueOptions);
Task SetValueAsync(string target, SetValueOptions setValueOptions);
void SetValue(IElementDescriptor elementDescriptor, string value);
Task SetValueAsync(IElementDescriptor elementDescriptor, string value);
void SetValue(IElementDescriptor elementDescriptor, SetValueOptions setValueOptions);
Task SetValueAsync(IElementDescriptor elementDescriptor, SetValueOptions setValueOptions);
void SetValue(TargetAnchorableModel target, string value);
Task SetValueAsync(TargetAnchorableModel target, string value);
void SetValue(TargetAnchorableModel target, SetValueOptions setValueOptions);
Task SetValueAsync(TargetAnchorableModel target, SetValueOptions setValueOptions);
void SetValue(RuntimeTarget target, SetValueOptions setValueOptions);
Task SetValueAsync(RuntimeTarget target, SetValueOptions setValueOptions);
```

### UpdateUIElement

```csharp
void UpdateUIElement(string target, string value);
Task UpdateUIElementAsync(string target, string value);
void UpdateUIElement(string target, UpdateUIElementOptions updateUIElementOptions);
Task UpdateUIElementAsync(string target, UpdateUIElementOptions updateUIElementOptions);
void UpdateUIElement(IElementDescriptor elementDescriptor, string value);
Task UpdateUIElementAsync(IElementDescriptor elementDescriptor, string value);
void UpdateUIElement(IElementDescriptor elementDescriptor, UpdateUIElementOptions updateUIElementOptions);
Task UpdateUIElementAsync(IElementDescriptor elementDescriptor, UpdateUIElementOptions updateUIElementOptions);
void UpdateUIElement(TargetAnchorableModel target, string value);
Task UpdateUIElementAsync(TargetAnchorableModel target, string value);
void UpdateUIElement(TargetAnchorableModel target, UpdateUIElementOptions updateUIElementOptions);
Task UpdateUIElementAsync(TargetAnchorableModel target, UpdateUIElementOptions updateUIElementOptions);
void UpdateUIElement(RuntimeTarget target, UpdateUIElementOptions updateUIElementOptions);
Task UpdateUIElementAsync(RuntimeTarget target, UpdateUIElementOptions updateUIElementOptions);
```

### UITask

```csharp
UITaskResult UITask(string task, NUITaskAgentType agentType = NUITaskAgentType.DOMBased, NChildInteractionMode interactionMode = NChildInteractionMode.SameAsCard);
Task<UITaskResult> UITaskAsync(string task, NUITaskAgentType agentType = NUITaskAgentType.DOMBased, NChildInteractionMode interactionMode = NChildInteractionMode.SameAsCard);
UITaskResult UITask(UITaskOptions options);
Task<UITaskResult> UITaskAsync(UITaskOptions options);
```

### InjectJsScript

```csharp
string InjectJsScript(string target, string scriptCodePath, string inputParameter = null);
Task<string> InjectJsScriptAsync(string target, string scriptCodePath, string inputParameter = null);
string InjectJsScript(string target, InjectJsScriptOptions injectJsScriptOptions);
Task<string> InjectJsScriptAsync(string target, InjectJsScriptOptions injectJsScriptOptions);
string InjectJsScript(IElementDescriptor elementDescriptor, string scriptCodePath, string inputParameter = null);
Task<string> InjectJsScriptAsync(IElementDescriptor elementDescriptor, string scriptCodePath, string inputParameter = null);
string InjectJsScript(IElementDescriptor elementDescriptor, InjectJsScriptOptions injectJsScriptOptions);
Task<string> InjectJsScriptAsync(IElementDescriptor elementDescriptor, InjectJsScriptOptions injectJsScriptOptions);
string InjectJsScript(TargetAnchorableModel target, string scriptCodePath, string inputParameter = null);
Task<string> InjectJsScriptAsync(TargetAnchorableModel target, string scriptCodePath, string inputParameter = null);
string InjectJsScript(TargetAnchorableModel target, InjectJsScriptOptions injectJsScriptOptions);
Task<string> InjectJsScriptAsync(TargetAnchorableModel target, InjectJsScriptOptions injectJsScriptOptions);
string InjectJsScript(RuntimeTarget target, InjectJsScriptOptions injectJsScriptOptions);
Task<string> InjectJsScriptAsync(RuntimeTarget target, InjectJsScriptOptions injectJsScriptOptions);
```

### SAPReadStatusbar

```csharp
SAPReadStatusbarResult SAPReadStatusbar();
Task<SAPReadStatusbarResult> SAPReadStatusbarAsync();
SAPReadStatusbarResult SAPReadStatusbar(SAPReadStatusbarOptions readStatusbarOptions);
Task<SAPReadStatusbarResult> SAPReadStatusbarAsync(SAPReadStatusbarOptions readStatusbarOptions);
```

### SAPSelectDatesInCalendar

```csharp
void SAPSelectDatesInCalendar(string target, DateTime date);
Task SAPSelectDatesInCalendarAsync(string target, DateTime date);
void SAPSelectDatesInCalendar(string target, DateTime startDate, DateTime endDate);
Task SAPSelectDatesInCalendarAsync(string target, DateTime startDate, DateTime endDate);
void SAPSelectDatesInCalendar(string target, int year, int week);
Task SAPSelectDatesInCalendarAsync(string target, int year, int week);
void SAPSelectDatesInCalendar(string target, SAPSelectDatesInCalendarOptions sapSelectDatesInCalendarOptions);
Task SAPSelectDatesInCalendarAsync(string target, SAPSelectDatesInCalendarOptions sapSelectDatesInCalendarOptions);
void SAPSelectDatesInCalendar(IElementDescriptor elementDescriptor, DateTime date);
Task SAPSelectDatesInCalendarAsync(IElementDescriptor elementDescriptor, DateTime date);
void SAPSelectDatesInCalendar(IElementDescriptor elementDescriptor, DateTime startDate, DateTime endDate);
Task SAPSelectDatesInCalendarAsync(IElementDescriptor elementDescriptor, DateTime startDate, DateTime endDate);
void SAPSelectDatesInCalendar(IElementDescriptor elementDescriptor, int year, int week);
Task SAPSelectDatesInCalendarAsync(IElementDescriptor elementDescriptor, int year, int week);
void SAPSelectDatesInCalendar(IElementDescriptor elementDescriptor, SAPSelectDatesInCalendarOptions sapSelectDatesInCalendarOptions);
Task SAPSelectDatesInCalendarAsync(IElementDescriptor elementDescriptor, SAPSelectDatesInCalendarOptions sapSelectDatesInCalendarOptions);
void SAPSelectDatesInCalendar(TargetAnchorableModel target, SAPSelectDatesInCalendarOptions sapSelectDatesInCalendarOptions);
Task SAPSelectDatesInCalendarAsync(TargetAnchorableModel target, SAPSelectDatesInCalendarOptions sapSelectDatesInCalendarOptions);
void SAPSelectDatesInCalendar(RuntimeTarget target, SAPSelectDatesInCalendarOptions sapSelectDatesInCalendarOptions);
Task SAPSelectDatesInCalendarAsync(RuntimeTarget target, SAPSelectDatesInCalendarOptions sapSelectDatesInCalendarOptions);
```

### SAPCallTransaction

```csharp
void SAPCallTransaction(string transactionCode);
Task SAPCallTransactionAsync(string transactionCode);
void SAPCallTransaction(string transactionCode, string prefix);
Task SAPCallTransactionAsync(string transactionCode, string prefix);
void SAPCallTransaction(SAPCallTransactionOptions sapCallTransactionOptions);
Task SAPCallTransactionAsync(SAPCallTransactionOptions sapCallTransactionOptions);
```

### SAPExpandTree

```csharp
void SAPExpandTree(string target, string path);
void SAPExpandTree(string target, SAPExpandTreeOptions sapExpandTreeOptions);
void SAPExpandTree(IElementDescriptor elementDescriptor, string path);
void SAPExpandTree(IElementDescriptor elementDescriptor, SAPExpandTreeOptions sapExpandTreeOptions);
void SAPExpandTree(TargetAnchorableModel target, SAPExpandTreeOptions sapExpandTreeOptions);
void SAPExpandTree(RuntimeTarget target, SAPExpandTreeOptions sapExpandTreeOptions);
Task SAPExpandTreeAsync(string target, string path);
Task SAPExpandTreeAsync(string target, SAPExpandTreeOptions sapExpandTreeOptions);
Task SAPExpandTreeAsync(IElementDescriptor elementDescriptor, string path);
Task SAPExpandTreeAsync(IElementDescriptor elementDescriptor, SAPExpandTreeOptions sapExpandTreeOptions);
Task SAPExpandTreeAsync(TargetAnchorableModel target, SAPExpandTreeOptions sapExpandTreeOptions);
Task SAPExpandTreeAsync(RuntimeTarget target, SAPExpandTreeOptions sapExpandTreeOptions);
```

### SAPExpandALVTree

```csharp
void SAPExpandALVTree(string target, string path);
void SAPExpandALVTree(string target, SAPExpandALVTreeOptions sapExpandALVTreeOptions);
void SAPExpandALVTree(IElementDescriptor elementDescriptor, string path);
void SAPExpandALVTree(IElementDescriptor elementDescriptor, SAPExpandALVTreeOptions sapExpandALVTreeOptions);
void SAPExpandALVTree(TargetAnchorableModel target, SAPExpandALVTreeOptions sapExpandALVTreeOptions);
void SAPExpandALVTree(RuntimeTarget target, SAPExpandALVTreeOptions sapExpandALVTreeOptions);
Task SAPExpandALVTreeAsync(string target, string path);
Task SAPExpandALVTreeAsync(string target, SAPExpandALVTreeOptions sapExpandALVTreeOptions);
Task SAPExpandALVTreeAsync(IElementDescriptor elementDescriptor, string path);
Task SAPExpandALVTreeAsync(IElementDescriptor elementDescriptor, SAPExpandALVTreeOptions sapExpandALVTreeOptions);
Task SAPExpandALVTreeAsync(TargetAnchorableModel target, SAPExpandALVTreeOptions sapExpandALVTreeOptions);
Task SAPExpandALVTreeAsync(RuntimeTarget target, SAPExpandALVTreeOptions sapExpandALVTreeOptions);
```

### Close

```csharp
void Close();
```

### Dispose

```csharp
void Dispose();
```

## Options Classes

### ClickOptions

| Property | Type | Description |
|----------|------|-------------|
| `KeyModifiers` | [`NKeyModifiers`](../activities/common/NKeyModifiers.md) | Key modifier. Options: None, Alt, Ctrl, Shift, Win. Default None. |
| `ClickType` | [`NClickType`](../activities/common/NClickType.md) | Mouse click type (single, double, up, down). Default single. |
| `MouseButton` | [`NMouseButton`](../activities/common/NMouseButton.md) | Mouse button (left, right, middle). Default left. |
| `CursorMotionType` | [`CursorMotionType`](../activities/common/CursorMotionType.md) | Cursor motion type. |
| `AlterIfDisabled` | `bool` | Executes even if UI element disabled. |
| `InteractionMode` | [`NChildInteractionMode`](../activities/common/NChildInteractionMode.md) | Interaction type. |
| `ActivateBefore` | `bool` | Bring UI element to foreground before clicking. Default true. |
| `VerifyOptions` | `VerifyOptions` | Runtime verification. |
| `UnblockInput` | `bool` | Used at runtime when Click triggers modal dialog box. Only works with Simulate Click. |

### TypeIntoOptions

| Property | Type | Description |
|----------|------|-------------|
| `Text` | `string` | Text to enter. |
| `SecureText` | `SecureString` | Secure text to enter. |
| `DelayBetweenKeys` | `double` | Delay (seconds) between consecutive keystrokes. Max 1 second. |
| `ActivateBefore` | `bool` | Bring UI element to foreground before typing. Default true. |
| `ClickBeforeMode` | [`NClickMode`](../activities/common/NClickMode.md) | Click text-field before typing to activate it. |
| `EmptyFieldMode` | [`NEmptyFieldMode`](../activities/common/NEmptyFieldMode.md) | Clear existing content before typing. |
| `ClipboardMode` | [`NTypeByClipboardMode`](../activities/common/NTypeByClipboardMode.md) | Whether clipboard used for typing. |
| `DeselectAfter` | `bool` | Adds Complete event after text entry. Default false. |
| `AlterIfDisabled` | `bool` | Execute even if target element disabled. Default false. |
| `InteractionMode` | [`NChildInteractionMode`](../activities/common/NChildInteractionMode.md) | Execution method. |
| `VerifyOptions` | `VerifyOptions` | Runtime verification. |

### CheckOptions

| Property | Type | Description |
|----------|------|-------------|
| `CheckType` | [`NCheckType`](../activities/common/NCheckType.md) | Action: Check, Uncheck, or Toggle. |
| `AlterIfDisabled` | `bool` | Execute even if target element disabled. Default false. |

### CheckStateOptions

| Property | Type | Description |
|----------|------|-------------|
| `Mode` | [`NCheckStateMode`](../activities/common/NCheckStateMode.md) | Wait for appear or disappear. |
| `CheckVisibility` | `bool` | Check element visibility. |
| `HealingAgentBehavior` | [`NChildHealingAgentBehavior`](../activities/common/NChildHealingAgentBehavior.md) | Healing agent behavior. Default Disabled. |

### HighlightOptions

| Property | Type | Description |
|----------|------|-------------|
| `HighlightTime` | `double` | Highlight time (seconds). Default 2. |
| `Color` | `KnownColor` | Highlight box color. Default Yellow. |

### HoverOptions

| Property | Type | Description |
|----------|------|-------------|
| `HoverTime` | `double` | Seconds to hover. Default 1. |
| `CursorMotionType` | [`CursorMotionType`](../activities/common/CursorMotionType.md) | How to execute hover. |
| `InteractionMode` | [`NChildInteractionMode`](../activities/common/NChildInteractionMode.md) | Interaction type. |
| `VerifyOptions` | `VerifyOptions` | Runtime verification. |

### GetTextOptions

| Property | Type | Description |
|----------|------|-------------|
| `ScrapingOptions` | [`NScrapingOptions`](#nscrapingoptions) | Options: None, AllowFormatting, IgnoreHiddenText, GetWordsCoordinates. |
| `ScrapingMethod` | [`NScrapingMethod`](../activities/common/NScrapingMethod.md) | Method: Default, Text Attribute, Fulltext, Native. |

### SelectItemOptions

| Property | Type | Description |
|----------|------|-------------|
| `Item` | `string` | Item name to select. |
| `AlterIfDisabled` | `bool` | Execute even if target element disabled. Default false. |

### KeyboardShortcutOptions

| Property | Type | Description |
|----------|------|-------------|
| `Shortcuts` | `string` | Keyboard shortcuts to send. |
| `ActivateBefore` | `bool` | Bring UI element to foreground first. Default true. |
| `DelayBetweenShortcuts` | `double` | Delay (seconds) between shortcuts. Default 0.5. |
| `DelayBetweenKeys` | `double` | Delay (seconds) between consecutive keys. |
| `ClickBeforeMode` | [`NClickMode`](../activities/common/NClickMode.md) | Click before sending shortcuts: None, Single, or Double. |
| `InteractionMode` | [`NChildInteractionMode`](../activities/common/NChildInteractionMode.md) | Execution method. |
| `VerifyOptions` | `VerifyOptions` | Runtime verification. |

### MouseScrollOptions

| Property | Type | Description |
|----------|------|-------------|
| `InteractionMode` | [`NChildInteractionMode`](../activities/common/NChildInteractionMode.md) | Interaction type. |
| `CursorMotionType` | [`CursorMotionType`](../activities/common/CursorMotionType.md) | Cursor motion: Instant or Smooth. |
| `KeyModifiers` | [`NKeyModifiers`](../activities/common/NKeyModifiers.md) | Key modifiers during scroll. Default None. |
| `Direction` | [`NScrollDirection`](../activities/common/NScrollDirection.md) | Scroll direction: Left, Up, Right, Down. |
| `MovementUnits` | `int` | Mouse wheel detents. Default 10. |
| `SearchedElement` | `SearchedElementOptions` | UI element to scroll to. |

### TargetAppOptions

| Property | Type | Description |
|----------|------|-------------|
| `Timeout` | `double` | Timeout in seconds. |
| `InteractionMode` | [`NInteractionMode`](../activities/common/NInteractionMode.md) | Interaction mode. |
| `AttachMode` | [`NAppAttachMode`](../activities/common/NAppAttachMode.md) | Attachment mode. Default ByInstance. |
| `OpenMode` | [`NAppOpenMode`](../activities/common/NAppOpenMode.md) | Open mode. |
| `CloseMode` | [`NAppCloseMode`](../activities/common/NAppCloseMode.md) | Whether to close app on scope exit. Default Always. |
| `WindowResize` | [`NWindowResize`](../activities/common/NWindowResize.md) | Window resize mode. |
| `UserDataFolderMode` | [`BrowserUserDataFolderMode`](../activities/common/BrowserUserDataFolderMode.md) | Browser user data folder mode. |
| `UserDataFolderPath` | `string` | Custom user data folder path. |
| `IsIncognito` | `bool` | Open in incognito mode. |
| `WebDriverMode` | [`NWebDriverMode`](../activities/common/NWebDriverMode.md) | WebDriver mode. |
| `WorkingDirectory` | `string` | Application working directory. |
| `DialogHandlingOptions` | `DialogHandlingOptions` | Browser dialog handling options. |

### TakeScreenshotOptions

| Property | Type | Description |
|----------|------|-------------|
| `FileName` | `string` | Screenshot save path. |

### GetAttributeOptions

Inherits from `GetAttributeBaseOptions`.

### GetAttributeBaseOptions

| Property | Type | Description |
|----------|------|-------------|
| `Attribute` | `string` | Attribute name to retrieve. |

### GetChildrenOptions

| Property | Type | Description |
|----------|------|-------------|
| `SelectorFilter` | `string` | Selector filter. |
| `Timeout` | `double` | Timeout in seconds. Default 30. |
| `Recursive` | `bool` | Recursive search. |
| `CheckRootVisibility` | `bool` | Check visibility while searching for root node. |

### ExtractDataOptions

| Property | Type | Description |
|----------|------|-------------|
| `ExtractMetadata` | `string` | XML string defining data to extract. |
| `TableSettings` | `string` | Table settings for extraction. |
| `InteractionMode` | [`NChildInteractionMode`](../activities/common/NChildInteractionMode.md) | Interaction mode for next-page clicks. |
| `LimitExtractionTo` | [`LimitType`](../activities/common/LimitType.md) | Limit type. |
| `NumberOfItems` | `int?` | Max results to extract. 0 = all. |
| `NextPage` | `NextPageOptions` | Next page navigation options. |

### ExtractTableDataOptions

| Property | Type | Description |
|----------|------|-------------|
| `InteractionMode` | [`NChildInteractionMode`](../activities/common/NChildInteractionMode.md) | Interaction mode. |
| `LimitExtractionTo` | [`LimitType`](../activities/common/LimitType.md) | Limit type. |
| `NumberOfItems` | `int?` | Max items. |
| `DelayBetweenPages` | `double` | Delay between page navigations. |

### DragAndDropOptions

| Property | Type | Description |
|----------|------|-------------|
| `DestinationTarget` | `TargetAnchorableModel` | Drop target. |
| `KeyModifiers` | [`NKeyModifiers`](../activities/common/NKeyModifiers.md) | Key modifiers during drag. |
| `MouseButton` | [`NMouseButton`](../activities/common/NMouseButton.md) | Mouse button. |
| `CursorMotionType` | [`CursorMotionType`](../activities/common/CursorMotionType.md) | Cursor motion type. |
| `UseSourceHover` | `bool` | Hover over source before dragging. |
| `DelayBetweenActions` | `double` | Delay between actions (seconds). |

### FillFormOptions

| Property | Type | Description |
|----------|------|-------------|
| `DataSource` | `object` | Input data. |
| `EnableValidation` | `bool` | Validate execution. |

### SetValueOptions

| Property | Type | Description |
|----------|------|-------------|
| `Value` | `string` | Value to fill target. |
| `EnableValidation` | `bool` | Validate execution. |

### UpdateUIElementOptions

| Property | Type | Description |
|----------|------|-------------|
| `Value` | `string` | Value to fill target. |
| `EnableValidation` | `bool` | Validate execution. |

### ClosePopupOptions

| Property | Type | Description |
|----------|------|-------------|
| `PreferredButtons` | `string[]` | Preferred button search order. Default ["Close", "Cancel"]. |
| `PopupAppearTimeout` | `double` | Popup appear timeout (seconds). |
| `EnableAI` | `bool` | Use AI to close popup. |

### InjectJsScriptOptions

| Property | Type | Description |
|----------|------|-------------|
| `ScriptCodePath` | `string` | JavaScript code or path to .js file. |
| `InputParameter` | `string` | Script input data. |
| `ExecutionWorld` | [`NExecutionWorld`](../activities/common/NExecutionWorld.md) | Execution world: Isolated (default) or Page. |

### UITaskOptions

| Property | Type | Description |
|----------|------|-------------|
| `Task` | `string` | Task to execute. |
| `AgentType` | [`NUITaskAgentType`](../activities/common/NUITaskAgentType.md) | Agent type. |
| `InteractionMode` | [`NChildInteractionMode`](../activities/common/NChildInteractionMode.md) | Interaction mode. |
| `MaxIterations` | `int` | Max iterations. 0 = no limit. |
| `ClipboardMode` | [`NTypeByClipboardMode`](../activities/common/NTypeByClipboardMode.md) | Agent can use clipboard for typing. |
| `IsDOMEnabled` | `bool` | Agent can use native DOM. |
| `IsVariableSecurityEnabled` | `bool` | Variable security enabled. Default true. |
| `TraceAttachMode` | [`NTraceAttachMode`](../activities/common/NTraceAttachMode.md) | Trace attach mode. |

### SetRuntimeBrowserOptions

| Property | Type | Description |
|----------|------|-------------|
| `BrowserType` | [`NBrowserType`](../activities/common/NBrowserType.md) | Browser type. |

### SetClipboardOptions

| Property | Type | Description |
|----------|------|-------------|
| `Text` | `string` | Text to set on clipboard. |

### GoToUrlOptions

| Property | Type | Description |
|----------|------|-------------|
| `Url` | `string` | URL to navigate to. |

### GetRuntimeTargetOptions

| Property | Type | Description |
|----------|------|-------------|
| `Timeout` | `double` | Target search timeout (seconds). |

### TargetOptions

| Property | Type | Description |
|----------|------|-------------|
| `Timeout` | `double` | Timeout in seconds. Default 30. |
| `DelayAfter` | `double` | Delay after executing (seconds). Default 0.3. |
| `DelayBefore` | `double` | Delay before executing (seconds). Default 0.2. |

### GetAccessibilityCheckOptions

Inherits from [`TargetOptions`](#targetoptions). No additional properties.

### GetClipboardOptions

Inherits from [`TargetOptions`](#targetoptions). No additional properties.

### IsEnabledOptions

Inherits from [`TargetOptions`](#targetoptions). No additional properties.

### GetUrlOptions

Inherits from [`TargetOptions`](#targetoptions). No additional properties.

### SAPReadStatusbarOptions

Inherits from [`TargetOptions`](#targetoptions). No additional properties.

### VerifyOptions

| Property | Type | Description |
|----------|------|-------------|
| `Target` | `TargetAnchorableModel` | Element to verify at runtime. |
| `Mode` | [`NVerifyMode`](../activities/common/NVerifyMode.md) | Change to check: Appears, Disappears, TextChanges, AspectChanges. |
| `Retry` | `bool` | Retry action if verification fails. |
| `Timeout` | `double` | Verification timeout (seconds). |
| `DelayBefore` | `double` | Delay before verification (seconds). Default 0.2. |
| `IsLoose` | `bool` | Target requires application card. |
| `ExpectedText` | `string` | Expected text to compare after typing. |

### DialogHandlingOptions

Runtime form of [`DialogHandling`](../activities/common/DialogHandling.md). Full property table in that doc; in coded API, properties are raw values (e.g., `bool`, `NBrowserDialogResponse`) instead of `InArgument<T>`.

### SAPCallTransactionOptions

| Property | Type | Description |
|----------|------|-------------|
| `Transaction` | `string` | Transaction code to call. |
| `Prefix` | `string` | Transaction prefix. Default "/n". |

### SAPExpandTreeOptions

| Property | Type | Description |
|----------|------|-------------|
| `Path` | `string` | Tree node path to expand. |

### SAPExpandALVTreeOptions

| Property | Type | Description |
|----------|------|-------------|
| `Path` | `string` | ALV tree node path to expand. |

### SAPSelectDatesInCalendarOptions

| Property | Type | Description |
|----------|------|-------------|
| `SelectType` | [`NDateSelectionType`](../activities/common/NDateSelectionType.md) | Date, Range, or Week. |
| `Date` | `DateTime` | Single date to select. |
| `StartDate` | `DateTime` | Start date for range selection. |
| `EndDate` | `DateTime` | End date for range selection. |
| `Year` | `int` | Year for week selection. |
| `Week` | `int` | Week number for week selection. |

### NextPageOptions

| Property | Type | Description |
|----------|------|-------------|
| `Target` | `TargetAnchorableModel` | Next-page link target. |
| `DelayBetweenPages` | `double` | Delay between page navigations. |

### SearchedElementOptions

Runtime form of [`SearchedElement`](../activities/common/SearchedElement.md). Full property table in that doc; in coded API, type carries `Target: TargetAnchorableModel` and `Timeout: double` (activity-only `InUiElement` / `OutUiElement` handles do not apply).

## Result Types

### UITaskResult

Returned by [`UITask`](#uitask) / `UITaskAsync`. Holds ScreenPlay prompt execution outcome.

| Property | Type | Description |
|----------|------|-------------|
| `Status` | `string?` | Prompt execution status. |
| `Result` | `string?` | String result of prompt execution. |
| `ErrorMessage` | `string?` | Error message; `null` if prompt execution succeeded. |
| `HttpErrorStatusCode` | `string?` | HTTP error status code. Set only for server-side errors. |
| `TraceFiles` | `NUITaskTraceFiles?` | Trace files generated during prompt execution, controlled by project's ScreenPlay trace-files setting. `null` if none generated. |

### SAPReadStatusbarResult

Returned by [`SAPReadStatusbar`](#sapreadstatusbar). Parsed SAP statusbar message contents.

| Property | Type | Description |
|----------|------|-------------|
| `MessageType` | `string` | SAP statusbar message type (e.g. `S` success, `E` error, `W` warning, `I` info, `A` abort). |
| `MessageText` | `string` | Displayed message text. |
| `MessageId` | `string` | SAP message class identifier. |
| `MessageNumber` | `string` | Numeric message identifier within message class. |
| `MessageData` | `string[]` | Additional message parameters / placeholder values. |

## Common Patterns

### 1. Attach to a browser and click a button

```csharp
using var app = uiAutomation.Attach("MyWebApp");
app.Click("SubmitButton");
```

### 2. Open an application, type text, and read a result

```csharp
using var app = uiAutomation.Open("LoginScreen");
app.TypeInto("UsernameField", "admin@example.com");
app.TypeInto("PasswordField", new TypeIntoOptions
{
    SecureText = securePassword,
    EmptyFieldMode = NEmptyFieldMode.SingleLine
});
app.Click("LoginButton");

bool loaded = app.WaitState("Dashboard", NCheckStateMode.WaitAppear, 10);
string welcomeText = app.GetText("WelcomeLabel");
```

### 3. Extract table data from a web page with pagination

```csharp
using var app = uiAutomation.Open("ProductCatalog");
DataTable products = app.ExtractTableData("ProductsTable", new ExtractTableDataOptions
{
    LimitExtractionTo = LimitType.Rows,
    NumberOfItems = 100,
    DelayBetweenPages = 1.5
});

foreach (DataRow row in products.Rows)
{
    Log(row["ProductName"].ToString());
}
```

### 4. Use keyboard shortcuts and check element state

```csharp
using var app = uiAutomation.Attach("SpreadsheetApp");

app.Click("CellA1");
app.KeyboardShortcut("CellA1", "ctrl+c");
app.Click("CellB1");
app.KeyboardShortcut("CellB1", "ctrl+v");

bool isSaveEnabled = app.IsEnabled("SaveButton");
if (isSaveEnabled)
{
    app.Click("SaveButton");
}
```

### 5. Iterate child elements and get attributes

```csharp
using var app = uiAutomation.Attach("FileManager");

var children = app.GetChildren("FileList", new GetChildrenOptions
{
    SelectorFilter = "<webctrl tag='li' />",
    Recursive = false,
    Timeout = 10
});

foreach (var child in children)
{
    string fileName = app.GetText(child, new GetTextOptions
    {
        ScrapingMethod = NScrapingMethod.TextAttribute
    });
    string href = app.GetAttribute(child, new GetAttributeOptions { Attribute = "href" });
    Log($"File: {fileName}, Link: {href}");
}
```

## Enums

### NScrapingOptions

`UiPath.UIAutomationNext.Enums.NScrapingOptions`

Text scraping behaviour options. `[Flags]` enum — combine values with bitwise OR.

| Value | Description |
|-------|-------------|
| `NScrapingOptions.None` | No options. |
| `NScrapingOptions.AllowFormatting` | Adds spaces and new lines to align text as seen on screen. Applies only to `NScrapingMethod.Native`. |
| `NScrapingOptions.IgnoreHiddenText` | Ignores non-visible text. Visibility checked via accessibility or element bounds (ignoring empty area elements). Applies only to `NScrapingMethod.Fulltext`. |
| `NScrapingOptions.GetWordsCoordinates` | Enables word coordinates. Applies to `NScrapingMethod.Native` and `NScrapingMethod.Fulltext`. |

