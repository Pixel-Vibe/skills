# Index / list view

> **Pixel MCP validation gate** — Before generating code from this reference, validate every Pixel component via `get-component("<name>")` on the Pixel MCP. This file defines anatomy, placement, and taste rules. MCP is the source of truth for current props and slot API. If MCP output conflicts with this file → trust MCP, note the discrepancy.


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

## Summary card / summary strip (optional but common)

Sits above the table when the list has aggregate context worth surfacing at a glance — total balance, package name, period date range, or per-type counts.

### Summary strip variant (per-type count row)

Used when the list has multiple source/type categories and the user benefits from seeing counts per type at a glance (e.g. AI Resources: "2 Links · 3 Files · 1 Content").

```vue
<MpFlex :class="css({ h: '96px', borderWidth: '1px', borderColor: 'border.default', rounded: 'lg', overflow: 'hidden', bg: 'background.neutral', flexShrink: 0 })">
  <template v-for="(card, index) in summaryItems" :key="card.type">
    <div v-if="index > 0" :class="css({ width: '1px', h: '56px', bg: 'border.default', flexShrink: 0, my: 5 })" />
    <MpFlex direction="column" gap="1" :class="css({ flex: 1, minW: 0, px: 5, pt: 5, pb: 5 })">
      <MpText size="label" weight="semiBold" color="text.default">{{ card.type }}</MpText>
      <MpFlex alignItems="center" gap="2">
        <MpIcon :name="card.icon" size="md" color="icon.default" />
        <MpFlex alignItems="baseline" gap="1">
          <MpText size="h2" weight="semiBold">{{ card.count }}</MpText>
          <MpText size="label" color="text.secondary">{{ card.unit }}</MpText>
        </MpFlex>
      </MpFlex>
    </MpFlex>
  </template>
</MpFlex>
```

- Strip height: fixed `96px`. Flex row.
- Cells separated by `1px` vertical dividers, `56px` tall, vertically centered (`my: 5`).
- Each cell: type label (`Label small/Semibold`), then icon (`size="md" color="icon.default"`) + count (`H2/Semibold`) + unit (`Label/Regular text.secondary`).
- All icons use `color="icon.default"` — no status-colored icons in summary strips.

### Key-value summary card variant

Used when aggregate info is a single record (e.g. subscription details, period).

- Layout: horizontal flex, evenly distributed columns or fixed-width key-value pairs.
- Card padding `pxl-space-xl` 24.
- Each cell: top `Label small/Regular Color/Text/secondary`, bottom `Heading/H2` or `Label/Semibold`.
- Gap between cells: `pxl-space-3xl` 40 horizontal.

Use either summary variant only when the aggregate info changes the user's reading of the table below. Skip for plain lists where every record is independent.

## Toolbar above the table

A single row, full-width inside the card or above it.

- Left cluster: filters (`Filter` button or chip group), separator, search input.
- Right cluster: view options ("Columns", "Density", "Export") as outline buttons or icon buttons.
- Height: ~40px. Padding: `pxl-space-sm` 12 vertical, `pxl-space-md` 16 horizontal.
- Border: `Color/Border/default` 1px at the bottom only — it separates toolbar from the table header.

### Toolbar component sizing rules

See `foundations.md` for the full sizing table. Summary for toolbar elements:

- `MpSelect` filters → always `size="md"`. Use `placeholder` prop for the "all" state (e.g. `placeholder="All channels"`); do **not** add an "All channels" `<option>` as the first item. Init the bound ref to `''`.
- `MpInputGroup` / `MpInput` search → always `size="md"`.
- `MpButton` in toolbar (sort, view switcher, "All filters") → always `size="md"`.
- "All filters" button → `variant="textLink"` not `ghost`.
- View switcher button (icon + chevron, no label) → `variant="secondary" size="md"`, use slot content with `<MpIcon>` directly (not `left-icon`/`right-icon` props) so icon `color="icon.default"` can be set explicitly.

