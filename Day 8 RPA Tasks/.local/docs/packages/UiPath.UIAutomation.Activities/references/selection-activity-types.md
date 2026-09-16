# Selection Activity Types

Activity type tells CLI target's workflow use and tunes generated default selector. Pass per element as `activityType` in `--refs` JSON of `target-anchorable resolve-defaults` (see [`uia-configure-target`](../skills/uia-configure-target/SKILL.md), TARGET-6).

Choose from element description/request; default `None` if unclear. Values are case-sensitive; unknown values fall back to `None`. Covers only element-targeting interactions; excludes SAP, Semantic (Fill Form, …), Extract Table Data ([uia-configure-target-guide.md § Unsupported Activities](uia-configure-target-guide.md#unsupported-activities)).

### Neutral — no activity-specific tuning

| `activityType` | Interaction | Choose when |
|---|---|---|
| `None` | No specific interaction | Intended interaction unknown, or no type below fits. Produces neutral, untuned selector. |

### Pointer / focus / visual — no attribute restrictions

| `activityType` | Interaction | Choose when |
|---|---|---|
| `Click` | Click element | Element is pressed/activated (buttons, links, menu items). |
| `Hover` | Hover over element | Workflow only moves pointer over it. |
| `Highlight` | Visually highlight element | Element is highlighted for demonstration/debugging. |
| `SetFocus` | Put keyboard focus on element | Element only needs focus, without typing. |
| `TakeScreenshot` | Capture element's image | Screenshot of element is taken. |
| `MouseScroll` | Scroll over element | Element is scroll target. |
| `DragAndDrop` | Drag element / drop onto it | Element is drag source or drop destination. |
| `KeyboardShortcut` | Send keystrokes to element | Keys sent while element holds focus. |
| `InjectJsScript` | Run JavaScript against element | Script injected targeting element. |
| `GetAttribute` | Read attribute value | Arbitrary attribute read (does not identify by content). |

### Text read/write — avoid content-reflecting attributes (`text`, `aaname`, `visibleinnertext`, `innertext`)

| `activityType` | Interaction | Choose when |
|---|---|---|
| `TypeInto` | Type text into field | Element is text input workflow fills. |
| `SetText` | Set field's text directly | Text written without simulated typing. |
| `GetText` | Read text from element | Element's text content is read. |

### Checkbox / toggle — avoid state attributes (`checked`, `aastate`)

| `activityType` | Interaction | Choose when |
|---|---|---|
| `Check` | Check / uncheck control | Element is checkbox or radio button being toggled. |
| `CheckState` | Read control's state | Element's checked/enabled state read without changing it. |

### Selection — avoid value attributes (`selecteditem`, `value`)

| `activityType` | Interaction | Choose when |
|---|---|---|
| `SelectItem` | Select option | Element is dropdown / combo box / list whose option is chosen. |

### Presence / multiplicity / scope

| `activityType` | Interaction | Choose when |
|---|---|---|
| `CheckElement` | Verify element's presence/state | Workflow checks whether element exists / is in given state. |
| `FindElements` | Find all matching elements | Selector matches a **set** of elements — do not over-constrain to single instance. |
| `ForEachUiElement` | Iterate over matching elements | Workflow loops over every element matching selector. Like `FindElements`, selector matches a set. |
| `ElementScope` | Scope subsequent actions to container | Element is container scoping nested activities. |

### Triggers — monitor an element for an event (strict selector only)

| `activityType` | Interaction | Choose when |
|---|---|---|
| `ClickTrigger` | Fire when element clicked | Element watched for clicks in trigger scope. |
| `KeyboardTrigger` | Fire on key press | Element watched for keyboard input. |
| `NativeTrigger` | Fire on native UI event | Element watched for native UI event. |

### Window-level

| `activityType` | Interaction | Choose when |
|---|---|---|
| `WindowOperations` | Operate on window | Target is window (minimize/maximize/move/close), not inner element. |
