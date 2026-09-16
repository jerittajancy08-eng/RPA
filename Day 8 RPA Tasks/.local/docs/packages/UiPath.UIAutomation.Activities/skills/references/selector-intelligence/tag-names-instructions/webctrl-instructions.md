## Webctrl:

WebCtrl tags automate browser UI elements. Attributes generate dynamically from HTML tag attributes — distinguish value-adding from unreliable.

Subset of predefined attributes, by reliability:

a) very reliable: tag, aria-label, aria-labeledby, aria-role, shadowhostid, aria-describedby
b) alright: id, parentid, visibleinnertext, innertext, name, parentname, isleaf, type, aaname
c) table-specific: tableCol, tableRow, colName, rowName
d) last resort, only to make selector unique: action, alt, href, src, tabindex, placeholder, uipath-html-title, css-selector, class, parentclass, aria-pressed, data-cy
e) last resort — framework-generated, sensitive to framework updates; only if nothing from a-d uniquely identifies target: ng-reflect-*, ng-version, _ngcontent-*, _nghost-*, data-reactid, __reactProps$*, v-*, data-v-*

## CRITICAL - IMMEDIATE DISQUALIFICATION FOR WEBCTRL:

### shadowhostid rule:
Tags matching elements inside shadow DOM **must** include `shadowhostid`, even with empty-string value.
Engine searches entire HTML DOM including all shadow roots, so **intermediate ancestor tags are free to add or remove**.
Only requirement: kept or added tags whose node has `shadowhostid` in attribute map must include it with exact value.

### frame/iframe rule:
Do not remove tags with tag 'frame' or 'iframe' — critical for identifying elements inside frames. They must keep pointing to same ancestor nodes after modifications.

## ADVICE FOR WEBCTRL SELECTORS:

### Dynamic Content Handling
Attributes like `aaname`, `visibleinnertext`, `innertext` often contain dynamic data (dates, numbers, user names, amounts) changing between contexts; wildcard to preserve stable parts:

**Unreliable selector:**
`<webctrl tag='LABEL' aaname='$72.41 (1.12%)' id='201__25061_totalRaise_field' visibleinnertext='$72.41 (1.12%)' parentid='201__field_editor_layout' />`

Breaks: dollar amount and percentage change, exact matches fail.

**Improved with wildcards:**
`<webctrl tag='LABEL' aria-labelledby='*totalRaise*' id='*totalRaise*' aaname='$*%*' />`

Works: keeps semantic ""totalRaise"" pattern, wildcards dynamic values, removes redundant duplicate text attributes.

### Text-Aggregating Attributes as Reliable Pillars

Previous section wildcards target's *own* volatile text; here *container's* aggregated text — often stable — uniquely identifies row/card.

`visibleinnertext` and `innertext` on `webctrl` tags concatenate text of element AND all descendants. On containers (rows, cards, list items, sections) they're reliable selector pillars: single wildcarded value pins whole subtree to known row/card identity.

**When to use:**
- Target inside repeating structure (table row, card grid, list item) where structural attributes alone don't differentiate from siblings.
- Nearby text (row label, card title, ticker symbol) uniquely identifies correct instance.
- Row/column index is fragile (sorting, pagination, virtualization).

**How to apply:**
- Put text constraint on CONTAINER (TR, DIV card, LI), not target.
- Always wildcard: `visibleinnertext='*ETH*'` — never paste full row text.
- Combine with stable structural attribute on same tag when possible (`parentid`, `data-testid`, `aria-role`) so selector survives text changes.
- On tables, pair row pinned by `visibleinnertext='*<rowKey>*'` with cell `colName='<column>'` — more robust than row index.

**Example — stock table, reading ETH price cell:**
```
<html app='msedge.exe' title='Cryptocurrency Prices, Live Charts, Market Cap, News - Crypto.com EEA' />
<webctrl tag='TABLE' />
<webctrl parentid='cdc-market-body' tag='TR' visibleinnertext='*ETH*' />
<webctrl tag='TD' colName='Price' />
<webctrl tag='P' />
```
Target `P` has no stable identifier; ancestor `TR` with `visibleinnertext='*ETH*'` plus `colName='Price'` cell make selector unambiguous without filtering target by own dynamic price text.

### Framework Directives Are Last Resort

Use framework-injected attributes only as last resort, even when value looks semantic (e.g., `ng-reflect-name='labelEmail'`, `ng-reflect-dictionary-value='First Name'`).

