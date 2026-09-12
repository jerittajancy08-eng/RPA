# UIAutomation CLI Reference

Entry: `uip rpa uia` (for example, `uip rpa uia object-repository get-apps`).

Command shapes, examples, behavior. Grep `^##`; read matching section; get flags from `<command> --help`. Top-level `uip rpa uia --help` intentionally lists no subcommands.

## Common Options

All commands accept:

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `project-dir` | `--project-dir <project-dir>` | Project directory. | No (default: current directory) |

## Object Repository

**Base command:** `uip rpa uia object-repository`

`get-apps`, `get-screens`, and `get-elements` exit zero when nothing matches, printing a no-results message instead of a table.

### Applications

#### Create Application

Create application entry.

Syntax: `uip rpa uia object-repository create-app [options]`

```bash
# Version defaults to "1.0.0"
uip rpa uia object-repository create-app --name "My Web App"

uip rpa uia object-repository create-app --name "My Web App" --description "The main web application"

uip rpa uia object-repository create-app --name "My Web App" --version "2.0.0"
```

#### Get Applications

Get all applications.

```bash
uip rpa uia object-repository get-apps [options]
```

Only common options.

```bash
uip rpa uia object-repository get-apps

uip rpa uia object-repository get-apps --project-dir "C:\MyProject"
```

#### Get Application

Get application by reference ID.

Syntax: `uip rpa uia object-repository get-app [options]`

```bash
uip rpa uia object-repository get-app --reference-id "app-123"
```

#### Delete Application

Delete application entry.

Syntax: `uip rpa uia object-repository delete-app [options]`

```bash
uip rpa uia object-repository delete-app --reference-id "app-123"
```

### Screens

#### Create Screen

Create screen entry from definition file.

Syntax: `uip rpa uia object-repository create-screen [options]`

```bash
# Without --app-reference-id, a new application is created automatically
uip rpa uia object-repository create-screen --definition-file-path "path/to/login.xaml"

uip rpa uia object-repository create-screen --definition-file-path "path/to/login.xaml" --app-reference-id "app-123"
```

#### Get Screens

Get screens, optionally filtered.

Syntax: `uip rpa uia object-repository get-screens [options]`

```bash
uip rpa uia object-repository get-screens

# Filter by definition file's window selector
uip rpa uia object-repository get-screens --definition-file-path "path/to/definition.xaml"

uip rpa uia object-repository get-screens --app-reference-id "app-123"
```

#### Get Screen

Get screen by reference ID.

Syntax: `uip rpa uia object-repository get-screen [options]`

```bash
uip rpa uia object-repository get-screen --reference-id "screen-456"
```

#### Get Screen XAML

Get screen's XAML representation.

Syntax: `uip rpa uia object-repository get-screen-xaml [options]`

```bash
uip rpa uia object-repository get-screen-xaml --reference-id "screen-456"
```

#### Delete Screen

Delete screen entry.

Syntax: `uip rpa uia object-repository delete-screen [options]`

```bash
uip rpa uia object-repository delete-screen --reference-id "screen-456"
```

### Elements

#### Create Elements

Create multiple element entries from definition files in single batch.

Syntax: `uip rpa uia object-repository create-elements [options]`

```bash
uip rpa uia object-repository create-elements \
  --screen-reference-id "screen-456" \
  --definition-file-paths "path/to/username.xaml,path/to/password.xaml"
```

#### Get Elements

Get elements for given screen.

Syntax: `uip rpa uia object-repository get-elements [options]`

```bash
uip rpa uia object-repository get-elements --screen-reference-id "screen-456"
```

#### Get Element

Get element by reference ID.

Syntax: `uip rpa uia object-repository get-element [options]`

```bash
uip rpa uia object-repository get-element --reference-id "elem-789"
```

#### Get Elements XAML

Get XAML representation of multiple elements in single batch.

Syntax: `uip rpa uia object-repository get-elements-xaml [options]`

```bash
uip rpa uia object-repository get-elements-xaml --reference-ids "elem-789,elem-012"
```

#### Delete Element

Delete element entry.

Syntax: `uip rpa uia object-repository delete-element [options]`

```bash
uip rpa uia object-repository delete-element --reference-id "elem-789"
```

