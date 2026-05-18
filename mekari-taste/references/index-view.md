# Index / list view

A page listing many records of the same type. Anchored example: Subscription quota table in Qontak, Transaction list in Expense, Employee list in Talenta.

## When to use this pattern

- The user needs to scan, sort, filter, or pick from many records.
- Each record has 4–8 columns worth displaying at the list level; deeper info lives in the detail view.
- The page is the entry point to detail views — clicking a row navigates in.

## Anatomy (in order, top to bottom)

```
Page header: H1 + primary CTA (top-right)
Tab bar (if the entity has multiple views)
┌──────────────────────────────────────────────┐
│ Summary card (optional)                      │
│   Key meta · Value          Key meta · Value │
└──────────────────────────────────────────────┘
┌──────────────────────────────────────────────┐
│ Toolbar:  [filters] [search]    [view opts]  │
├──────────────────────────────────────────────┤
│ Table                                        │
│ ┌────────────────────────────────────────┐  │
│ │ Col1   Col2   Col3   Col4   Col5      │  │
│ ├────────────────────────────────────────┤  │
│ │ row data ...                          │  │
│ │ row data ...                          │  │
│ └────────────────────────────────────────┘  │
│ Pagination                                  │
└──────────────────────────────────────────────┘
```

## Summary card (optional but common)

Sits above the table when the list has aggregate context worth surfacing at a glance — total balance, package name, period date range.

- Layout: horizontal flex, evenly distributed columns or fixed-width key-value pairs.
- Card padding `pxl-space-xl` 24.
- Each summary cell:
  - Top: small label `Label small/Regular Color/Text/secondary` (e.g. "Package name", "Renewal date").
  - Bottom: large value `Heading/H2` or `Label/Semibold` depending on importance. Status pill inline if applicable (e.g. `Qontak 360` + `Active` pill).
- Gap between cells: `pxl-space-3xl` 40 horizontal.

Use this card only when the aggregate info changes the user's reading of the table below. Skip it for plain lists where every record is independent.

## Toolbar above the table

A single row, full-width inside the card or above it.

- Left cluster: filters (`Filter` button or chip group), separator, search input.
- Right cluster: view options ("Columns", "Density", "Export") as outline buttons or icon buttons.
- Height: ~40px. Padding: `pxl-space-sm` 12 vertical, `pxl-space-md` 16 horizontal.
- Border: `Color/Border/default` 1px at the bottom only — it separates toolbar from the table header.

See `filter.md` and `bulk-select.md` for what may appear in this toolbar.

## Table

### Container

- White background.
- Border: optional — if the table is inside a card already, no inner border. If the table sits directly on stage with no wrapping card, give it `Color/Border/default` 1px + `pxl-radii-md` 6.
- No shadow.

### Header row

- Background: subtle gray. In Pixel 2.4 use `Color/Background/surface` (`#F1F5F9`) OR a slightly darker `#EDF0F2`. Match what the product uses elsewhere; don't introduce a new shade.
- Height: ~44px.
- Padding: `pxl-space-md` 16 horizontal, `pxl-space-sm` 12 vertical.
- Text: `Label/Semibold` 14, `Color/Text/default`.
- Sortable column: chevron `▾` after label, `Color/Icon/default`. Active sort: chevron in `Color/Icon/brand`.
- Info-icon (ⓘ) for columns that need a tooltip — e.g. "Initial ⓘ", "Remaining additional ⓘ". Hover reveals definition. Use this whenever a column header is jargon.

### Body row

- Height: dynamic, minimum 56px.
- Padding: `pxl-space-md` 16 horizontal, `pxl-space-md` 16 vertical.
- Bottom border only: `Color/Border/default` 1px. No top border (the header has its bottom border). Last row has no bottom border.
- No zebra stripes. No outer table border.
- Hover: row bg lightens to `#F8FAFC` (a tint above `surface`). Cursor pointer if the row is clickable.
- Click: navigate to detail view. Whole row is the click target, not just the first column.

### Cell content rules