Framework-generated attributes:
- Angular:  `ng-reflect-*`, `ng-version`, `_ngcontent-*`, `_nghost-*`, `ng-*`
- React:    `data-reactid`, `__reactProps$*`, `__reactFiber$*`
- Vue:      `v-*`, `data-v-*`

**Why last resort:** they reflect internal framework state and @Input() property names, sensitive to every framework update — app can look identical to user while all these attributes shift.

**Preferred alternatives, in priority order:**
1. Ancestor/container text attribute: `visibleinnertext`, `innertext`, `aaname` on wrapper (DIV, LABEL, custom component tag) — visible label text survives framework upgrades.
2. ARIA attributes on or near target: `aria-label`, `aria-labelledby`, `aria-describedby`.
3. Author-controlled `data-*` identifiers (e.g., `data-testid='first-name-input'`). `data-reactid` / `data-v-*` do NOT count — framework-generated.

Fall back to framework directives only when none above uniquely identifies target; never rank above semantic text alternative in scoring.

### Auto-Generated Attribute Values
IDs and classes often generate dynamically. Distinguish semantic value vs purely random:

**Unreliable (fully auto-generated, no semantic meaning):**
- `id='89763184740'` → Avoid completely
- `id='container-45f6g7h8'` → Random suffix, unreliable
- `class='css-1wq41pf'` → CSS-in-JS generated class (Emotion/styled-components)
- `class='__className_6efda9'` → Next.js build hash class

**Partially reliable (contains semantic parts):**
- `id='129__25061_startDate_field'` → `id='*startDate*'` extracts semantic part
- `parentid='201__field_editor_layout'` → `parentid='*editor*'` or avoid

**Reliable (semantic, human-readable):**
- `id='search-form'` → Keep as-is
- `parentid='main-content'` → Keep as-is

### Hash-Based Tag Names
Some frameworks (e.g., ServiceNow) generate custom element tag names embedding hex identifier, e.g. `MACROPONENT-F51912F4C700201072B211D4D8C26010` or `SCREEN-ACTION-TRANSFORMER-77B1DA1E6F22111089060168E25B36FD`. Deployment-specific; may change across environments.
- **Prefer skipping these tags entirely** if stable descendant with semantic tag name (e.g., `SN-PAR-SCHEDULED-EXPORT`, `NOW-BUTTON`, `FILE-TO-EXPORT`) exists further down chain.
- If one must stay (e.g., only shadow DOM entry point), wildcard the hash: `tag='MACROPONENT-*'`.

### Component Path IDs (id, data-testid, parentid)

`id`, `data-testid`, and `parentid` values with 3+ dot-separated segments often encode component tree hierarchy, not stable identifiers. Renaming any intermediate component breaks them — same fragility as deep `css-selector` paths, hidden in one attribute. If value has zero semantic content (purely numeric like `id='89763184740'`), avoid attribute entirely.

**Detect:** Split on `.` or major boundaries. If most segments are structural (`ui`, `view`, `content`, `layout`, `components`, `container`, `wrapper`, `cell`, `body`, `header`, `lib`) not semantic (`auth`, `cart`, `submit-btn`, `search-field`), it's a component path.

**Fix:** Wildcard to last 1-2 semantic segments:
- `data-testid='polaris-ideas.ui.view-content.idea-list.header.cell.trigger'` → `data-testid='*cell.trigger'`
- `id='polaris.ideas.ui.view-controls.filter-button'` → `id='*filter-button'`
- `parentid='app-layout-sidebar-navigation-menu-container'` → `parentid='*navigation-menu*'` or prefer `aria-role`
- `id='mat-form-field-0-input-container'` → `id='*form-field*input*'`

**Keep as-is:** Short semantic IDs (`data-testid='filter-button'`, `id='search-form'`) and intentional hierarchical IDs where most segments are semantic (`data-testid='auth.login-form.submit-btn'`).

**Hash suffixes:** `data-component-selector='header-button-eE39'` → `data-component-selector='header-button*'`

### CSS Classes — Semantic Core Extraction

Framework class names mix semantic developer-authored names with volatile build hashes, prefixes, and utility tokens. Don't detect which classes are hashes — extract semantic core, wildcard rest.

**Algorithm:**
1. Split class value into segments on `__` and `_` separators
2. Score segments: longer with hyphens (kebab-case like `search-input`) or multi-word (camelCase like `cartTotal`) score high; short mixed-case alphanumeric (3-8 chars like `kR4mZ`, `TnB8j`, `vYe3p`) likely build hashes, score low
3. Select highest-scoring segment as semantic core
4. Emit `class='*semantic-core*'` with wildcards absorbing volatile prefix/suffix/hash

