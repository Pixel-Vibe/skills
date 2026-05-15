# Form view

A page for creating or editing a single record. Anchored example: Request reimbursement in Talenta, Create transaction in Expense, Add employee in Talenta.

## When to use this pattern

- The user is creating a new record or editing an existing one.
- The flow has 3+ fields. (Fewer → inline edit or modal.)
- There's a clear submit action at the end.

## Anatomy

```
Breadcrumb (e.g. Requests)
H1 "Request reimbursement"  (or "Edit ...")
┌──────────────────────────────────────────────┐
│ [Form fields, single column, max ~440px]    │
│                                              │
│ Field label *                                │
│ [Input              ▾]                       │
│ Helper text                                  │
│                                              │
│ Field label *                                │
│ [Input              ▾]                       │
│                                              │
│ Section divider                              │
│                                              │
│ H3 Sub-section                               │
│ [Sub-section content]                        │
└──────────────────────────────────────────────┘
                            Cancel    [Submit]
```

## Form column width

- Single column, left-aligned.
- **Maximum 6 columns** (out of a 12-column grid). In practice this is ~440px for most forms, up to ~560px if any field needs wider input (date range, multi-select with chips).
- Form sits **left-aligned within the content area** — right side stays empty. This is deliberate: forms are vertical-scanning tasks, the empty space on the right reduces eye travel and leaves room for inline help if needed.
- Exception: forms with side-by-side related fields (start date / end date, address city / postal code) can use a 2-column grid for those rows only. Field pairs: gap `pxl-space-md` 16.

### Form group with stepper

When a form includes a stepper component, the form group position depends on the stepper's layout:

- **Stepper left-aligned** (same alignment as the form group) → form group stays left-aligned, max 6 columns. The stepper and form share the same left edge.
- **Stepper full-width** (spans the entire content area) → form group centers itself horizontally within the content area, still max 6 columns. This prevents the form from feeling anchored to one side while the stepper spans the full width above it.

Never exceed 6 columns for the form group regardless of stepper layout.

## Field anatomy

Every field is a vertical stack with `pxl-space-2xs` 6 gap between elements:

1. **Label** (top): `Label/Semibold` 14, `Color/Text/default`. Required indicator is a red asterisk `*` in `Color/Background/danger-bold` immediately after the label text with a small gap (`pxl-space-3xs` 4).
2. **Input** (control): the field itself.
3. **Helper text** (below): `Label small/Regular` 12, `Color/Text/secondary`. Optional but encouraged for complex fields.
4. **Error message** (replaces helper text on validation fail): `Label small/Regular` 12, `Color/Background/danger-bold`.

Gap between fields (vertical): `pxl-space-md` 16.

## Input controls

### Text input

- Height: 40px.
- Padding: `pxl-space-sm` 12 horizontal, ~`pxl-space-xs` 8 vertical.
- Border: `Color/Border/default` 1px, radius `pxl-radii-md` 6.
- Text: `Label/Regular` 14.
- Placeholder: `Color/Text/secondary`, `Label/Regular`. Use placeholders for example/format hints only (`e.g. john@mekari.com`), never to replace the label.
- Focus: border `Color/Border/selected` (`#4B61DC`), subtle focus ring optional.
- Error: border `Color/Background/danger-bold`.
- Disabled: bg `Color/Background/surface`, text `Color/Text/secondary`.

### Select / dropdown

- Same chrome as text input. Trailing chevron `▾`, `Color/Icon/default`.
- Closed placeholder: e.g. "Select reimbursement name".
- Open: dropdown panel below, max-height ~280px with scroll. White bg, `Color/Border/default` 1px, subtle elevation (this is a floating layer, lift allowed).
- Options: `Label/Regular`, padding `pxl-space-sm` 12 horizontal, `pxl-space-xs` 8 vertical. Hover bg `Color/Background/surface`. Selected option has a checkmark trailing.
- For searchable selects (typically when options > 10), include a search input at the top of the panel.

### Date picker

- Same chrome as text input. Trailing calendar icon.
- Format displayed: `20 Sep 2023` (consistent with detail-view date format).
- On click, opens a calendar popover. White bg, `Color/Border/default` 1px, elevation allowed.

### Textarea

- Same chrome as text input. Min-height ~96px.
- Resize: vertical only, resize handle visible bottom-right.
- Character counter: if there's a max length, show `Label small/Regular` "X / Y characters" below right.

### File upload

This is one of Mekari's strongest patterns. See `upload-flow.md` for the full spec. Minimum here:

- "Choose file" outline button + status text in a small bordered container (button on the left, filename or "No file chosen yet" on the right).
- **Always include helper text below stating allowed formats and max size**, e.g. "File must be in JPG, JPEG, PNG, and PDF with a maximum of 10 MB."

### Checkbox / radio / toggle

- Use Pixel components, don't roll your own.
- Checkbox label: right of the box, `Label/Regular` 14, vertical-aligned center.
- Group label (when multiple checkboxes form a question): `Label/Semibold` above the group, `pxl-space-2xs` 6 gap to first option.

### Multi-select / chip input

- Selected chips inside the input, gap `pxl-space-3xs` 4, each chip has an `×` to remove.
- Chip styling: `pxl-radii-full` 999, bg `Color/Background/brand-selected`, text `Color/Text/selected`, `Label small/Regular` Semibold, padding `pxl-space-3xs` 4 vertical, `pxl-space-xs` 8 horizontal.