- **Text values**: `Label/Regular` 14, `Color/Text/default`.
- **Numeric values**: right-aligned. Always. Currency, counts, percentages, durations.
- **Multi-line cells**: primary value `Label/Regular`, secondary `Label small/Regular Color/Text/secondary` below it (e.g. "WhatsApp balance" + "Used by all WhatsApp business account").
- **Status cells**: status pill (anatomy in `detail-view.md`), left-aligned.
- **Date cells**: format `12 Jul 2026` (no comma, abbreviated month). For "last X days ago" relative time, only use if recency is the column's purpose; otherwise prefer absolute dates.
- **Currency cells**: `Rp1.000.000` format, right-aligned. If a column mixes currencies, append unit: `Rp1.000` / `USD9.80`.
- **Empty cell value**: em dash `—` in `Color/Text/secondary`, right-aligned if numeric column, left-aligned otherwise.
- **Action cell** (the last column with row-level actions): icon buttons or `⋮` overflow menu, right-aligned, no header label.

### Column widths

- First column (identifier): widest, no max width unless reasonable. Truncate with ellipsis only if name length exceeds 240px, with a tooltip on hover.
- Numeric columns: fit-to-content, with at least `pxl-space-md` 16 padding on both sides.
- Action column: fixed ~60px, no shrink.

## Pagination

Below the table, inside the card or directly under it.

- Padding `pxl-space-md` 16.
- Left: page size selector "Show 10 per page ▾".
- Center: "Showing 1–10 of 1.247" in `Label small/Regular Color/Text/secondary`.
- Right: prev/next chevrons + numeric page buttons. Active page button has bg `Color/Background/brand-selected`, text `Color/Text/selected`.

For infinite scroll: only use when sort order is reverse-chronological feed (notifications, activity log). Default to numeric pagination.

## Empty state inside an index view

When the table has zero rows (either no data ever, or filtered to empty):

- **No data ever**: replace the whole table area with a full empty state — illustration, title, helper, CTA. See `empty-state.md`.
- **Filtered to empty**: smaller inline empty state inside the table area — no illustration, just `Label/Semibold` "No results found" + `Label small/Regular` "Try adjusting your filters." with a `Clear all filters` text link.

## Loading state

- Initial load: skeleton rows (5–8 placeholder rows with gray bars matching cell widths). `MpSkeleton` in Pixel.
- Re-load after filter/sort: keep existing rows visible, apply a subtle overlay or shimmer; don't blank the table.

## Edge cases the PRD probably forgot

- **Very long single-row content** (a description column with 300 chars) — truncate to 1 line with ellipsis + tooltip on hover, OR move the column out of the table into the detail view.
- **No-permission rows** — hide entirely or show with redacted values? Default: filter them out server-side.
- **Bulk actions** — does this list need bulk select? See `bulk-select.md`.
- **Saved views / pinned filters** — does the user need to save a filter combination? Often missed in PRDs.
- **Default sort** — what sorts the first time the user lands? Newest first is the usual default; confirm with PM.
- **Sticky header on scroll** — yes by default. Confirm.
- **Export** — CSV, XLSX, or PDF? What's the row limit before it goes to async email delivery?
- **Real-time updates** — does this list refresh while the user is on it (Qontak inbox, Expense notifications)?

## Anti-patterns specific to this pattern

- **Card-per-row layout.** Never wrap each record in a card. Cards consume vertical space and break scannability. Use table rows — flat, with a 1px bottom border separator. The only card allowed on an index page is a summary card above the table (for aggregate context like total balance or active package), and only when it genuinely changes how the user reads the table.
- **Outer border on the table.** The table is borderless on the outside. The page's content area provides the boundary.
- **Zebra striping.** Rows alternate only via the 1px bottom border. No alternating background colors.
- **Center-aligned numeric columns.** Always right-align numbers — amounts, counts, percentages.
- **Colored row backgrounds for status.** Status goes inside a pill in the cell, not as a row background tint.

## Output contract for this pattern

When you ship an index view:

- Column list with: name, data type, sortable y/n, alignment, default sort y/n.
- Pagination decision (numeric vs infinite) with reasoning.
- Filter inventory (see `filter.md`).
- Empty state copy and CTA.
- Bulk action inventory if applicable.
- Loading and re-loading behavior specified.