#### Replace Elements

Replace multiple elements from definition files in single batch.

Syntax: `uip rpa uia object-repository replace-elements [options]`

```bash
uip rpa uia object-repository replace-elements \
  --elements '[{"referenceId":"elem-789","definitionFilePath":"path/to/username.xaml"}]'

uip rpa uia object-repository replace-elements \
  --elements '[{"referenceId":"elem-789","definitionFilePath":"path/to/username.xaml"},{"referenceId":"elem-012","definitionFilePath":"path/to/password.xaml"}]'
```

### Linking

#### Link Elements

Link one or more elements to activities in XAML workflow.

Syntax: `uip rpa uia object-repository link-elements [options]`

```bash
# targetProperty defaults to "Target"
uip rpa uia object-repository link-elements \
  --elements '[{"workflowFilePath":"Main.xaml","activityId":"act-123","referenceId":"elem-789"}]'

# Per-entry target properties and workflow paths
uip rpa uia object-repository link-elements \
  --elements '[{"workflowFilePath":"Main.xaml","activityId":"act-123","referenceId":"elem-789","targetProperty":"Target"},{"workflowFilePath":"Main.xaml","activityId":"act-456","referenceId":"elem-012","targetProperty":"SearchedElement.Target"}]'
```

#### Link Screen

Link screen to activity in XAML workflow.

Syntax: `uip rpa uia object-repository link-screen [options]`

```bash
uip rpa uia object-repository link-screen \
  --workflow-file-path "Main.xaml" \
  --activity-id "act-456" \
  --reference-id "screen-789"
```

### Definitions

#### Get Element Definition

Get element definition.

Syntax: `uip rpa uia object-repository get-element-definition [options]`

```bash
uip rpa uia object-repository get-element-definition \
  --definition-file-path "path/to/target.xaml" \
  --reference-id "elem-789"
```

#### Get Screen Definition

Get screen definition.

Syntax: `uip rpa uia object-repository get-screen-definition [options]`

```bash
uip rpa uia object-repository get-screen-definition \
  --definition-file-path "path/to/target.xaml" \
  --reference-id "screen-456"
```

## Target Anchorable

**Base command:** `uip rpa uia target-anchorable`

### Link

Link one or more target anchorable definition files to activities in XAML workflow.

Syntax: `uip rpa uia target-anchorable link [options]`

```bash
uip rpa uia target-anchorable link \
  --targets '[{"workflowFilePath":"Main.xaml","activityId":"act-123","definitionFilePath":"path/to/target.xaml"}]'

# Per-entry target properties and workflow paths
uip rpa uia target-anchorable link \
  --targets '[{"workflowFilePath":"Main.xaml","activityId":"act-123","definitionFilePath":"path/to/t1.xaml"},{"workflowFilePath":"Main.xaml","activityId":"act-456","definitionFilePath":"path/to/t2.xaml","targetProperty":"SearchedElement.Target"}]'
```

### Get Definition

Get target anchorable definition.

Syntax: `uip rpa uia target-anchorable get-definition [options]`

```bash
uip rpa uia target-anchorable get-definition \
  --definition-file-path "path/to/target.xaml" \
  --activity-id "act-1" \
  --workflow-file-path "workflows/Main.xaml"

uip rpa uia target-anchorable get-definition \
  --definition-file-path "path/to/target.xaml" \
  --activity-id "act-1" \
  --workflow-file-path "workflows/Main.xaml" \
  --target-property "SearchedElement.Target"
```

### Validate

Validate target anchorable definition against live application. Probes live UI for target and its anchors, reports whether it matched. On match, saves screenshot with matched element highlighted, prints its path. Works for both selector-based and Computer Vision (CV) definitions.

Syntax: `uip rpa uia target-anchorable validate [options]`

```bash
uip rpa uia target-anchorable validate \
  --definition-file-path "path/to/target.xaml"
```

### Preview Text

Extract text a Get Text activity would return for target anchorable definition. Searches target against live application, scrapes its text — same as Studio's Preview Extraction option. Prints extracted text; an element with no text succeeds and reports an empty extraction.

Syntax: `uip rpa uia target-anchorable preview-text [options]`