## Section structure within a form

For longer forms, split into sub-sections inside the same card:

1. First sub-section: fields directly under H1 (no sub-header needed).
2. Subsequent sub-sections: horizontal divider `Color/Border/default` 1px full-width, then `pxl-space-xl` 24 gap, then `Heading/H3` sub-section title, then fields.

Example: a reimbursement form has the request meta on top (date, name, attachment, description), then a divider, then "Benefit component" as H3 with related fields.

If sub-sections become too long, consider multiple cards instead of dividers — but only if the sub-sections are independently submittable or logically separate (rare in a single form).

## Empty / placeholder state for dynamic sub-sections

Some form sections are populated by user action — e.g. "Benefit component" empty until the user clicks "Add benefit", a line items table empty until rows are added.

When empty:

- Show a small illustrated empty state centered inside the section. See `empty-state.md`.
- Title: descriptive of what will appear ("Reimbursement benefit will appear here").
- Helper: action-pointing with the literal button name bolded ("Add benefit from **Add benefit** button.").
- The add-action button sits **above** the empty state (top-left of the section, outline button), not inside the empty state itself. This way the affordance is reachable whether the section is empty or populated.

When populated:

- Sub-section transitions to a small inline table or list of the added items, with edit/remove affordances per row.

## Action footer

Always at the bottom, sticky or naturally flowing:

- **Cancel** (text link or low-emphasis button) — left side OR left of the Submit button.
- **Submit** (primary, brand-bold) — right side. Label is a verb specific to the action: "Submit", "Create transaction", "Save changes", "Send request". Not "OK" or "Done".
- For long forms with intermediate steps, add **Save draft** as a tertiary outline button between Cancel and Submit.
- Gap between buttons: `pxl-space-md` 16.

For multi-step forms (wizards), the footer also has **Back** and **Next** instead of Submit until the last step.

## Validation

### When to validate

- **On blur** for individual fields with format requirements (email, phone, currency).
- **On submit** for everything else.
- **Never on every keystroke** — that's noise.

### Error display

- Field-level: helper text replaced with error message in `Color/Background/danger-bold`. Field border turns red.
- Form-level (multiple errors or non-field errors like server validation): banner at the top of the form card. Banner bg `#FBE9E7`, border `Color/Background/danger-bold` 1px left, padding `pxl-space-md` 16. Icon (alert-circle) `Color/Background/danger-bold` left of text. Plain language: "We couldn't submit your request. Please review the highlighted fields."
- **Don't** scroll the user to the first error automatically — focus the first invalid field instead, which the browser scrolls to gracefully.

### Success state

- After successful submit, navigate away (usually to the detail view of the created record, or back to the index). Show a success toast briefly.
- Do not show "Form submitted!" inside the form itself unless the form stays open for repeat entries.

## Conditional fields

When a field's visibility depends on another field's value:

- Reveal/hide with a brief fade-in (150ms), don't slide.
- Maintain field order — don't move existing fields up/down to fill space.
- Conditional fields are still subject to the same validation; if hidden, treat their value as null/empty.

## Anti-patterns specific to this pattern

- **Card-per-section layout.** Never wrap form sections in cards. Form sections are separated by a horizontal divider line and a sub-section heading (`Heading/H3`), not by boxed containers. Cards add visual weight, increase nesting, and make the form feel like a settings page rather than a task.
- **Two primary buttons.** A form has exactly one primary action (Submit, Save, Create). If there's a secondary action (Save draft), it's an outline button. Cancel is always a text link or low-emphasis button — never a primary.
- **Primary button inside a card.** If a form section has a sub-action (like "Add benefit" inside a Benefit component section), that action is an outline button, not a primary. The page-level primary lives in the footer.
- **Form group exceeding 6 columns.** Never stretch the form group beyond 6 columns regardless of screen width or stepper layout.
- **Custom stepper variants.** Never create a new stepper style — use the Pixel stepper component as-is. Completed step states follow the component definition exactly.

## Edge cases the PRD probably forgot

- **What's the max length** of free-form fields? Default text limit if PM didn't say: 255 chars for short text, 2000 for textarea.
- **Auto-save behavior** — does this form auto-save drafts? If yes, where's the indicator ("Saved 2 seconds ago")?
- **Unsaved changes warning** — does the user see a confirm if they navigate away with unsaved changes? Default: yes for edit forms, no for create forms with empty state.
- **Server-side validation** — what errors can come back from the API that the form needs to render? Default banner copy listed above is generic; PM may want product-specific copy.
- **Field-level permissions** — can some fields be view-only based on user role? If yes, render them disabled with helper text "You don't have permission to edit this".
- **Pre-filled values** — for edit forms, what state should the form show on load? Confirm whether placeholders are still useful or if the existing values fill in.
- **Cancel behavior** — does Cancel discard changes immediately, or show a confirm? Default: confirm if dirty, immediate if clean.

## Output contract for this pattern

When you ship a form view:

- Field list with: name, control type, required y/n, validation rule, helper text copy.
- Section structure (single section or sub-sections with dividers).
- Footer action labels (Submit verb specific to the action).
- Validation strategy (on blur vs on submit per field type).
- Error copy for known failure cases.
- Cancel and unsaved-changes behavior.
- Auto-save behavior (if any).
