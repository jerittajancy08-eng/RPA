# SAP Automation Guide

Cross-cutting guidance for automating SAP GUI for desktop (WinGUI and Java) and SAP web (Fiori/UI5, Web Dynpro, Ariba).

## Input mode: use Simulate for SAP

Simulate is the recommended input mode for SAP (desktop and web). It uses the SAP GUI Scripting when available which makes actions synchronous (e.g. a click waits until the event is processed, not just delivered). For web it uses the Web Framework API, which is more reliable than using hardware events.
Set it once at the project level — **Project Settings → UI Automation Modern → Targeting methods → SAP → Input mode → Simulate** — or per activity via `InteractionMode` (see [NInteractionMode](../activities/common/NInteractionMode.md)).

Use the Default UI framework to utilize SAP GUI Scripting. Switch to UIA or AA if elements don't surface (e.g. on pages with embedded HTML).

Some control-action pairs won't support Simulate and return an explicit error. Use HardwareEvents for those.
- e.g. **Keyboard shortcuts / function keys** — SAP hotkeys are not dispatched by Simulate. Send them with **HardwareEvents** using `NKeyboardShortcuts` / `uip rpa uia interact type <ref> "[k(...)]" --input-method HardwareEvents`.

## SAP GUI (desktop)

**Prerequisite:** SAP GUI Scripting must be enabled on both server and client.
- SAP WinGUI setup: https://docs.uipath.com/activities/other/latest/ui-automation/sap-wingui-configuration-steps
- SAP GUI for Java setup: https://docs.uipath.com/activities/other/latest/ui-automation/gui-for-java-configuration-steps

SAP GUI for Java supports the same activities, verbs and elements as SAP WinGUI, with the exception of the Logon Pad (the window, NSapLogon and `interact sap logon`).

