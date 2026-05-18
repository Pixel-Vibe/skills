# Layout shell

Every Mekari product screen sits inside this shell. Read this once when starting a new screen, then move to the pattern reference for the page content.

## Color zones (the most-confused part)

Before anything else: Mekari uses **surface** (slate `#F1F5F9`) and **stage** (white) in a specific alternating pattern. The principle is _surface frames, stage holds content_ — but the zones aren't intuitive, so here's the explicit map:

| Zone                                                                                     | Background                                                                   |
| ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Top bar                                                                                  | `Color/Background/stage` (white)                                             |
| Nav rail (any variant: single, dual primary, dual secondary)                             | `Color/Background/surface`                                                   |
| Page header area (the band that holds breadcrumb, H1, status pill, primary CTA, tab bar) | `Color/Background/surface`                                                   |
| Content area (below the page header, holds cards / tables / forms)                       | `Color/Background/stage` (white)                                             |
| Card inside the content area (when a pattern uses one)                                   | `Color/Background/stage` (white) — same as content area, separated by border |
| Table header row                                                                         | `Color/Background/surface`                                                   |
| Table body rows                                                                          | `Color/Background/stage` (white)                                             |
| Floating layers (dropdowns, modals, popovers)                                            | `Color/Background/stage` (white) with elevation                              |

**Most common mistake**: putting white on the nav rail or page header, putting surface on the content area. If a generated screen feels "off" in a way you can't name, check the backgrounds against this table first.

## Three zones

```
┌─────────────────────────────────────────────────────────────┐
│ Top bar (56px, white)                                       │
├──────┬──────────────────────────────────────────────────────┤
│      │ Page header (surface)                                │
│      │   Breadcrumb / H1 + status pill / CTA / tabs         │
│ Nav  ├──────────────────────────────────────────────────────┤
│ rail │                                                      │
│ (sur-│ Content area (white)                                 │
│ face)│                                                      │
│      │   ┌──────────────────────────────────────────┐      │
│      │   │ Card / table / form  (white)             │      │
│      │   │                                          │      │
│      │   └──────────────────────────────────────────┘      │
│      │                                                      │
└──────┴──────────────────────────────────────────────────────┘
```

## Top bar

- Height: **56px max** (Pixel constraint, do not exceed).
- Background: white.
- Bottom border: `Color/Border/default` 1px, or none if relying on the stage contrast below.
- Horizontal padding: `pxl-space-md` (16) left, `pxl-space-md` right.

### Left cluster