See `filter.md` and `bulk-select.md` for what may appear in this toolbar.

## Table

### Container
- White background.
- Border: optional — if the table is inside a card already, no inner border. If the table sits directly on stage with no wrapping card, give it `Color/Border/default` 1px + `pxl-radii-md` 6.
- No shadow.

### Header row
- Background: subtle gray. In Pixel 2.4 use `Color/Background/surface` (`#F1F5F9`) OR a slightly darker `#EDF0F2`. Match what the product uses elsewhere; don't introduce a new shade.
- Height: **52px** — set globally in `pixel.css` (`.mp-table thead th { height: 52px; }`). Do not override per-page.
- Padding: `pxl-space-md` 16 horizontal, `pxl-space-sm` 12 vertical.
- Text: `Label/Semibold` 14, `Color/Text/default`.
- Sortable column: chevron `▾` after label, `Color/Icon/default`. Active sort: chevron in `Color/Icon/brand`.
- Info-icon (ⓘ) for columns that need a tooltip — e.g. "Initial ⓘ", "Remaining additional ⓘ". Hover reveals definition. Use this whenever a column header is jargon.
- **Do NOT use `is-fixed` on `MpTableHead`** — it causes double border artifacts that require workarounds. Use plain `<MpTableHead>`.

### Body row
- Height: dynamic, minimum 56px.
- Padding: `pxl-space-md` 16 horizontal, **`pxl-space-sm` 12 vertical** — set globally in `pixel.css` (`.mp-table tbody td { padding-block: var(--mp-spacing-3); }`). Do not override per-page.
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
- **Action cell** (the last column with row-level actions): use `MpButton variant="secondary" right-icon="chevrons-down"` for labeled dropdown actions, or icon-only `⋮` overflow menu. Right-aligned (`textAlign: right`), no header label. Do not set `size="sm"` — use default (md).

### Column spacing

Always set `padding-left` and `padding-right` on every `th` and `td` to `pxl-space-xl` (24px = `var(--mp-spacing-6)`). Use a `<style scoped>` block with `:deep()` — do not add `:class` to every individual cell:

```vue
<style scoped>
/* Column spacing — pxl-space-xl = 24px */
:deep(.mp-table th),
:deep(.mp-table td) {
  padding-left: var(--mp-spacing-6);
  padding-right: var(--mp-spacing-6);
}
</style>
```

### Selectable tables (checkbox + first column)

When rows are selectable, **never add a standalone checkbox column**. Merge the checkbox into the first data column:

```vue
<!-- Header -->
<MpTableCell scope="col">
  <MpFlex alignItems="center" gap="3">
    <MpCheckbox id="select-all" v-model="allSelected" />
    Customer name
  </MpFlex>
</MpTableCell>

<!-- Data rows -->
<MpTableCell as="td" scope="row">
  <MpFlex alignItems="center" gap="3">
    <MpCheckbox :id="`row-${item.id}`" />
    <MpAvatar :name="item.name.split(' ')[0]" size="sm" />
    <MpText size="label" weight="semiBold">{{ item.name }}</MpText>
  </MpFlex>
</MpTableCell>
```

- `MpAvatar` when present: pass only the **first word** of the name as `:name` so it renders a single initial at `size="sm"`.
- Gap between checkbox, avatar, and label: `gap="2"` (8px).

### Column widths
- First column (identifier): widest, no max width unless reasonable. Truncate with ellipsis only if name length exceeds 240px, with a tooltip on hover.
- Numeric columns: fit-to-content, with at least `pxl-space-md` 16 padding on both sides.
- Action column: fixed ~120px, no shrink.

## Table + Pagination wrapper

Wrap `MpTableContainer` and the pagination row together in `MpFlex direction="column"` so they stay tight — no gap between last table row and pagination controls.

