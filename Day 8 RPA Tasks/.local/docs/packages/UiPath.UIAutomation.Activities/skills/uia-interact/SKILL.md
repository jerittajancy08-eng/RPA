---
name: uia-interact
description: "Inspect and interact with live desktop/browser apps -- click buttons, type text, read values, take screenshots, inspect UI state, verify behavior, fill forms, navigate menus, and extract table data from running applications"
allowed-tools: Bash(uip:*), Bash, Read
---

# UI Interaction via "uip rpa uia"

## CLI

```bash
CLI="uip rpa uia"
```

If `$PROJECT_DIR` set, append it: `CLI="uip rpa uia --project-dir \"$PROJECT_DIR\""`. All subsequent `"$CLI" ...` commands then include it.

**Commands producing output print the output-file path to stdout — read it.**

**Prior step already captured a tree (e.g., `uia-configure-target`)? Grep it for the ref, call `uia interact` directly -- do NOT re-run `uia snapshot`.** Re-snapshot only when UI changed since capture.

## Quick Start

```bash
# Tree already captured? Reuse refs:
grep -n "Submit" tree.yml             # Find [ref=eN] for the target
"$CLI" interact click e42             # Use it directly

# Otherwise, fresh inspection:
"$CLI" snapshot                       # List top-level windows and browser tabs
"$CLI" snapshot w1                    # Inspect a window to get its UI tree
"$CLI" interact click e5              # Interact using element refs from the snapshot
"$CLI" interact type e3 "hello"
"$CLI" interact select e8 "Option B"
"$CLI" snapshot w1                    # Re-inspect after UI changes -- refs may be stale
"$CLI" interact screenshot w1         # Take a screenshot
"$CLI" snapshot semantic-find w1 "the red Submit button"  # No usable tree? Find by description, then act on s-refs
```

Example stdout with file output:

```
### Top-level snapshot (11 window(s), 2 browser tab(s))
- [Tree](.local/.uia/.interact/output/snapshot-2026-04-24T14-32-15-001.yml)
- [Node tree info](.local/.uia/.interact/output/snapshot-2026-04-24T14-32-15-001.json)
- [Screenshot](.local/.uia/.interact/output/snapshot-2026-04-24T14-32-15-001.png)
```

Read linked Tree file for full results.

## Commands

All commands accept `--help`. `--visualize` available on every verb — use for visual confirmation; omitted below.

### Discover

```bash
"$CLI" snapshot                     # List windows (w-refs) and browser tabs (b-refs)
"$CLI" snapshot --all-windows       # Show ALL top-level targets (disables automatic filtering)
"$CLI" snapshot w1                  # Capture UI tree with element refs (e-refs)
"$CLI" snapshot b1                  # Inspect a browser tab
"$CLI" snapshot w1 --framework UIA  # Use specific UI framework
"$CLI" snapshot semantic-find w1 "the red Submit button"  # Find elements by natural-language description (s-refs)
```

### Core

```bash
"$CLI" interact click e5                                         # Click (left, single)
"$CLI" interact click e5 --button Right                          # Right-click
"$CLI" interact click e5 --type Double                           # Double-click
"$CLI" interact click e5 --input-method Simulate                 # Click via element API (background windows)
"$CLI" interact click e5 --offset-x 10 --offset-y -5             # Click with pixel offset from center (default origin)
"$CLI" interact click e5 --origin TopLeft --offset-x 5 --offset-y -10 # Origin: Center, TopLeft, TopRight, BottomLeft, BottomRight
"$CLI" interact click e5 --modifiers Ctrl                        # Ctrl+click (e.g. multi-select in lists)
"$CLI" interact click e5 --modifiers "Ctrl,Shift"                # Ctrl+Shift+click
"$CLI" interact hover e4                                         # Hover over element (center)
"$CLI" interact hover e4 --offset-x 10 --offset-y -5             # Hover with pixel offset from center (default origin)
"$CLI" interact hover e4 --origin TopLeft --offset-x 5 --offset-y -10 # Origin: Center, TopLeft, TopRight, BottomLeft, BottomRight
"$CLI" interact type e3 "some text"                              # Type into element
"$CLI" interact type e3 "text" --clear-before-mode SingleLine    # Clear field, then type (implies click-before)
"$CLI" interact type e3 "text" --click-before-mode Single        # Click field before typing (auto-enabled for HardwareEvents)
"$CLI" interact type e3 "text" --input-method Simulate           # Use Simulate (may auto-clear)
"$CLI" interact type e3 "text" --input-method DebuggerApi        # Use browser debugger protocol (recommended for Chrome/Edge)
"$CLI" interact type e3 "[d(ctrl)]a[u(ctrl)]"                    # Select all (Ctrl+A)
"$CLI" interact select e8 "Option B"                             # Select "Option B" from dropdown/list
"$CLI" interact wheel e5 --direction Down --units 10             # Scroll down 10 clicks
"$CLI" interact wheel e5 --direction Down --units 10 --offset-x 50 --offset-y 0 # Scroll with offset from center
"$CLI" interact wheel e5 --direction Down --origin TopLeft --offset-x 30 --offset-y 30 # Scroll anchored to origin
"$CLI" interact focus e5                                         # Bring element into view and focus
```

