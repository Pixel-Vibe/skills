# Detail view

A page showing one record with many fields. Anchored example: Transaction detail in Expense, Employee detail in Talenta, Customer detail in Qontak.

## When to use this pattern

- The user navigated into a single entity to inspect it.
- The screen is read-first; edits happen via dedicated action buttons (Edit, Approve, Reject) or a separate edit page, not inline.
- The data is mostly key-value pairs, with some embedded media (receipts, attachments) and a status/audit section.

## Anatomy

```
Breadcrumb (e.g. Transactions / Cards)
H1 EntityName  [Status pill]
┌──────────────────────────────────────────────┐
│ H2 "Transaction details"  (or similar)      │
│                                              │
│ Label1            Value1                     │
│ Label2            Value2                     │
│                   Secondary value             │
│ ...                                          │
│ Status label      ● Approved by [Name]       │
│                   25 Jul 2023, 10:24 (GMT+7) │
│                   Show more                  │
│ ...                                          │
│ Receipt           [thumb][thumb][thumb]      │
│ Note              free-form text             │
└──────────────────────────────────────────────┘
```

## Page header

- Breadcrumb: required (the user came from somewhere).
- H1: the entity's primary identifier (merchant name, employee name, customer name).
- Status pill inline to the right of H1.
- Primary CTA: action verbs for this entity ("Approve", "Edit", "Cancel transaction"). If multiple actions, primary is the most common positive action; others sit as outline buttons or in an overflow menu (`⋮`) to the right of primary.

## Status pill anatomy

- Radius: `pxl-radii-full` 999.
- Padding: `pxl-space-3xs` 4 vertical, `pxl-space-xs` 8 horizontal.
- Text: `Label small/Regular` 12, weight Semibold — exception to default Regular because status pills are scanned.
- Color mapping (background + text):
  - Success / Approved / Active → bg `#E7F5EE` (lightened from `success-bold`), text `Color/Background/success-bold` (`#1C8459`).
  - Danger / Rejected / Failed → bg `#FBE9E7`, text `Color/Background/danger-bold` (`#C33E35`).
  - Warning / Pending / Awaiting → bg `#FEF3C7`, text `#956400`.
  - Info / Draft / In review → bg `Color/Background/brand-selected` (`#D6DBF7`), text `Color/Text/selected` (`#4B61DC`).
  - Neutral / Closed / Archived → bg `#EDF0F2`, text `Color/Text/secondary` (`#656F80`).

Do not invent new colors. If the status doesn't fit a bucket above, ask the PM what semantic bucket it belongs to.

## Key-value list

This is the heart of the pattern.

- Layout: **two-column grid**, label column fixed-width, value column flexible.
- Label column width: ~180px. Adjust ±20px to fit the longest label, but keep it fixed once chosen.
- Vertical gap between rows: `pxl-space-xl` 24. Use `pxl-space-md` 16 only if the row count is high (15+) and density matters more than legibility.
- Label cell: `Label/Regular` 14, color `Color/Text/secondary` (`#656F80`). Not Semibold — labels are quieter than values.
- Value cell: `Label/Regular` 14, color `Color/Text/default` (`#272B32`).
- Vertical alignment: top — important when value has multiple lines.

### Value composition rules

- **Plain value**: just the text. `Notion`, `25 Jul 2023`.
- **Primary + secondary**: stack two lines, primary `Label/Regular`, secondary `Label small/Regular Color/Text/secondary`.
  - Example: Amount value = `Rp150.000` on first line, `USD9.80` on second.
  - Example: Cardholder value = `Agung Setiawarman` on first line, employee ID `CP092` on second.
- **Copyable value** (IDs, tokens, references): value text followed by a copy icon button (`pxl-space-2xs` 6 gap). Click copies to clipboard, shows micro-toast "Copied".
- **Status with audit trail**: status dot + status text on line 1, actor + timestamp on line 2, optional `Show more` text link on line 3 that expands a full timeline.
  - Status dot: 8px circle, color matches the status semantic.
  - Format: `● Approved by Rizal Candra` / `25 Jul 2023, 10:24 (GMT+7)`. Always include timezone.
  - `Show more` is `Label small/Textlink Color/Text/link`, underlined on hover. When expanded, it becomes `Show less`.
- **Media thumbnails** (receipts, ID photos): inline row of 56–72px squares, gap `pxl-space-xs` 8, radius `pxl-radii-md` 6, border `Color/Border/default` 1px. Click opens a lightbox. Empty slots show a placeholder grey square — do not skip empty slots if the system reserves them.
- **Long free-form text** (notes, descriptions): just the text wrapped in the value cell. Do not truncate by default. If exceptionally long (>500 chars), collapse after 3 lines with a `Show more` text link.
- **Linked entity** (e.g. linked invoice from a transaction): value is a text link in `Color/Text/link`, clicking navigates to that entity's detail page.

## Sectioning a long detail view

If the entity has 20+ fields, split into multiple cards stacked vertically, each with its own H2:

- `Transaction details`
- `Approval history`
- `Linked documents`
- `Activity log`

Gap between cards: `pxl-space-md` 16. Do not separate cards with horizontal rules.

For very long records, consider an anchor nav on the right side (sticky list of section titles). Threshold: 4+ sections OR 40+ total fields.

## Embedded actions

- **Inline edit on a field** is rare in Mekari detail views — prefer routing to an Edit page. If unavoidable (e.g. assigning a category), present a select dropdown directly in the value cell, no "edit" pencil affordance.
- **Row-level secondary actions** (resend receipt, download PDF) live in a card-level action menu (`⋮` button top-right of the card), not next to each row.

## Edge cases the PRD probably forgot

Surface these before designing:

- What appears in a value cell when the field is **null**? Default: an em dash (`—`) in `Color/Text/secondary`, not blank, not "N/A".
- What if the user **lacks permission** to see a field? Default: hide the row entirely, not greyed-out.
- What if the **status changes** while the user is viewing? Polling? Stale banner? Decide explicitly.
- What about **deleted referenced entities** (the cardholder was offboarded)? Default: show the name with a tooltip "(no longer active)", do not show as a link.
- **Print / export** view of this page — same layout but no nav, no top bar, no actions? Confirm with PM.

## Empty inner sections

A detail view's inner card can have empty sub-sections (e.g. "Attachments" with no files yet). Use a minimal inline empty state — `Label small/Regular Color/Text/secondary` "No attachments" — not a full illustrated empty state. Save the illustration for whole-page empty states.

## Output contract for this pattern

When you ship a detail view:

- Label column width is documented (e.g. "180px").
- Every status field includes actor + timestamp + timezone.
- Null-value behavior is specified.
- Empty-section behavior is specified.
- Permission-gated fields are listed.
- The card-level action menu inventory is listed (download, export, delete, etc.).
