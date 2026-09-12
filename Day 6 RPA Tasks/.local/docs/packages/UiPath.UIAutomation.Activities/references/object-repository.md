# Object Repository

Central store for reusable UI Automation application, screen, and element descriptors.

**IMPORTANT: Use only CLI: `uip rpa uia object-repository` (e.g., `uip rpa uia object-repository get-screens`). Full reference: [cli-reference.md](cli-reference.md).**

## Key Concepts

### Application

**Application** — top-level screen container representing automated desktop app or browser page.

### Screen

**Screen** — contains **TargetApp** for scope activities such as `Use Application/Browser`; created from window-selector definition files.

### Element

**Element** — screen UI element containing **TargetAnchorable** for targeted activities (e.g., `Click`, `Type Into`). Created from definition file; always belongs to parent screen.

## CLI Commands

All options, flags, examples: [cli-reference.md](cli-reference.md). Four groups:

### Applications

| Command | Description |
|---------|-------------|
| `create-app` | Creates application entry in Object Repository. |
| `get-apps` | Gets all applications from Object Repository. |
| `get-app` | Gets application by reference ID. |
| `delete-app` | Deletes application entry. |

### Screens

| Command | Description |
|---------|-------------|
| `create-screen` | Creates screen entry from definition file. |
| `get-screens` | Gets screens, optionally filtered by definition file or application. |
| `get-screen` | Gets screen by reference ID. |
| `get-screen-xaml` | Gets XAML representation of screen. |
| `delete-screen` | Deletes screen entry. |

### Elements

| Command | Description |
|---------|-------------|
| `create-elements` | Creates multiple element entries from definition files in single batch. |
| `get-elements` | Gets elements for given screen. |
| `get-element` | Gets element by reference ID. |
| `get-elements-xaml` | Gets XAML representation of multiple elements in single batch. |
| `delete-element` | Deletes element entry. |

### Linking

| Command | Description |
|---------|-------------|
| `link-elements` | Links Object Repository elements to activities in XAML workflow. |
| `link-screen` | Links Object Repository screen to activity in XAML workflow. |