### Window

```bash
"$CLI" interact window foreground w2    # Bring window to front
"$CLI" interact window close b1         # Close browser tab
"$CLI" interact window maximize w1      # Actions: close, foreground, maximize, minimize, restore, hide, show
```

### Browser

```bash
"$CLI" interact browser open                                    # Open default browser
"$CLI" interact browser open "https://example.com" --browser Edge # Open in specific browser (Chrome, Edge, Firefox)
"$CLI" interact browser navigate b1 "https://example.com"       # Navigate tab to URL
"$CLI" interact browser eval b1 "() => document.title"          # Execute JavaScript in tab
"$CLI" interact browser eval e5 "(el) => el.textContent"        # Execute JavaScript on element
"$CLI" interact browser eval b1 "() => document.title" --world Isolated # Run in isolated execution world
"$CLI" interact browser tab-new b1 "https://example.com"        # Open new tab with URL
"$CLI" interact browser tab-close b1                            # Close tab
"$CLI" interact browser tab-select b2                           # Switch to tab
"$CLI" interact browser go-back b1                              # Navigate back
"$CLI" interact browser go-forward b1                           # Navigate forward
"$CLI" interact browser reload b1                               # Reload page
```

### Inspect

```bash
"$CLI" interact get e5 text                # Read a single attribute
"$CLI" interact get-all e5                 # Read all attributes
"$CLI" interact screenshot                 # Full desktop screenshot
"$CLI" interact screenshot w1              # Window screenshot
"$CLI" interact screenshot b2 --full-page  # Full browser tab screenshot
"$CLI" interact screenshot e5              # Element screenshot
"$CLI" interact extract-table e5           # Extract table data as markdown
"$CLI" interact highlight e5               # Box the element in a screenshot (red)
"$CLI" interact highlight e5 --color Blue --duration 5 --visualize  # Box it and draw the border on screen too
```

## Node Format

`"$CLI" snapshot` outputs one node format for top-level, window, or browser tab refs:

- **Role** -- Node type: `Window`, `BrowserTab`, `Button`, `InputBox`, `CheckBox`, `DropDown`, `List`, `TreeItem`, `TabPage`, `MenuItem`, etc.
- **"Name"** -- Accessible label in quotes (e.g., `Button "OK"`)
- **[ref=...]** -- Interaction reference: `w1` (window), `b1` (browser tab), `e5` (element)
- **[state]** -- State markers: `[selected]`, `[focused]`, `[disabled]`, `[read only]`, `[minimized]`, `[items deferred]`
- **: text** -- Inline value (e.g., `InputBox [ref=e3]: pre-filled`)
- **/attr** -- Attributes as child lines (e.g., `/url: https://...`, `/placeholder: Type here`)
- **Children** -- Nested with indentation

Top-level example (`uia snapshot`, no ref):

```
- Window "Hello World" [minimized] [ref=w1]:
  - /process: notepad.exe
  - /class: Notepad
- Window "Google Chrome" [ref=w2]:
  - /process: chrome.exe
  - BrowserTab "Example" [ref=b1] [selected]:
    - /url: https://example.com/
  - BrowserTab "HTML5 Test Page" [ref=b2] [file URL]:
    - /url: file:///C:/Pages/index.html
```

Per-window example (`uia snapshot w1`):