```bash
uip rpa uia target-anchorable preview-text \
  --definition-file-path "path/to/target.xaml"

# Specific scraping method
uip rpa uia target-anchorable preview-text \
  --definition-file-path "path/to/target.xaml" \
  --scraping-method "Fulltext"
```

### Update Definition

Update target anchorable definition file. Only provided options updated; omitted unchanged.

Syntax: `uip rpa uia target-anchorable update-definition [options]`

```bash
uip rpa uia target-anchorable update-definition \
  --definition-file-path "path/to/target.xaml" \
  --name "Login Button" \
  --full-selector "<wnd cls='LoginForm' /><ctrl name='Login' role='button' />"

uip rpa uia target-anchorable update-definition \
  --definition-file-path "path/to/target.xaml" \
  --scope-selector "<wnd cls='LoginForm' />" \
  --activity-type "Click"

uip rpa uia target-anchorable update-definition \
  --definition-file-path "path/to/target.xaml" \
  --semantic-selector "the primary Login button below the password field" \
  --validate
```

### Resolve Defaults

Resolve target anchorable definitions from snapshot refs (`w*`/`b*`/`e*`) or CV refs (`cve*` element / `cvw*` word); write one definition file per entry. CV refs resolve into CV-based definitions. Ref kinds can mix in one call; failures reported per entry. Each entry may also carry `name`/`description`, written to the created definition's metadata immediately (CV refs no longer default `name` to the application's name — set it explicitly if wanted).

Syntax: `uip rpa uia target-anchorable resolve-defaults [options]`

```bash
uip rpa uia target-anchorable resolve-defaults \
  --refs '[{"ref":"e42","definitionFilePath":"path/to/target.xaml","activityType":"Click"}]' \
  --window-definition-file-path "path/to/window.xaml" \
  --window-ref "w1"

# Per-entry activity types
uip rpa uia target-anchorable resolve-defaults \
  --refs '[{"ref":"e28","definitionFilePath":"path1.xaml","activityType":"Click"},{"ref":"e30","definitionFilePath":"path2.xaml","activityType":"TypeInto"}]' \
  --window-definition-file-path "path/to/window.xaml"

# Per-entry name/description, set on creation
uip rpa uia target-anchorable resolve-defaults \
  --refs '[{"ref":"e28","definitionFilePath":"path1.xaml","activityType":"Click","name":"Login Button","description":"Button that submits the login form"}]' \
  --window-definition-file-path "path/to/window.xaml"

# CV refs picked from cv-tree.yml (requires prior `snapshot capture --type cv`)
uip rpa uia target-anchorable resolve-defaults \
  --refs '[{"ref":"cve4","definitionFilePath":"path1.xaml","activityType":"Click"},{"ref":"cvw3","definitionFilePath":"path2.xaml","activityType":"GetText"}]' \
  --window-definition-file-path "path/to/window.xaml"

# From semantic description (no element ref needed): semantic-selector-only target.
uip rpa uia target-anchorable resolve-defaults \
  --refs '[{"definitionFilePath":"path/to/target.xaml","activityType":"Click","semanticDescription":"the submit button"}]' \
  --window-definition-file-path "path/to/window.xaml" \
  --window-ref "w1"
```

> **Tip:** Inline JSON with Windows `C:\…` paths fails: `\U` etc. are invalid JSON escapes. Use forward slashes (`C:/Users/...`) inside JSON. Same applies to `--elements` / `--targets` on link commands.

### Add Anchor

Resolve anchor element from default or CV snapshot ref, append to target anchorable definition. Target anchorable holds up to four anchors (slots `0..3`); new anchor written to next free slot, command returns that slot index.

Syntax: `uip rpa uia target-anchorable add-anchor [options]`

```bash
uip rpa uia target-anchorable add-anchor \
  --element-ref "e28" \
  --window-ref "w1" \
  --definition-file-path "path/to/target.xaml"

uip rpa uia target-anchorable add-anchor \
  --element-ref "e28" \
  --window-ref "w1" \
  --definition-file-path "path/to/target.xaml" \
  --name "TitleBar" \
  --description "The window title bar"

# CV anchor from word ref (requires prior `snapshot capture --type cv`)
uip rpa uia target-anchorable add-anchor \
  --element-ref "cvw3" \
  --window-ref "w1" \
  --definition-file-path "path/to/target.xaml" \
  --name "SubmitLabel" \
  --description "The static 'Submit' caption next to the button"
```