```vue
<MpFlex direction="column">
  <MpTableContainer>...</MpTableContainer>
  <!-- pagination -->
  <Pixel.div px="2" py="4">...</Pixel.div>
</MpFlex>
```

## Pagination

Below the table, always inside the shared wrapper above.

**Visibility rule:** Show pagination whenever total rows > 10. If total rows ≤ 10, hide the entire pagination row — the table is short enough to read without it. Never show an empty or disabled pagination bar.

Source: Pixel MCP `get-pattern("pagination")`. The implementation below is the production-ready version derived from that pattern — use this, not the raw MCP output.

**Pixel 3 implementation** (use this exact pattern — do not substitute with plain MpSelect for rows-per-page):

```vue
<!-- Only render when total > 10 -->
<Pixel.div v-if="total > 10" px="2" py="4">
  <MpFlex justify="space-between">
    <!-- Left: rows-per-page + showing info -->
    <MpFlex alignItems="center">
      <MpText :class="css({ pr: '1', pl: '1' })" color="text.secondary">Rows per page</MpText>
      <MpPopover is-close-on-select>
        <MpPopoverTrigger>
          <MpButton size="sm" variant="ghost" :class="css({ h: '7', display: 'inline-flex', pl: '3', pr: '2', py: '2' })">
            <MpText>{{ rowsPerPage }}</MpText>
            <MpIcon name="chevrons-down" size="sm" />
          </MpButton>
        </MpPopoverTrigger>
        <MpPopoverContent>
          <MpPopoverList>
            <MpPopoverListItem
              v-for="opt in [10, 25, 50]" :key="opt"
              :is-active="rowsPerPage === opt"
              @click="rowsPerPage = opt; currentPage = 1"
            >{{ opt }}</MpPopoverListItem>
          </MpPopoverList>
        </MpPopoverContent>
      </MpPopover>
      <MpText :class="css({ pl: '5', py: '1' })" color="text.secondary">
        Showing {{ start }}-{{ end }} of {{ total }}
      </MpText>
    </MpFlex>
    <!-- Right: page jump + prev/next -->
    <MpFlex alignItems="center">
      <MpFlex :class="css({ flexShrink: '0' })">
        <MpSelect size="sm" :model-value="String(currentPage)" :is-disabled="totalPages === 1"
          @change="(e: Event) => currentPage = Number((e.target as HTMLSelectElement).value)">
          <option v-for="p in totalPages" :key="p" :value="String(p)">{{ p }}</option>
        </MpSelect>
      </MpFlex>
      <MpText :class="css({ pl: '2', pr: '4', py: '1', whiteSpace: 'nowrap' })" color="text.secondary">of {{ totalPages }} page</MpText>
      <MpTooltip label="Previous page" position="bottom">
        <MpButton variant="ghost" left-icon="chevrons-left" size="sm" :is-disabled="currentPage <= 1" @click="prevPage" />
      </MpTooltip>
      <MpTooltip label="Next page" position="bottom">
        <MpButton variant="ghost" left-icon="chevrons-right" size="sm" :is-disabled="currentPage >= totalPages" @click="nextPage" />
      </MpTooltip>
    </MpFlex>
  </MpFlex>
</Pixel.div>
```

**State** — use number refs directly, not string refs:
```ts
const rowsPerPage = ref(10)
const currentPage = ref(1)

// Reset page on filter/search change
watch([filterA, searchQuery], () => { currentPage.value = 1 })
watch(totalPages, (next) => { if (currentPage.value > next) currentPage.value = next })
```

**Imports needed:** `Pixel, MpPopover, MpPopoverTrigger, MpPopoverContent, MpPopoverList, MpPopoverListItem, MpTooltip, MpSelect, MpButton, MpIcon, MpText, css`

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
- **Sticky header on scroll** — do NOT use `MpTableHead is-fixed` (causes double border). Use plain `<MpTableHead>` and handle scroll at the container level if needed.
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