- **The status bar is the confirmation channel.** SAP reports operation outcomes — success `S`, error `E`, warning `W`, info `I`, abort `A` — on the status bar. Read it after an action to confirm success and surface errors; the read is also a synchronous round-trip, so it doubles as a wait point.
- **Tables expose only visible rows.** A snapshot/scrape sees only the rows in view. Use **Extract Table** for the full dataset.
- **Use longer timeouts** for SAP operations than for typical desktop/web apps.
- **Never pass the SAP login password as a command-line value** (it leaks into argv, shell history, and logs). With `interact sap login`, pipe it via `--password-stdin` — e.g. `Get-Content secret.txt | uip rpa uia interact sap login <w-ref> --user <name> --client <nnn> --language <xx> --password-stdin` — or name an environment variable with `--password-env <VAR>`.
- **Ensure a clean state while authoring.** A clean state reduces unexpected errors and makes authoring **faster**. Never skip it.
  - **WinGUI — workflows:** use `NSAPLogon` with `OpenMode="Always"`; `CloseMode="Never"` while authoring so that end state stays visible, then close the session before each new run. Don't try `OpenMode="IfNotOpen"` or `OpenMode="Never"` since SapLogon always opens a new session window. Switch to `CloseMode="Always"` once the test is stable.
  - **Windows — exploration:** use `interact sap logon` to open a fresh session, close every window whose `sapSysSessionId` differs from it, then use `interact sap login`.
  - **Java — workflows:** Launch a new connection manager only if not already open ([SAP GUI for Java](#sap-gui-for-java));
  - **Java — exploration:** open a fresh session from the connection manager, close every session window whose `sapSysSessionId` differs, then use `interact sap login`.

## Window types

A SAP automation touches up to three window kinds. Identify them by `cls`/`app`/`title` — these differ between the two desktop variants:

| Window | SAP GUI for Windows (WinGUI) | SAP GUI for Java |
|---|---|---|
| **Logon Pad / connection manager** | SAP Logon Pad (`saplogon.exe`). | "SAP GUI for Java" connection manager (`javaw.exe`, launched via `guilogon`). `cls='SunAwtFrame'` |
| **Session** | `<wnd app='saplogon.exe' cls='SAP_FRONTEND_SESSION' />` | `<wnd app='java*.exe' cls='SunAwtFrame' title='S4H (n) (client)' />` |
| **Popup / modal dialog** | own OS window, same `sapSysSessionId` as the session, `cls='#32770'`, title = dialog caption | own OS window, same `sapSysSessionId`, `cls='SunAwtDialog'`, title = dialog caption |

## SAP GUI for Java

There is no Logon Pad (no `saplogon.exe`, no **SAP Logon** activity / `interact sap logon`) — open sessions through the connection manager instead:

- **Launch the connection manager** by running the `guilogon` launcher (in the SAP GUI for Java install's `bin` directory — `guilogon.bat` on Windows). It opens a `javaw.exe` / `SunAwtFrame` window titled "SAP GUI for Java".
- **Launch it once — avoid duplicate instances.** Unlike WinGUI's SAP Logon (a single Logon Pad every session reuses), each `guilogon` run starts a **new** `javaw` connection-manager process. Before launching, check whether a "SAP GUI for Java" window / `javaw` process is already open and reuse it — extra instances leave orphan managers and can open the session under an instance you aren't driving.
- **Open a session by double-clicking the connection.** The configured connections are Java list items — e.g. `<java cls='SytemListTile' role='list' /><java name='<YourConnection> - *' role='label' />` (capture your own; the label carries the connection name). Double-click the entry for your connection; a session window (`SunAwtFrame`, title `S4H (n) (client)`) opens showing the login screen.
- **Scope the SAP Login card to the session, not the connection manager.** `NSAPLogin` operates on **its card's window**, and the login screen lives in the newly-opened session window — so the card hosting `NSAPLogin` must be scoped to the session (`title='S4H*'`), not the connection-manager window.

Don't use `--framework java`. The default framework surfaces elements as SAP scripting controls that provide a better API.


## Discovering control items (`interact get … items`)

Some controls omit inner entries from snapshot nodes. Read `uip rpa uia interact get <ref> items` (or `get-all`); pass result as activity `Item`:

- **Toolbar buttons** — `get <toolbarRef> items` → the button identifiers. Press one ad-hoc with `interact select <toolbarRef> "<button>"`, or pass it to **Click Toolbar Button**.
- **Dropdown / combobox entries** — `get <comboRef> items` → the selectable entries. Pick one ad-hoc with `interact select <comboRef> "<entry>"`, or pass it to **Select Item**.
- **Menu items** — `get <menuBarRef> items` (the menu-bar element) returns the full slash-separated menu paths. Navigate ad-hoc with `interact select <menuBarRef> "<menu/path>"`, or pass a path to the **Select Menu Item** activity.
- **Tree node paths** — `get <treeRef> items` → each node's slash-separated path (its `relpath`, e.g. `Root/Child/Leaf`), for **Expand Tree**'s `Item`. Only **materialized** nodes appear — a lazy child whose parent has never been expanded is absent until the parent is expanded (see below).

## Navigating a SAP tree (expand / collapse)

Twisty is not separate clickable element; node row toggles. Choose by path knowledge:

- **Known path → Expand Tree.** Set activity `Item`, or run `interact sap expand-tree <treeRef> "<path>"`. Use root-relative slash-separated `relpath` from `get <treeRef> items`.
  - `A/B/C` — expands ancestors up to `C`: `C` becomes visible but stays **collapsed** (children not revealed).
  - `A/B/C/` — trailing `/` also expands `C` itself, revealing **its** children.
- **Discovering → click.** Single Simulate click on node row toggles expand/collapse.

## Target configuration

- NSAPLogon, NSAPLogin, NSAPCallTransaction, and NSAPReadStatusBar do not require target configuration as they address SAP through the connection and session, not a captured selector.
- **Prefer `[sap]`-tagged nodes.** SAP web framework elements carry richer, more stable attributes than generic HTML, so they produce more reliable selectors.
- **SAP session window:** WinGUI `<wnd app='saplogon.exe' cls='SAP_FRONTEND_SESSION' />`; Java `<wnd app='java*.exe' cls='SunAwtFrame' title='S4H*' />` (see [Window types](#window-types)).
- **Modal dialogs are their own window.** A SAP popup is a separate OS window with the same `sapSysSessionId` as the session, title = the dialog caption; `cls='#32770'` on WinGUI, `cls='SunAwtDialog'` on Java.

## Other Gotchas

- **SAP UI5 autocomplete re-renders.** When selecting from a combobox/autocomplete dropdown, the list can re-render briefly after typing; give the click on the dropdown item a small `DelayBefore` (and use Simulate) so it doesn't hit a stale node.
- **Use [Select Menu Item](../activities/SAPSelectMenuItem.md)** for menu navigation. A direct menu-bar selector, like `<sap id='mbar/menu[a]/menu[b]'/>`, is positional and will shift between screens (the System menu may be `menu[4]` on SAP Easy Access but `menu[3]` elsewhere).
- **Context menus are a native `#32768` popup.** Right-click the target with **HardwareEvents** to open it; the menu appears as a separate `#32768` window. Read and click its `MenuItem`s with `--framework UIA`.
- **SAP tables are scripting-backed.** Accessing a table row scrolls the table to bring it into view.

## Troubleshooting

Trigger -> Actions:

- After each step -> read the status bar. For complex issues also capture and read a screenshot.
- Element not found in a snapshot -> 1. use a screenshot to validate that the element exists. 2. Widen your grep expression. 3. Capture with `--framework UIA`. 4. Look inside other SAP windows.
- Actions throw errors or don't have an effect -> 1. read the status bar. 2. check a screenshot. 3. use HardwareEvents.
- interact works but `rpa run` doesn't -> 1. Close leftover SAP windows. 2. Add logging and screenshots after each activity step. Interact uses the same backend as rpa run so they should have similar capabilities.
