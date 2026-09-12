# Target System

The target system defines how UI Automation activities locate application windows and UI elements at runtime. There are two main target types used across activities: **TargetAnchorable** (for locating UI elements within an application) and **TargetApp** (for locating the application window itself). Together, they form a hierarchical targeting model: TargetApp identifies the application, while TargetAnchorable identifies specific elements within it.

## TargetAnchorable

`TargetAnchorable` is used by most UI element activities (Click, Type Into, Get Text, etc.) to locate a specific element within an application window. It supports selectors, anchors, fuzzy matching, and offset configurations.

**Inherits from: `Target`**
**Latest version: V6**

### Own Properties

| Property | Display Name | Type | Description |
|----------|-------------|------|-------------|
| `PointOffset` | Click offset | [`PointOffset`](#pointoffset) | The offset values used to perform the click. The default is the center of the target. |
| `RegionOffset` | Area | [`RegionOffset`](#regionoffset) | The offset values for the area used to perform the action. |
| `ElementVisibilityArgument` | Visibility check | InArgument<[`NElementVisibility`](#nelementvisibility)> | When enabled, the activity also checks whether the UI element is visible or not. |
| `IsResponsive` | Responsive websites | `bool` | Enable responsive websites layout. Default: `false`. |
| `ScopeSelectorArgument` | Window selector (Application instance) | `InArgument<string>` | Selector for the application window. Only applicable when Window attach mode is set to Application instance. |
| `WaitForReadyArgument` | Wait for page load | `InArgument<NWaitForReady>` | Before performing the action, wait for the application to become ready to accept input. The options are: None - does not wait for the target to be ready; Interactive - waits until only a part of the app is loaded; Complete - waits for the entire app to be loaded. **Project setting.** |
| `SemanticSelectorArgument` | Semantic selector | `InArgument<string>` | A semantic description that defines the target. |

### Inherited Properties (from Target)

| Property | Display Name | Type | Description |
|----------|-------------|------|-------------|
| `FullSelectorArgument` | Strict selector | `InArgument<string>` | The strict selector generated for the target UI element. |
| `FuzzySelectorArgument` | Fuzzy selector | `InArgument<string>` | The fuzzy selector parameters. |
| `SearchSteps` | Targeting methods | [`TargetSearchSteps`](#targetsearchsteps) | Combined targeting methods: Selector, FuzzySelector, Image, TextNative, CV, SemanticSelector. Default: `TargetSearchSteps.None`. |
| `ImageAccuracyArgument` | Image accuracy | `InArgument<double>` | Indicates the accuracy level for image matching. Default value is 0.8. |
| `ImageOccurrenceArgument` | Image occurrence | `InArgument<int>` | Indicates a specific occurrence to be used, when multiple matches are found. A value greater than 0 indicates the nth occurrence (1-based index). Default value is 0, meaning no specific occurrence will be used. |
| `ImageFindModeArgument` | Image find mode | InArgument<[`NImageFindMode`](#nimagefindmode)> | Indicates the algorithm used for image matching. Default value is Find enhanced all. |
| `NativeTextArgument` | Native text | `InArgument<string>` | The text to find to identify the UI element. |
| `NativeTextOccurrenceArgument` | Native text occurrence | `InArgument<int>` | Indicates a specific occurrence to be used, when multiple matches are found. Default value is 0, meaning no specific occurrence will be used. |
| `IsNativeTextCaseSensitive` | Native text case-sensitive | `bool` | Indicates whether text matching is case-sensitive. Default: `false`. |
| `CvType` | CV Control type | [`UIVisionCategoryType`](#uivisioncategorytype) | Indicates the type of control identified using Computer Vision. Default: `UIVisionCategoryType.None`. |
| `CvTextArgument` | CV Text | `InArgument<string>` | Indicates the text identified using Computer Vision. |
| `CvTextOccurrenceArgument` | CV Text occurrence | `InArgument<int>` | Indicates a specific occurrence to be used, when multiple matches are found. Default value is 0, meaning no specific occurrence will be used. |
| `CvTextAccuracyArgument` | CV Text accuracy | `InArgument<double>` | Indicates the accuracy level for OCR text matching. Default value is 0.7. |

### XAML Syntax

```xml
<uix:TargetAnchorable Version="V6">
  <uix:TargetAnchorable.PointOffset>
    <uix:PointOffset />
  </uix:TargetAnchorable.PointOffset>
  <uix:TargetAnchorable.RegionOffset>
    <uix:RegionOffset />
  </uix:TargetAnchorable.RegionOffset>
  <uix:TargetAnchorable.ElementVisibilityArgument>
    <InArgument x:TypeArguments="uix:NElementVisibility" />
  </uix:TargetAnchorable.ElementVisibilityArgument>
  <uix:TargetAnchorable.WaitForReadyArgument>
    <InArgument x:TypeArguments="uix:NWaitForReady" />
  </uix:TargetAnchorable.WaitForReadyArgument>
  <uix:TargetAnchorable.SemanticSelectorArgument>
    <InArgument x:TypeArguments="x:String" />
  </uix:TargetAnchorable.SemanticSelectorArgument>
  <uix:TargetAnchorable.FullSelectorArgument>
    <InArgument x:TypeArguments="x:String">[selector]</InArgument>
  </uix:TargetAnchorable.FullSelectorArgument>
  <uix:TargetAnchorable.FuzzySelectorArgument>
    <InArgument x:TypeArguments="x:String">[fuzzySelector]</InArgument>
  </uix:TargetAnchorable.FuzzySelectorArgument>
  <uix:TargetAnchorable.ImageAccuracyArgument>
    <InArgument x:TypeArguments="x:Double">0.8</InArgument>
  </uix:TargetAnchorable.ImageAccuracyArgument>
  <uix:TargetAnchorable.NativeTextArgument>
    <InArgument x:TypeArguments="x:String">[text]</InArgument>
  </uix:TargetAnchorable.NativeTextArgument>
</uix:TargetAnchorable>
```

## Anchors

An anchor is a secondary UI element used to uniquely identify the main target. When a target's selector matches more than one element on the screen, anchors disambiguate the matches based on the **geometric relation** between the candidate target elements and the anchor element(s) — for example, the input that sits next to a specific label.

### Class model

`Target` is the base class for the targeting system:

- The **main target** of an activity is a `TargetAnchorable`, which inherits from `Target`. It defines a single main element plus **up to 4 anchors**.
- Each **anchor** is a plain `Target` (not a `TargetAnchorable`). Anchors cannot themselves have anchors.

In other words, a `TargetAnchorable` is a `Target` that additionally owns a list of anchor `Target`s.

### When anchors apply

Anchors are only used when the main target (`TargetAnchorable`) defines **at least one anchor-supporting targeting method** (search step). If the configured targeting methods do not support anchors, any anchors present are ignored.

| Supports anchors | Does **not** support anchors |
|------------------|------------------------------|
| `FuzzySelector`, `CV`, `TextNative`, `Image` | `Selector`, `SemanticSelector` |

`Selector` itself ignores anchors, but CLI `add-anchor` accepts a strict target: it atomically converts the main target to `FuzzySelector`, then attaches. Removing its last anchor reverses conversion. The table describes persisted state, not accepted CLI input.

### Choosing an anchor

Anchors are typically — though not exclusively — static text that describes the main target. Good candidates are elements that stay constant across runs and are positioned consistently relative to the target.

Examples:

- **The label of an input** — main element: `Input`, anchor: `label`.
- **The text of a button** — main element: `Button`, anchor: `text`.

### Selector vs Fuzzy Selector

On a `Target` / `TargetAnchorable`, the `Selector` search step reads its selector from `FullSelectorArgument`, while the `FuzzySelector` search step reads from `FuzzySelectorArgument`. These two targeting methods differ in how many matches they can produce, which is why one uses anchors and the other does not:

- **`Selector` (strict)** resolves to **exactly one** element. To target a specific match among several similar elements, use the `idx` attribute (e.g. `idx='2'` for the second match). When `idx` is omitted, the first match is used. Because a strict selector always resolves to a single element, it does **not** use anchors.
- **`FuzzySelector`** can match **multiple** elements and relies on anchors to resolve the specific one. The `idx` attribute is **not** supported for `FuzzySelector`.

### XAML example

A `TargetAnchorable` that targets an input via `FuzzySelector`, anchored by its "First Name" label:

```xml
<uix:TargetAnchorable DesignTimeRectangle="463, 605, 218, 55" DesignTimeScaleFactor="1.25" ElementType="InputBox" ElementVisibilityArgument="Interactive" FullSelectorArgument="&lt;webctrl id='LaqF8' tag='INPUT' /&gt;" FuzzySelectorArgument="&lt;webctrl id='LaqF8' tag='INPUT' type='' class='ng-untouched ng-pristine ng-invalid' aaname='' matching:id='fuzzy' fuzzylevel:id='0.0' matching:class='fuzzy' fuzzylevel:class='0.0' matching:aaname='fuzzy' fuzzylevel:aaname='0.0' /&gt;" Guid="47b92f6c-1242-4568-8a71-7c8ffdef3883" InformativeScreenshot="3a7e6476fcd7b2006a51c391ce3004d5.jpg" ScopeSelectorArgument="&lt;html app='chrome.exe' title='Rpa Challenge' url='https://rpachallenge.com/*' /&gt;" SearchSteps="FuzzySelector" Version="V6" WaitForReadyArgument="Interactive">
  <uix:TargetAnchorable.Anchors>
    <scg:List x:TypeArguments="uix:ITarget" Capacity="4">
      <uix:Target DesignTimeRectangle="463, 582, 77, 20" ElementType="Text" FullSelectorArgument="&lt;webctrl aaname='First Name' tag='LABEL' /&gt;" FuzzySelectorArgument="&lt;webctrl aaname='First Name' tag='LABEL' type='' class='' matching:aaname='fuzzy' fuzzylevel:aaname='0.0' matching:class='fuzzy' fuzzylevel:class='0.0' check:text='First Name' /&gt;" Guid="1386b608-591c-44b8-9a48-8699c0c719b4" SearchSteps="FuzzySelector" />
    </scg:List>
  </uix:TargetAnchorable.Anchors>
</uix:TargetAnchorable>
```

## TargetApp

`TargetApp` is used by the **Use Application/Browser** activity to identify and connect to the target application window or browser tab. `TargetApp` does **not** support anchors — it identifies a single application window. Its `Selector` property behaves like the `Selector` (strict) search step on `Target` / `TargetAnchorable`: it resolves to exactly one window.

**Latest version: V3**

| Property | Display Name | Type | Description |
|----------|-------------|------|-------------|
| `Selector` | Selector | `InArgument<string>` | List of attributes used to find a particular application window. |
| `FilePath` | File path | `InArgument<string>` | The full path to the executable file that starts the application. Used only when opening a new application instance. |
| `Arguments` | Arguments | `InArgument<string>` | Parameters to pass to the target application at startup. Used only when opening a new application or browser instance. |
| `Url` | URL | `InArgument<string>` | The URL of the web page to open. |
| `WorkingDirectory` | Working directory | `InArgument<string>` | Path of the current working directory. |

### XAML Syntax

```xml
<uix:TargetApp Version="V3">
  <uix:TargetApp.Selector>
    <InArgument x:TypeArguments="x:String">[selector]</InArgument>
  </uix:TargetApp.Selector>
  <uix:TargetApp.FilePath>
    <InArgument x:TypeArguments="x:String">[filePath]</InArgument>
  </uix:TargetApp.FilePath>
  <uix:TargetApp.Arguments>
    <InArgument x:TypeArguments="x:String">[arguments]</InArgument>
  </uix:TargetApp.Arguments>
  <uix:TargetApp.Url>
    <InArgument x:TypeArguments="x:String">[url]</InArgument>
  </uix:TargetApp.Url>
  <uix:TargetApp.WorkingDirectory>
    <InArgument x:TypeArguments="x:String">[workingDirectory]</InArgument>
  </uix:TargetApp.WorkingDirectory>
</uix:TargetApp>
```

## PointOffset

`UiPath.UIAutomationNext.PointOffset`

The offset values used to perform the click. The default is the center of the target.

| Property | Display Name | Type | Description |
|----------|-------------|------|-------------|
| `Position` | Anchoring point | `NPosition` | Describes the starting point of the cursor to which offsets from `OffsetX` and `OffsetY` are added. Available options: `TopLeft`, `TopRight`, `BottomLeft`, `BottomRight`, `Center`. Default: `Center`. |
| `X` | Offset X | `InArgument<int>` | Horizontal displacement of the cursor position according to the option selected in the Anchoring point field. |
| `Y` | Offset Y | `InArgument<int>` | Vertical displacement of the cursor position according to the option selected in the Anchoring point field. |

### XAML Syntax

```xml
<uix:PointOffset Position="Center">
  <uix:PointOffset.X>
    <InArgument x:TypeArguments="x:Int32">10</InArgument>
  </uix:PointOffset.X>
  <uix:PointOffset.Y>
    <InArgument x:TypeArguments="x:Int32">5</InArgument>
  </uix:PointOffset.Y>
</uix:PointOffset>
```

## RegionOffset

`UiPath.UIAutomationNext.RegionOffset`

The offset values for the area used to perform the action.

| Property | Display Name | Type | Description |
|----------|-------------|------|-------------|
| `Position` | Anchoring point | `NPosition` | Describes the starting point of the region to which offsets from `OffsetX` and `OffsetY` are added. Available options: `TopLeft`, `TopRight`, `BottomLeft`, `BottomRight`, `Center`. Default: `TopLeft`. |
| `X` | Offset X | `InArgument<int>` | Horizontal displacement of the region according to the option selected in the Anchoring point field. |
| `Y` | Offset Y | `InArgument<int>` | Vertical displacement of the region according to the option selected in the Anchoring point field. |
| `Width` | Width | `InArgument<int>` | Width of the offset region. |
| `Height` | Height | `InArgument<int>` | Height of the offset region. |

### XAML Syntax

```xml
<uix:RegionOffset Position="TopLeft">
  <uix:RegionOffset.X>
    <InArgument x:TypeArguments="x:Int32">0</InArgument>
  </uix:RegionOffset.X>
  <uix:RegionOffset.Y>
    <InArgument x:TypeArguments="x:Int32">0</InArgument>
  </uix:RegionOffset.Y>
  <uix:RegionOffset.Width>
    <InArgument x:TypeArguments="x:Int32">100</InArgument>
  </uix:RegionOffset.Width>
  <uix:RegionOffset.Height>
    <InArgument x:TypeArguments="x:Int32">50</InArgument>
  </uix:RegionOffset.Height>
</uix:RegionOffset>
```

## Notes

- **Version attributes**: Always specify the latest version in XAML. `TargetAnchorable` uses `Version="V6"` and `TargetApp` uses `Version="V3"`. Omitting the version or using an older version may result in legacy behavior or missing features.
- **TargetAnchorable** is embedded as a sub-object (named `Target`) in activities that interact with UI elements. It is not set directly as an activity property in the Properties panel; instead, its sub-properties appear under the Target category.
- **TargetApp** is embedded as a sub-object (named `TargetApp`) in the Use Application/Browser activity. It configures the application window identification.
- **Anchors**: `TargetAnchorable` supports up to four anchors to disambiguate elements that share similar selectors. See the [Anchors](#anchors) section for the class model, which targeting methods support anchors, and a XAML example.
- **Semantic selectors** (in `TargetAnchorable`) enable AI-powered element identification using natural language descriptions, providing resilience against UI layout changes.
- **Project settings**: Properties marked with `isProjectSetting: true` (such as `WaitForReadyArgument`) can have their defaults configured at the project level.

## Enums

### TargetSearchSteps

`UiPath.UIAutomationNext.Enums.TargetSearchSteps`

The selector types used to identify the target UI element. This is a `[Flags]` enum — values can be combined using bitwise OR.

| Value | Description |
|-------|-------------|
| `TargetSearchSteps.None` | No targeting method selected. |
| `TargetSearchSteps.Selector` | Strict selector. |
| `TargetSearchSteps.FuzzySelector` | Fuzzy selector. |
| `TargetSearchSteps.Image` | Image. |
| `TargetSearchSteps.TextNative` | Native text. |
| `TargetSearchSteps.CV` | Computer Vision. |
| `TargetSearchSteps.SemanticSelector` | Semantic selector. |

### NImageFindMode

`UiPath.UIAutomationNext.Shared.Enums.NImageFindMode`

The algorithm used for image matching when targeting elements via image.

| Value | Description |
|-------|-------------|
| `NImageFindMode.Default` | The default method for modern find image. Uses Enhanced multiple find. |
| `NImageFindMode.LegacyEnhanced` | Legacy behavior for Enhanced image find profile. |
| `NImageFindMode.LegacyBasic` | Legacy behavior for Basic image find profile. |

### UIVisionCategoryType

`UiPath.UIAutomationNext.Models.CV.UIVisionCategoryType`

The type of control identified using Computer Vision.

| Value | Description |
|-------|-------------|
| `UIVisionCategoryType.None` | None. |
| `UIVisionCategoryType.Button` | Button. |
| `UIVisionCategoryType.InputBox` | Input box. |
| `UIVisionCategoryType.CheckBox` | Checkbox. |
| `UIVisionCategoryType.RadioButton` | Radio button. |
| `UIVisionCategoryType.CloseButton` | Close button. |
| `UIVisionCategoryType.MaximizeButton` | Maximize button. |
| `UIVisionCategoryType.MinimizeButton` | Minimize button. |
| `UIVisionCategoryType.Icon` | Icon. |
| `UIVisionCategoryType.ArrowButton` | Arrow button. |
| `UIVisionCategoryType.Cell` | Cell. |
| `UIVisionCategoryType.Text` | Text. |
| `UIVisionCategoryType.Image` | Image. |
| `UIVisionCategoryType.Area` | Area. |
| `UIVisionCategoryType.AnyText` | Any Text. |
| `UIVisionCategoryType.AnyWordGroup` | Any Word Group. |
| `UIVisionCategoryType.AnyIcon` | Any Icon. |
| `UIVisionCategoryType.Table` | Table. |
| `UIVisionCategoryType.TableCell` | Table Cell. |

### NElementVisibility

`UiPath.UIAutomationNext.Enums.NElementVisibility`

Controls whether the activity also checks that the UI element is visible before performing the action.

| Value | Description |
|-------|-------------|
| `NElementVisibility.None` | No visibility checks will be performed. |
| `NElementVisibility.Interactive` | Ensures that the element is present and not hidden by other elements from the target application, ignoring page scroll and obstructions by other apps, or the fact that the application is minimized. This applies to the Fuzzy selector targeting method. |
| `NElementVisibility.Visible` | Ensures that the element is visible on the screen. |
| `NElementVisibility.Legacy` | Ensures that the element is visible on the screen. (Legacy mode) |