```
- DropDown [ref=e73]: Second
  - ListItem "-- Choose --" [ref=e405]
  - ListItem "First" [ref=e406]
  - ListItem "Second" [ref=e407] [selected]
  - ListItem "Third (disabled)" [ref=e408]
- DropDown "Region" [ref=e74] [items deferred]: East
- InputBox "Username" [ref=e5]: john_doe
  - /placeholder: Enter username
- CheckBox "Remember me" [ref=e6] [selected]
- Button "Submit" [ref=e7]
- Button "Cancel" [ref=e8] [disabled]
```

**Key rules:**

- Only nodes with `[ref=...]` are interactable; ref-less nodes are context only.
- `[disabled]` elements are not interactable -- skip.
- `[selected]` on CheckBox/RadioButton = checked; on TabPage/ListItem = active.
- `[items deferred]` on DropDown/List: items not in snapshot — may still be retrievable via `"$CLI" interact get-all <ref>` (attributes `items`, `selectedItem`, `selecteditems`).
- Output may omit values. Read via `"$CLI" interact get <ref> text` or `"$CLI" interact get-all <ref>`; table data via `"$CLI" interact extract-table <ref>`.

## Ref Lifecycle

Any w/b/e-ref from most recent snapshot is valid for `uia interact`, whichever `uia snapshot` subcommand loaded it. Each new snapshot replaces all prior refs.

**Window refs (w1, w2) and browser refs (b1, b2)** assigned by top-level `uia snapshot`. Each call invalidates prior w/b-refs.

**Element refs (e1, e2, e3...)** assigned by `uia snapshot <w/b-ref>`. Each call invalidates prior e-refs.

**Semantic refs (s1, s2, s3...)** assigned by `snapshot semantic-find <w/b-ref>`. Each call invalidates prior s-refs. Screen positions, not live elements: after layout change they may silently hit the wrong place.

b-refs target browser tabs; w-refs target windows. See Application Guides > Browsers.

**Always re-snapshot after actions that change UI state.** Clicking, selecting a tab, or typing may alter UI tree, invalidating prior e-refs.

```bash
"$CLI" snapshot w1               # Get refs
"$CLI" interact click e7         # Perform action
"$CLI" snapshot w1               # Get fresh refs -- old ones are stale
"$CLI" interact type e5 "hello"  # Use new refs
```

## Frameworks

`--framework` on `uia snapshot` controls UI tree scanning. `Default` and `Java` work everywhere; `UIA` and `AA` Windows-only, `AX` macOS-only.

Default works for most apps. On Windows, use `--framework UIA` for:
- WinUI3 apps — modern Windows apps: Windows Terminal, redesigned Notepad, Paint, Calculator, Media Player
- WPF apps — .NET desktop apps with rich UI: Visual Studio, Blend, any XAML-built app

Snapshot empty or incomplete? Try different framework.

## Input Methods

Use `--input-method` with `click`, `type`, and `hover`:

- **HardwareEvents** (default) -- Simulates real mouse/keyboard. Auto-activates window (foreground required). Typing appends to existing text; `--clear-before-mode` clears first.
- **Simulate** -- Uses element's native API directly. Works on background windows. Usually auto-clears field before typing -- skip `--clear-before-mode` by default; verify result via `interact get`, `interact get-all`, or re-inspect, retry with `--clear-before-mode` only if field has unexpected text. Recommended for Firefox (b-ref mode only), Java Swing/AWT apps, SAP GUI session windows.
- **DebuggerApi** -- Dispatches via Chromium Debugger. Recommended for Chrome/Edge. No foreground required.

Switch input method when default has no visible effect on target. Full decision guide: [input-methods-guide.md](../../references/input-methods-guide.md).

Special keys in `interact type` and `--modifiers` in `interact click`: fully supported with HardwareEvents and DebuggerApi. Simulate may support them for some applications (e.g.: Browsers (b-refs), SAP session windows) but not guaranteed -- other input methods may silently ignore them.

## Special Keys

`"$CLI" interact type` special key syntax: `[k(key)]` press and release, `[d(key)]` hold down, `[u(key)]` release.

Full key list and examples: [references/special-keys.md](references/special-keys.md).

## Common Patterns

### Select from dropdown/list

DropDown and List elements may show options as children. Use **parent's ref** and option's text:

```bash
"$CLI" interact select e73 "First"    # Select "First" using the DropDown's ref
```