### Remove Anchor

Remove anchor at given index from target anchorable definition. Metadata slots shift down to stay aligned with remaining anchors.

Syntax: `uip rpa uia target-anchorable remove-anchor [options]`

```bash
uip rpa uia target-anchorable remove-anchor \
  --definition-file-path "path/to/target.xaml" \
  --index 0
```

### Update Anchor Definition

Update anchor properties at given index on target anchorable definition. Only provided options updated; omitted unchanged.

Syntax: `uip rpa uia target-anchorable update-anchor-definition [options]`

```bash
uip rpa uia target-anchorable update-anchor-definition \
  --definition-file-path "path/to/target.xaml" \
  --index 0 \
  --name "TitleBar" \
  --description "The window title bar"

uip rpa uia target-anchorable update-anchor-definition \
  --definition-file-path "path/to/target.xaml" \
  --index 1 \
  --full-selector "<ctrl name='OK' role='button' />"

# Update CV anchor's text; pick second on-screen occurrence
uip rpa uia target-anchorable update-anchor-definition \
  --definition-file-path "path/to/target.xaml" \
  --index 2 \
  --cv-text "Submit" \
  --cv-text-occurrence 2
```

### Fuzzify

Fuzzify selector of one or more target anchorables. Per definition, strict full selector converts to fuzzy selector: fuzzy search step enabled, strict selector step disabled. Definitions are processed independently — failure on one entry does not abort others.

```bash
uip rpa uia target-anchorable fuzzify [options]
```

Definition fails to fuzzify (reported with an `Error:` line) when its fuzzy selector already enabled, no strict selector enabled to convert, or fuzzy selector generation fails.

```bash
uip rpa uia target-anchorable fuzzify \
  --definition-file-paths '["path/to/target.xaml"]'

uip rpa uia target-anchorable fuzzify \
  --definition-file-paths '["path/to/t1.xaml","path/to/t2.xaml"]'
```

### Fuzzify Anchors

Fuzzify selector of an anchor within target anchorable definition file. Like `fuzzify`, but targets anchor at given slot index rather than target itself. Entries are processed independently — failure on one entry does not abort others.

```bash
uip rpa uia target-anchorable fuzzify-anchors [options]
```

Anchor fails to fuzzify (reported with an `Error:` line) when index out of range, its fuzzy selector already enabled, no strict selector enabled to convert, or fuzzy selector generation fails.

```bash
uip rpa uia target-anchorable fuzzify-anchors \
  --anchors '[{"definitionFilePath":"path/to/target.xaml","index":0}]'

uip rpa uia target-anchorable fuzzify-anchors \
  --anchors '[{"definitionFilePath":"path/to/t1.xaml","index":0},{"definitionFilePath":"path/to/t2.xaml","index":1}]'
```

## Target App

**Base command:** `uip rpa uia target-app`

### Link

Link target application to activity in XAML workflow.

Syntax: `uip rpa uia target-app link [options]`

```bash
uip rpa uia target-app link \
  --definition-file-path "path/to/app.xaml" \
  --workflow-file-path "Main.xaml" \
  --activity-id "act-123"
```

### Get Definition

Get target application definition.

Syntax: `uip rpa uia target-app get-definition [options]`

```bash
uip rpa uia target-app get-definition \
  --definition-file-path "path/to/target.xaml" \
  --activity-id "act-1" \
  --workflow-file-path "workflows/Main.xaml"
```

### Update Definition

Update target app definition file. Only provided options updated; omitted unchanged.

Syntax: `uip rpa uia target-app update-definition [options]`

```bash
uip rpa uia target-app update-definition \
  --definition-file-path "path/to/app.xaml" \
  --name "My Web App" \
  --selector "<wnd cls='MainWindow' />"
```

### Resolve Defaults

Resolve target apps from snapshot refs; write target definition files.

Syntax: `uip rpa uia target-app resolve-defaults [options]`

