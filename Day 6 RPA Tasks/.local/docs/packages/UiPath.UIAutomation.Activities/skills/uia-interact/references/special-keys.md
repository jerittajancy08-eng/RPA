# Special Keys

Key names, modifiers, combination patterns for `uip rpa uia interact type`.

## Syntax

| Syntax | Meaning |
|--------|---------|
| `[k(key)]` | Press and release |
| `[d(key)]` | Hold down |
| `[u(key)]` | Release |

- Supported with **HardwareEvents** (default) and **DebuggerApi**; other input methods: support varies by target application.
- Escape literal `[` as `[[`.
- Mix text and special keys: `"Hello[k(enter)]World"` types "Hello", presses Enter, types "World".

## Key Reference

| Category | Keys |
|----------|------|
| **Modifiers** | `ctrl`, `alt`, `shift` |
| **Navigation** | `left`, `right`, `up`, `down`, `home`, `end`, `pgup`, `pgdn`, `tab` |
| **Editing** | `enter`, `back` (Backspace), `del`, `ins`, `esc` |
| **Function** | `f1` through `f12` |
| **Toggle** | `caps`, `num` |
| **Windows** | `lwin`, `rwin` |
| **macOS** | `command` (⌘), `option` (⌥) |

Left/right modifier variants exist (`lctrl`, `rctrl`, `lalt`, `ralt`, `lshift`, `rshift`); `ctrl`/`alt`/`shift` suffice for most automation.

## Common Names

Use UiPath key names, not full names:

| Key | Name | NOT |
|-----|------|-----|
| Backspace | `back` | `backspace` |
| Delete | `del` | `delete` |
| Escape | `esc` | `escape` |
| Page Up | `pgup` | `pageup` |
| Page Down | `pgdn` | `pagedown` |
| Insert | `ins` | `insert` |

## Examples

### Single keys

| Action | Argument |
|--------|----------|
| Press Enter | `[k(enter)]` |
| Press Tab | `[k(tab)]` |
| Press Esc | `[k(esc)]` |
| Press Backspace | `[k(back)]` |
| Press F5 (refresh) | `[k(f5)]` |

### Single-modifier shortcuts

| Action | Argument |
|--------|----------|
| Ctrl+A (select all) | `[d(ctrl)]a[u(ctrl)]` |
| Ctrl+C (copy) | `[d(ctrl)]c[u(ctrl)]` |
| Ctrl+V (paste) | `[d(ctrl)]v[u(ctrl)]` |
| Ctrl+Z (undo) | `[d(ctrl)]z[u(ctrl)]` |
| Ctrl+S (save) | `[d(ctrl)]s[u(ctrl)]` |
| Shift+Tab (previous field) | `[d(shift)k(tab)u(shift)]` |
| Shift+Left x2 (select 2 chars) | `[d(shift)k(left)k(left)u(shift)]` |

### Multi-modifier shortcuts

Multiple modifiers can share single block; release in reverse order.

| Action | Argument |
|--------|----------|
| Ctrl+Shift+T (reopen tab) | `[d(ctrl)d(shift)]t[u(shift)u(ctrl)]` |
| Ctrl+Shift+J (DevTools console) | `[d(ctrl)d(shift)]j[u(shift)u(ctrl)]` |
| Ctrl+Alt+Del | `[d(ctrl)d(alt)k(del)u(alt)u(ctrl)]` |
| Alt+F4 (close window) | `[d(alt)k(f4)u(alt)]` |

### Windows-key shortcuts

| Action | Argument |
|--------|----------|
| Win+R (Run dialog) | `[d(lwin)]r[u(lwin)]` |
| Win+D (show desktop) | `[d(lwin)]d[u(lwin)]` |
| Win+L (lock) | `[d(lwin)]l[u(lwin)]` |

### macOS shortcuts

macOS primary shortcut modifier is **Command (⌘)**, not Control -- `Ctrl+C` / `Ctrl+V` / `Ctrl+A` / `Ctrl+S` equivalents are `⌘C` / `⌘V` / `⌘A` / `⌘S`. `interact type` does not auto-convert Windows modifiers -- write macOS token explicitly: `command` for ⌘, `option` for ⌥. `ctrl` (⌃ Control), `shift`, all other keys: same tokens as Windows.

| Action | Argument |
|--------|----------|
| ⌘A (select all) | `[d(command)]a[u(command)]` |
| ⌘C (copy) | `[d(command)]c[u(command)]` |
| ⌘V (paste) | `[d(command)]v[u(command)]` |
| ⌘S (save) | `[d(command)]s[u(command)]` |
| Clear field (⌘A then Delete) | `[d(command)]a[u(command)][k(del)]` |
| ⌘⇧T (reopen tab) | `[d(command)d(shift)]t[u(shift)u(command)]` |
| Move one word right (⌥→) | `[d(option)k(right)u(option)]` |

### Mixed text and keys

| Action | Argument |
|--------|----------|
| Type Hello, Enter, World | `Hello[k(enter)]World` |
| Fill three fields, tabbing between | `value1[k(tab)]value2[k(tab)]value3` |
| Select all then delete (clear field) | `[d(ctrl)]a[u(ctrl)][k(del)]` |
| Open Find, search, press Enter | `[d(ctrl)]f[u(ctrl)]todo[k(enter)]` |
| Type a literal `[k(enter)]` | `[[k(enter)]` |

### CLI invocation

```
"$CLI" interact type e3 "[k(enter)]" # press Enter
"$CLI" interact type e3 "[d(ctrl)]a[u(ctrl)]" # Ctrl+A (select all)
"$CLI" interact type e3 "[d(alt)k(f4)u(alt)]" # Alt+F4 (close window)
"$CLI" interact type e3 "[d(shift)k(left)k(left)u(shift)]" # Shift+Left x2 (select 2 chars)
"$CLI" interact type e3 "Hello[k(enter)]World" # type Hello, press Enter, type World
"$CLI" interact type e3 "[[k(enter)]" # types literal "[k(enter)]"
```