**Examples:**
- `class='sc__components_search-input__kR4mZ'` → segments: [`sc`, `components`, `search-input`, `kR4mZ`] → best: `search-input` (kebab-case, 12 chars) → `class='*search-input*'`
- `class='ui__layout_sidebar-nav-item__xPq2W'` → best: `sidebar-nav-item` → `class='*sidebar-nav-item*'`
- `class='mod__CartSummary_cartTotal__TnB8j'` → best: `cartTotal` (camelCase) → `class='*cartTotal*'`
- `class='page__DataGrid_columnHeader__vYe3p'` → best: `columnHeader` → `class='*columnHeader*'`

**Reject entirely (no semantic content):**
- Emotion/styled-components: `css-[alphanumeric]` (e.g., `css-1wq41pf`)
- Next.js build hashes: `__[word]_[hex]` (e.g., `__className_6efda9`)
- Utility-only classes: layout tokens like `m-2`, `flex`, `w-100`, `hidden`

**Multiple classes in one attribute:** Keep only best class's semantic part:
- `class='k-dialog-wrapper dialog-xl dialog-voucher'` → `class='*dialog-voucher*'`
- `parentclass='btn btn-primary btn-lg'` → `parentclass='*btn-primary*'`

Apply same extraction to `parentclass`.

### What to Avoid Completely
- Utility/layout-only CSS classes: `m-2`, `flex`, `w-100`
- DIV tags without semantic attributes(aria-*, role, meaningful id/class)

### What to Prefer
- Aria attributes highly reliable: `aria-label`, `aria-labelledby`, `aria-role`, `aria-describedby`
- Semantic HTML tags: prefer specific tags(BUTTON, INPUT) over generic(DIV, SPAN)

### SAP Web Frameworks (Fiori, Web GUI, Ariba)

SAP framework-specific attributes on `webctrl` tags: prefer elements carrying them — more reliable than generic HTML attributes on that page.

#### SAP Fiori (UI5)
- **Very reliable:** `ui5-label`, `ui5-tooltip`, `ui5-role`, `ui5-type`, `ui5-view-local-id`
- **Alright:** `ui5-class`
- **For tables:** `ui5-tableCol`, `ui5-tableRow`, `ui5-isEmpty`, `ui5-colLabel`
- **Inside trees:** `ui5-path`

Some UI5 control types expose additional reliable attributes per `ui5-class` value; prefer them:
- Breadcrumbs: `currentLocationText`, `separatorStyle`
- DeltaMicroChart: `title1`, `title2`
- CustomListItem: `highlightText`
- FeedContent / NewsContent: `contentText`
- FeedInput: `buttonTooltip`
- FeedListItem: `sender`
- GenericTile: `header`, `imageDescription`
- GroupHeaderListItem: `count`
- Image: `alt`
- ListBase / Tree: `headerText`, `footerText`, `mode`
- NotificationListBase: `authorName`, `authorPicture`
- ObjectHeader: `intro`, `number`, `numberUnit`
- ObjectNumber: `number`, `numberUnit`, `unit`
- Panel: `headerText`, `accessibleRole`
- QuickViewGroup: `heading`
- RadioButton: `groupName`
- UploadCollectionItem: `documentId`, `url`

#### SAP Web GUI
- **Very reliable:** `sapweb-id` (GUI scripting ID), `sapweb-type` (GUI scripting type) — strongly prefer `sapweb-id` for most elements
- **Avoid:** `sapweb-lsid` — LightSpeed control ID, unreliable
- **Alright:** `sapweb-lsclass` (LightSpeed control class), `sapweb-text` (text from controls like TextView), `sapweb-itemid` (item ID inside trees)
- **Session context:** `sapweb-ses-screen`, `sapweb-ses-transaction`, `sapweb-ses-client`, `sapweb-ses-user`, `sapweb-ses-program`
- **For tables:** `sapweb-tablerow`, `sapweb-tablecol`, `sapweb-coltooltip`, `sapweb-colname` — for table cells prefer these over `sapweb-id`
- **Inside trees:** `sapweb-path`

#### SAP Ariba
- **Very reliable:** `aw-name`, `aw-label`, `aw-type`
- **Avoid:** `id` — unreliable hash changing across sessions in Ariba pages
- **For tables:** `aw-tablerow`, `aw-tabledetailrow`, `aw-tablerowtype`, `aw-tablecol`, `aw-collabel`