```bash
uip rpa uia target-app resolve-defaults \
  --refs '[{"ref":"w1","definitionFilePath":"path/to/app.xaml"}]' \
  --name "My Web App"

uip rpa uia target-app resolve-defaults \
  --refs '[{"ref":"w1","definitionFilePath":"path/to/app.xaml"}]' \
  --name "My Web App" \
  --description "The main web application"
```

### Validate

Validate target application definition against live desktop. Probes live UI for application window, reports whether it matched. On match, saves screenshot of application, prints its path.

Syntax: `uip rpa uia target-app validate [options]`

```bash
uip rpa uia target-app validate \
  --definition-file-path "path/to/app.xaml"
```

## Snapshot

**Base command:** `uip rpa uia snapshot`

**Framework values by platform.** `Default` and `Java` everywhere; `UIA` and `AA` Windows-only; `AX` macOS-only.

**Ref lifecycle.** `uia interact` resolves against latest snapshot regardless of producer. New snapshot replaces prior refs:

- **Top-level snapshots** reset **all** refs and mint fresh `wN`/`bN` — they carry no `eN` refs.
- **App-level snapshots** re-mint `eN`; `wN`/`bN` remain until top-level snapshot changes.
- Old ref may not report stale; it can resolve to current element with same number.

### Capture

Capture DOM snapshot of live application.

Syntax: `uip rpa uia snapshot capture [options]`

```bash
# Scoped to a target definition (pin exact window/element by ref)
uip rpa uia snapshot capture \
  --folder-path "path/to/output" \
  --definition-file-path "path/to/target.xaml" \
  --window-ref "w1" \
  --element-ref "e28"

# Programmatic elements from the window's tree
uip rpa uia snapshot capture \
  --folder-path "path/to/output" \
  --definition-file-path "path/to/window.xaml" \
  --window-ref "w1"

# Elements visible in the view (Computer Vision)
uip rpa uia snapshot capture \
  --folder-path "path/to/output" \
  --definition-file-path "path/to/window.xaml" \
  --type cv

# Top-level windows (no target definition)
uip rpa uia snapshot capture \
  --folder-path "path/to/output"

# Same, but keep every top-level window in the rendered tree
uip rpa uia snapshot capture \
  --folder-path "path/to/output" \
  --all-windows
```

### Semantic Find

Find elements in window or browser tab by natural-language description. Recommended when app is not supported by tree-based snapshot flow. Returns up to 3 matches, each outlined on annotated screenshot in distinct colour and minted a semantic ref (`s1`, `s2`, …) for `interact click`/`type`/`hover`/`wheel`.

```bash
uip rpa uia snapshot semantic-find <w/b-ref> "<description>" [options]
```

Matches in no specific order — identify right one visually by outline colour (each match reports its colour name and hex). No matches and too-many-matches both exit non-zero; refine description. s-refs are screen positions captured at find time: each run resets previous ones, and they go stale if layout changes.

```bash
uip rpa uia snapshot semantic-find b2 "the red Submit button"
uip rpa uia interact click s1

uip rpa uia snapshot semantic-find w1 "row checkbox"
uip rpa uia interact click s2

# Avoid hard-to-spot highlight colours
uip rpa uia snapshot semantic-find w1 "the search box" --exclude-color yellow --exclude-color green

```

## Selector Intelligence

**Base command:** `uip rpa uia selector-intelligence`

### Get Instructions

Get instructions based on runtime data.

Syntax: `uip rpa uia selector-intelligence get-instructions [options]`

```bash
uip rpa uia selector-intelligence get-instructions \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/target.xaml" \
  --mode "recover"

uip rpa uia selector-intelligence get-instructions \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/target.xaml" \
  --mode "improve"
```

### Evaluate

Evaluate selector based on runtime data. Result reports `IsValid`, `MatchesOriginalTarget`, `MatchedWindowIds`, `FinalScore`, and `ToolingFeedback` per candidate.

- **With `--improve-selector-response-file-path`:** candidate selectors in that response file are scored.
- **Without it:** selector already stored in definition file is evaluated. Supported for **window selectors only** (window-only definition such as `window.xaml`); element/full selector requires the response file.

Syntax: `uip rpa uia selector-intelligence evaluate [options]`