1. Product logo (`mekari [productname]` lockup). The product name uses the product-specific accent (red for Talenta, blue-green for Expense, blue for Qontak) — do not invent a new color, use the brand asset.
2. Optional workspace switcher to the right of the logo (e.g. Talenta's `HRIS ▾`) — only in products with multiple workspaces.

### Right cluster (in order, left to right)

1. **Ask Airene pill** — `BETA` tag inline. Only in products that have shipped the AI assistant. If you're unsure whether the product has it, ask; do not assume.
2. `+` create shortcut icon button.
3. Search icon button.
4. Notification bell with red count badge (`9+` if over 9).
5. App switcher (9-dot grid icon, opens cross-product launcher).
6. **User identity block** — avatar (32px circle, with status dot bottom-right), then text stack: name (`Label/Semibold`) above, company (`Label small/Regular Color/Text/secondary`) below. Right-aligned. The whole block is clickable, opens user menu.

## Nav rail — pick one pattern

### Single rail with labels (Qontak, Talenta)

- Width ~210px.
- Background: `Color/Background/surface`.
- Right border: `Color/Border/default` 1px.
- Items: icon + label, padding `pxl-space-sm` 12 vertical, `pxl-space-md` 16 horizontal, gap `pxl-space-sm` 12.
- Default state: text `Color/Text/secondary`, icon `Color/Icon/default`.
- Hover: subtle bg tint, no color change on text.
- Active state: bg `Color/Background/brand-selected` (`#D6DBF7`), text `Color/Text/selected` (`#4B61DC`), icon `Color/Icon/brand`. Full-width fill, no left bar accent.
- Section grouping: blank line between groups, no headers, no dividers. Resources/Documents/Products/Expenses cluster, then Custom solutions, then Subscription/Settings cluster.
- Bottom: collapse toggle (`«` icon) flush left, and below it a `Company ID: 102938` micro-meta line in `Label small/Regular Color/Text/secondary`.

### Dual rail (Expense)

- Use when the product has 4+ modules each containing 4+ sub-pages.
- Primary rail: ~64px wide, icon-only, background `Color/Background/surface`. Active module has bg `Color/Background/brand-selected` + icon `Color/Icon/brand`. Bottom: collapse toggle.
- Secondary sidebar: ~210px wide, background `Color/Background/surface`, right border `Color/Border/default`.
  - Top: `Overline/Semibold` section header in uppercase (the module name: "TRANSACTIONS", "REPORTS"). Padding `pxl-space-md` top, `pxl-space-md` left.
  - List items below: `Label/Regular`, padding `pxl-space-sm` 12 vertical, `pxl-space-md` 16 horizontal.
  - Active item: bg `Color/Background/brand-selected`, text `Color/Text/selected`.
  - Bottom of secondary sidebar: collapse toggle.

## Page header area

The page header band sits **above** the content area, on `Color/Background/surface`. It holds breadcrumb, H1 + optional status pill, primary CTA, and optional tab bar. The page header is **not** inside a card and is **not** on white — it shares the surface tone with the nav rail.

- Background: `Color/Background/surface`.
- Horizontal padding: `pxl-space-xl` 24.
- Vertical padding: `pxl-space-xl` 24 top, `pxl-space-md` 16 bottom.
- Bottom edge: no explicit border — the transition from surface to the white content area below is the visual separator.

## Content area

The main work area, below the page header.

- Background: `Color/Background/stage` (white).
- Padding inside: `pxl-space-xl` 24 horizontal, `pxl-space-xl` 24 top, `pxl-space-3xl` 40 bottom.
- Max width: none by default. Let content stretch to fill. Form pages are an exception — see `form-view.md`.

## Page header internals (breadcrumb, H1, CTA, tabs)

Vertical order inside the page header area, with `pxl-space-xs` 8 gap between rows:

1. **Breadcrumb** (optional) — `Label small/Regular Color/Text/secondary`. Separator: `/` with spaces. Last segment is the current page, not clickable, same color as others (no bold). Hide breadcrumb if depth is 1.
2. **Title row** — flex row, space-between:
   - Left: `Heading/H1` + optional status pill inline (gap `pxl-space-sm` 12). Status pill anatomy in `detail-view.md`.
   - Right: primary CTA button. See "Primary CTA" below.
3. **Tab bar** (optional) — see "Tab bar" below.

Gap between the bottom of the page header area (surface) and the top of the content area (white): `pxl-space-md` 16. No divider line — the color transition is the separator.

### Primary CTA in header

- Button variant: solid brand. Background `Color/Background/brand-bold`, text `Color/Text/inverse` (white), `Label/Semibold` 14.
- Padding: `pxl-space-sm` 12 vertical, `pxl-space-md` 16 horizontal.
- Radius: `pxl-radii-md` 6.
- Leading icon optional (`+` for create actions).
- Only **one** primary CTA per page header. Secondary actions go in a menu or beside it as outline buttons.

### Tab bar (Pixel 2.4 style)

- Underline tabs, not pill tabs.
- Tab item: `Label/Semibold` 14, padding `pxl-space-sm` 12 vertical, `pxl-space-md` 16 horizontal.
- Active: text `Color/Text/selected` (`#4B61DC`), bottom border 2px `Color/Border/selected` (`#4B61DC`).
- Inactive: text `Color/Text/secondary`, no border.
- Container has a bottom border `Color/Border/default` 1px running the full content width — the active tab's 2px border sits on top of it.
- Gap between tabs: `pxl-space-md` 16. Tabs are left-aligned, not stretched.

## Card / panel surface

When content sits inside a card (most views do):

- Background: white (`Color/Background/stage`).
- Border: `Color/Border/default` 1px, OR borderless if the card is full-width and contrast with `surface` bg is sufficient.
- Radius: `pxl-radii-md` 6 — or up to `pxl-radii-lg` (8–12) for top-level summary cards. Stay consistent within a screen.
- Internal padding: `pxl-space-xl` 24 all sides. Tables override this — see `index-view.md`.
- No shadow. Cards in the page flow are flat.

## Page footer / floating action bar (when present)

Forms and edit screens use a sticky bottom action bar:

- Position: sticky at content area bottom, OR pinned to viewport bottom for long forms.
- Background: white, top border `Color/Border/default` 1px.
- Padding: `pxl-space-md` 16 vertical, `pxl-space-xl` 24 horizontal.
- Layout: flex row, space-between, OR right-aligned cluster with Cancel as text link on the left and Submit primary on the right.

## Common mistakes

- Top bar over 56px tall (often happens when designers add a search bar inline — put search behind an icon instead).
- Forgetting the right border on the nav rail — without it, nav and content merge visually.
- Putting the primary CTA inside the card instead of in the page header.
- Wrapping breadcrumb in a card — breadcrumb sits naked on the stage.
- Making both rails in dual-rail navigation the same width — the primary rail must be visibly narrower.

## When you need exact props

Load the `pixel` skill and use Pixel MCP `get-component` for the top bar, nav, and tabs. Common components: `MpAppBar`, `MpSidebar`, `MpTab`, `MpBreadcrumb`, `MpButton`.