Current selection: inline text after `:` or child marked `[selected]`. Re-inspect to confirm.

Parent tagged `[items deferred]`? Items absent from snapshot but may be retrievable:

```bash
"$CLI" interact get-all e74           # `items`, `selectedItem`, `selecteditems`
"$CLI" interact select e74 "North"    # assuming `items` includes "North"
```

Items missing? Click element to expand, retry -- some controls load children only when opened.

### Toggle a checkbox

```bash
"$CLI" interact click e6                # Click to toggle; re-inspect to verify
"$CLI" snapshot w1                      # [selected] = checked, no [selected] = unchecked
```

### Navigate a menu

```bash
"$CLI" interact click e13               # Click "File" menu item
"$CLI" snapshot w1                      # Inspect to see submenu items
"$CLI" interact click e42               # Click the desired submenu item
```

### Expand a tree node

```bash
"$CLI" interact click e108 --type Double   # Double-click to expand tree item
"$CLI" snapshot w1                         # Inspect to see children
```

### Switch non-browser tabs

```bash
"$CLI" interact click e115              # Click the tab you want
"$CLI" snapshot w1                      # Verify tab is now [selected] and content changed
```

### Fill a form

```bash
"$CLI" snapshot                         # Find the window
"$CLI" snapshot w1
"$CLI" interact type e5 "John Doe" --clear-before-mode SingleLine
"$CLI" interact type e6 "john@example.com" --clear-before-mode SingleLine
"$CLI" interact select e8 "USA"
"$CLI" interact click e10               # Submit button
"$CLI" snapshot w1                      # Verify result
```

### Extract table data

```bash
"$CLI" interact extract-table e5    # Recognizes tables, data grids, and other tabular structures
                                    # Prefer over other scraping methods; fall back only if it fails
```

### Find an element by description

When no usable tree exists or target is easiest to describe visually. Up to 3 matches, no specific order:

```bash
"$CLI" snapshot semantic-find w1 "the username field"       # Read the annotated screenshot; pick the match by its outline colour
"$CLI" snapshot semantic-find w1 "the username field" --exclude-color yellow  # Rerun if an outline colour is hard to spot (max 3)
"$CLI" interact type s1 "john_doe" --click-before-mode Single  # s-refs are not focused before typing
```

Rerun `semantic-find` after any action that changes layout.

## Error Recovery

**Possible misconfiguration:** App appears misconfigured for automation (e.g., missing extension, scripting disabled)? Tell user, point to relevant Application Guide.

**Empty/partial snapshot** -- Wrong framework or window not ready:

```bash
"$CLI" interact window foreground w1         # Ensure window is visible
"$CLI" interact window maximize w1           # Maximize to see all elements
"$CLI" snapshot w1 --framework UIA           # Try different framework
```

Tree still empty/unusable after framework changes? Find element by description:

```bash
"$CLI" snapshot semantic-find w1 "the username field"       # Visual fallback when no usable tree exists
"$CLI" interact type s1 "john_doe" --click-before-mode Single  # Act on a match; s-refs are not focused before typing
```

No matches and too many matches both exit non-zero with hint: refine description (relative position, label text, surrounding elements).

**Dropdown/list options not visible**

Control tagged `[items deferred]`? Read items directly:

```bash
"$CLI" interact get-all e10                  # see `items`, `selectedItem`, `selecteditems`
```

Fallback -- click to expand, then retry:

```bash
"$CLI" interact click e10                    # Click the DropDown to expand it
"$CLI" interact get-all e10                  # Retry reading the `items`

# Still no items? Snapshot again after the click:
"$CLI" snapshot w1                           # Re-inspect to see the options
```

**Interaction has no visible effect:**

```bash
"$CLI" interact get-all e5                        # Check element attributes for clues
"$CLI" interact click e5 --input-method Simulate  # Try a different input method
```

**Click lands on wrong spot** -- Click feedback shows screen coordinates. Screenshot; check coordinates fall visually inside intended element. If not:

```bash
"$CLI" interact highlight e5                                     # Box the element in a screenshot to see its bounds
"$CLI" interact screenshot e5                                    # Screenshot the element
"$CLI" interact get e5 position                                  # Get the reported position of the element

# Try
"$CLI" interact click e5 --origin TopLeft --offset-x 5 --offset-y 5 # Use a different origin point instead of center
"$CLI" interact click e5 --input-method Simulate                 # Might ignore the coordinates
"$CLI" snapshot w1 --framework UIA                               # Re-inspect with different framework (bounds may differ)
"$CLI" interact click e10                                        # Click a child element that may be more reliably located
```