```bash
# Window selector already stored in a window definition (no response file)
uip rpa uia selector-intelligence evaluate \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/window.xaml" \
  --mode "improve"

# Candidate selectors from an improve-selector response file
uip rpa uia selector-intelligence evaluate \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/target.xaml" \
  --mode "improve" \
  --improve-selector-response-file-path "path/to/response.xaml"
```

## Interact

Drive current snapshot refs (`wN`, `bN`, `eN`, `sN`).

**Base command:** `uip rpa uia interact`

> `--folder-path` is invalid for `interact`; it uses latest snapshot.

Use after `snapshot capture` or current-screen `uia-configure-target` capture. Older refs are reinterpreted against latest snapshot and may resolve differently or fail; see [Snapshot](#snapshot). Every leaf/sub-verb accepts `--visualize` for brief target highlight.

Some verbs accept semantic screen-position refs (`sN`) from last `semantic-find`; rerun after layout change.

**Input methods by platform.** `HardwareEvents`, `Simulate` and `DebuggerApi` everywhere; `WindowMessages` Windows-only.

### Click

Click UI element.

Syntax: `uip rpa uia interact click <e/s-ref> [options]`

```bash
# Single left click
uip rpa uia interact click e42

uip rpa uia interact click e42 --button Right --modifiers "Shift,Ctrl"

uip rpa uia interact click e42 --type Double --origin TopLeft --offset-x 5 --offset-y 10
```

### Type

Type text into element.

Syntax: `uip rpa uia interact type <e/s-ref> <text> [options]`

```bash
uip rpa uia interact type e42 "hello world"

# Clear a single-line field first, then type
uip rpa uia interact type e42 "new value" --clear-before-mode SingleLine
```

> **Note:** With an s-ref target, element is not clicked/focused before typing unless `--click-before-mode` is set — keystrokes go to whichever element has keyboard focus. If text landed in the wrong place, rerun with `--click-before-mode Single`.

### Hover

Hover over UI element.

Syntax: `uip rpa uia interact hover <e/s-ref> [options]`

```bash
uip rpa uia interact hover e42

# Pixel offset from the default origin (center)
uip rpa uia interact hover e42 --offset-x 10 --offset-y -5

uip rpa uia interact hover e42 --origin TopLeft --offset-x 5 --offset-y -10
```

### Select

Select item from dropdown.

Syntax: `uip rpa uia interact select <e-ref> <value> [options]`

```bash
uip rpa uia interact select e73 "Second"
```

Works on option-list controls across UI stacks — native `<select>`, desktop combo boxes (WinForms/WPF), Java and SAP lists, web/ARIA comboboxes alike. To decide whether a control is `select`-drivable, read its valid options, and confirm result after selecting: see [select-item-usage-guide.md](select-item-usage-guide.md).

### Wheel

Scroll UI element with mouse wheel.

Syntax: `uip rpa uia interact wheel <e/s-ref> [options]`

```bash
uip rpa uia interact wheel e5 --direction Down --units 10

# Pixel offset from the default origin (center)
uip rpa uia interact wheel e5 --direction Down --units 10 --offset-x 50 --offset-y 0

uip rpa uia interact wheel e5 --direction Down --origin TopLeft --offset-x 30 --offset-y 30
```

> **Note:** Scrolling changes the layout, invalidating all s-refs; rerun `uia snapshot semantic-find` before reuse.

### Focus

Bring target into view and focus it.

Syntax: `uip rpa uia interact focus <any-ref> [options]`

### Screenshot

Screenshot entire screen or specific target.

Syntax: `uip rpa uia interact screenshot [any-ref] [options]`

```bash
# Full desktop screenshot
uip rpa uia interact screenshot

uip rpa uia interact screenshot w1

# Full-page browser screenshot
uip rpa uia interact screenshot b2 --full-page

```

### Highlight

Writes screenshot of target's window with colored box drawn around target, confirming what was highlighted. With `--visualize`, same border is also drawn on live screen for fixed duration; `--duration` applies only to that on-screen border.

Syntax: `uip rpa uia interact highlight <any-ref> [options]`

```bash
uip rpa uia interact highlight e5 --color Blue

# Also draw the border on the live screen
uip rpa uia interact highlight e5 --color Blue --duration 5 --visualize

```

### Get

Read attribute value from target.

Syntax: `uip rpa uia interact get <any-ref> <attribute> [options]`

### Get All

Read all available attribute values from target.

Syntax: `uip rpa uia interact get-all <any-ref> [options]`

### Extract Table

Extract structured table data from element.

Syntax: `uip rpa uia interact extract-table <e-ref> [options]`

### Window Close

Close window or browser tab.

Syntax: `uip rpa uia interact window close <w/b-ref> [options]`

### Window Foreground

Bring window to foreground.

Syntax: `uip rpa uia interact window foreground <w/b-ref> [options]`

### Window Maximize

Maximize window.

Syntax: `uip rpa uia interact window maximize <w/b-ref> [options]`

### Window Minimize

Minimize window.

Syntax: `uip rpa uia interact window minimize <w/b-ref> [options]`

### Window Restore

Restore window.

Syntax: `uip rpa uia interact window restore <w/b-ref> [options]`

### Window Hide

Hide window.

Syntax: `uip rpa uia interact window hide <w/b-ref> [options]`

### Window Show

Show window.

Syntax: `uip rpa uia interact window show <w/b-ref> [options]`

### Browser Open

Launch new browser and navigate to URL.

Syntax: `uip rpa uia interact browser open [url] [options]`

```bash
# Open the default browser
uip rpa uia interact browser open

uip rpa uia interact browser open "https://example.com" --browser Edge
```

### Browser Navigate

Navigate browser tab to URL.

Syntax: `uip rpa uia interact browser navigate <b-ref> <url> [options]`

```bash
uip rpa uia interact browser navigate b1 "https://example.com"
```

### Browser Eval

Execute JavaScript in browser tab or on element.

Syntax: `uip rpa uia interact browser eval <any-ref> <script> [options]`

```bash
# Tab-level eval
uip rpa uia interact browser eval b1 "() => document.title"

# Element-level eval
uip rpa uia interact browser eval e5 "(el) => el.textContent"

# Run in an isolated world
uip rpa uia interact browser eval b1 "() => document.title" --world Isolated
```

### Browser Tab New

Open new tab in given browser.

Syntax: `uip rpa uia interact browser tab-new <b-ref> [url] [options]`

```bash
uip rpa uia interact browser tab-new b1 "https://example.com"
```

### Browser Tab Close

Close browser tab.

Syntax: `uip rpa uia interact browser tab-close <b-ref> [options]`

### Browser Tab Select

Switch to browser tab.

Syntax: `uip rpa uia interact browser tab-select <b-ref> [options]`

### Browser Go Back

Navigate back in browser history.

Syntax: `uip rpa uia interact browser go-back <b-ref> [options]`

### Browser Go Forward

Navigate forward in browser history.

Syntax: `uip rpa uia interact browser go-forward <b-ref> [options]`

### Browser Reload

Reload current page.

Syntax: `uip rpa uia interact browser reload <b-ref> [options]`

### SAP Logon

Launch SAP Logon application and open a connection by name. On success prints opened session's `sapSysSessionId` (`sapSysSessionId=<id>`) — server-side session identifier. To target the session, run `uip rpa uia snapshot capture`, pick the window whose `sapSysSessionId` matches; pass that window's `wN` ref to `sap login`.

Syntax: `uip rpa uia interact sap logon <connection> [options]`

```bash
uip rpa uia interact sap logon "MyConnection"
```

### SAP Login

Fill SAP login screen (client / user / password / language) on an already-open SAP window — the `w-ref` found via `uip rpa uia snapshot capture`, matching the window by the `sapSysSessionId` that `sap logon` printed. The password is **never** passed as a command-line value (which would leak into argv, shell history, and logs): supply it via stdin (piped from a file or secret store) **or** name an environment variable that holds it.

```bash
# stdin, read from a secret file (not a literal):
cat secret.txt | uip rpa uia interact sap login <w-ref> --user <name> --client <nnn> --language <xx> --password-stdin [options]

# or name an environment variable, set out-of-band in a separate step:
export SAP_PASSWORD="$(cat secret.txt)"
uip rpa uia interact sap login <w-ref> --user <name> --client <nnn> --language <xx> --password-env SAP_PASSWORD [options]
```

¹ Exactly one of `--password-stdin` / `--password-env` is required; specifying both is an error.

```bash
# Preferred for unattended/agent runs — set SAP_PASSWORD out-of-band in a separate step, then run:
export SAP_PASSWORD="$(cat secret.txt)"
uip rpa uia interact sap login w1 --user <USER> --client 100 --language EN --password-env SAP_PASSWORD

# Or pipe the secret straight from a file via stdin:
cat secret.txt | uip rpa uia interact sap login w1 --user <USER> --client 100 --language EN --password-stdin
```

### SAP Call Transaction

Run transaction code in open SAP session window (`w-ref` from `snapshot capture`). Okcd-aware equivalent of typing `/nXXXX` into command field.

```bash
uip rpa uia interact sap call-transaction <w-ref> <transaction> [options]
```

```bash
uip rpa uia interact sap call-transaction w1 VA01
```

### SAP Read Status Bar

Read active SAP session's status bar on given window; print message type / number / text.

```bash
uip rpa uia interact sap read-statusbar <w-ref> [options]
```

Prints `Status bar [<type>] <number>: <text>`.
```bash
uip rpa uia interact sap read-statusbar w1
```

### SAP Expand Tree

Expand node in SAP tree control by its slash-separated path (node's `relpath` returned by `interact get <treeRef> items`).

Syntax: `uip rpa uia interact sap expand-tree <e-ref> <node-path> [options]`

```bash
uip rpa uia interact sap expand-tree e12 "Root/Child"
```

### SAP Select Dates In Calendar

Select a date, date range, or week in SAP calendar control (`e-ref` from `snapshot capture`). `--select-type` chooses which date inputs apply.

```bash
uip rpa uia interact sap select-dates-in-calendar <e-ref> [options]
```

Single-date example:

```bash
uip rpa uia interact sap select-dates-in-calendar e34 --date 2026-03-15
```

### SAP Click Picture On Screen

Click SAP picture/image control (`e-ref` from `snapshot capture`). Optionally click at offset within picture and/or hold keyboard modifiers.

Syntax: `uip rpa uia interact sap click-picture-on-screen <e-ref> [options]`

```bash
uip rpa uia interact sap click-picture-on-screen e21 --click-type Double
```

> **Note:** pass picture's inner **`Image`** node, not surrounding `Container`.

## Indicate

User-click fallback for targets unavailable to automated `uia-configure-target` capture (for example, interaction-revealed compose form).

> Screen-blocking. Confirm readiness first; no short timeout (minutes, not seconds, if any).

> Top-level `uip rpa` commands, outside `uip rpa uia`.

**Base commands:** `uip rpa indicate-application`, `uip rpa indicate-element`

Returns `{ "Data": { "reference": "..." } }` for OR lookup/attachment. Studio regenerates OR files; coded workflows must reread `ObjectRepository.cs` descriptor paths.

**Workflow:** indicate screen first, then elements within it using `--parent-id` from screen's `Data.reference`.

### Indicate Application

Create screen entry in Object Repository from user click on target window.

```bash
uip rpa indicate-application [options]
```

When no App exists in `.objects/`, omit `--parent-id` and `--parent-name` — command creates App + AppVersion automatically. When adding to existing App, provide `--parent-id` with **AppVersion** reference.

```bash
uip rpa indicate-application \
  --name "LoginScreen" \
  --description "Main login screen" \
  --project-dir "<PROJECT_DIR>" \
  --output json
```

**Troubleshooting:**

| Error | Cause | Recovery |
|-------|-------|----------|
| `"No application version found matching parentId=..."` | AppVersion reference stale or App never created. | Re-read `.objects/` metadata for fresh reference. If no App exists, call `indicate-application` without `--parent-id` — creates App automatically. |
| `.objects/` has subdirectories but no `.metadata` files | Corrupted/incomplete App from failed creation. | Clear orphan directories and run `indicate-application` without `--parent-id`. |

### Indicate Element

Create element entry under existing screen in Object Repository, from user click on target element.

Syntax: `uip rpa indicate-element [options]`

```bash
uip rpa indicate-element \
  --name "UsernameField" \
  --activity-class-name "TypeInto" \
  --parent-id "<screen-reference>" \
  --project-dir "<PROJECT_DIR>" \
  --output json
```