## Application Guides

### Browsers

**Prerequisites:** UiPath browser extensions required for b-refs to appear in `uia snapshot`. Install: https://docs.uipath.com/studio/standalone/latest/user-guide/about-extensions

**Targeting:** Use b-refs (not w-refs) for web content -- b-refs provide DOM tree. Browser tabs nest under parent window in `uia snapshot` (see Node Format).

```bash
"$CLI" snapshot b1   # Preferred: inspect the browser tab
```

Active tab marked `[selected]`. Tab states signaling access limitations:

- `[discarded]` -- Suspended by browser to save memory. CLI attempts auto-restore on first interaction.
- `[internal page]` -- Browser internal pages (new tab, settings, etc.). Cannot inspect or interact with page elements. Browser tab commands (navigate, reload, close, etc.) may still work. Element-level interaction: use parent w-ref as desktop application.
- `[extension store]` -- Web store pages. Same limitations as internal pages.
- `[file URL]` -- Local file URLs. Same limitations unless "Allow access to file URLs" enabled for UiPath extension.

**Browser windows without tabs:** Browser window with no BrowserTab children: `uia snapshot` adds explanatory state to parent window:

- `[extension missing]` -- UiPath browser extension unavailable: not installed or disabled.
- `[incognito]` -- Extension installed but cannot access this window (likely incognito/private). Allow extension in incognito mode via browser's extension settings.

Both cases: w-ref still controls browser as desktop application.

After page navigation, re-inspect for fresh refs.

**Input methods:**
- Chromium (Chrome, Edge): Use `--input-method DebuggerApi`. Fallback: Simulate, then HardwareEvents.
- Firefox: Use `--input-method Simulate` -- DebuggerApi not supported. Fallback: HardwareEvents.

### SAP WinGUI

Read [SAP Automation Guide](../../references/sap-guide.md) first for general SAP automation concepts

**Prerequisites:** SAP GUI Scripting enabled (server and client). Setup guide: https://docs.uipath.com/activities/other/latest/ui-automation/sap-wingui-configuration-steps

**Logging in:** use dedicated SAP verbs, not hand-driving the SAP Logon pad. `sap logon` opens a connection by name, prints the opened session's `sapSysSessionId`. Then `uia snapshot`, pick the window whose `sapSysSessionId` matches — that `wN` goes to `sap login`.

```bash
"$CLI" interact sap logon "MyConnection"  # opens the connection; prints "sapSysSessionId=<id>"
"$CLI" snapshot                           # find the window whose sapSysSessionId matches → e.g. w1

# password via named env var, set out-of-band in separate step:
export SAP_PASSWORD="$(cat secret.txt)"
"$CLI" interact sap login w1 --user <USER> --client 100 --language EN --password-env SAP_PASSWORD
# or pipe secret from file via stdin:
cat secret.txt | "$CLI" interact sap login w1 --user <USER> --client 100 --language EN --password-stdin
```

**Transaction code navigation:**

```bash
"$CLI" interact sap call-transaction w1 VA01          # Navigate to transaction VA01 (current session; add --new-session for a new one)
"$CLI" interact sap read-statusbar w1                 # Confirm the outcome (prints "Status bar [<type>] <number>: <text>")
"$CLI" snapshot w1                                    # New transaction screen loaded with new refs
```

**Clicking a control:**

```bash
"$CLI" interact click e7 --input-method Simulate                    # Click a SAP control (e.g. Execute) via the scripting API
```

**Reading table data:** Snapshots may show only rows currently in view. Maximize first for more rows in snapshot:

```bash
"$CLI" interact extract-table e15    # Extracts entire table, not just visible rows
```

To interact with rows not in view, scroll and re-inspect:

```bash
"$CLI" interact wheel e15 --direction Down --units 5  # Scroll down
"$CLI" snapshot w1                                    # Fresh refs for newly rendered rows
```

**Status bar:** SAP confirms operations via status bar (see guide for message-type meanings). After an action:

```bash
"$CLI" interact get e99 text  # Read status bar
```
