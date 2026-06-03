---
name: mekari-taste
description: "Imagination phase for Mekari PRD to code workflow. Loaded by implement-to-pixel on the PRD path (when input is a Confluence link, pasted PRD text, or chat-described requirements — not Figma). This skill reads the PRD end-to-end, applies five Mekari visual principles (Layered, Quiet, Accountable, Density, Frames hold) to imagine each screen, identifies which Mekari patterns apply, renders one low-fidelity wireframe per screen via the visualizer, and drafts a structured implementation plan. After the user confirms the plan, control returns to implement-to-pixel to generate Vue/Nuxt code. Do not use this skill standalone for code generation — it produces wireframes and plans, not code. Do not use for Figma input — implement-to-pixel handles Figma directly without this imagination phase. Do not use for marketing pages, landing pages, or external-facing editorial content."
---

# Mekari Taste — Imagination Phase

The imagination phase for Mekari PRD → code workflow. Reads PRD, applies five visual principles, draws wireframes, drafts a plan. Then stops and waits for user confirmation before `implement-to-pixel` resumes code generation.

## When this loads

Loaded by `implement-to-pixel` when input is a PRD (Confluence URL, pasted text, or chat description). Not loaded when input is Figma — Figma already has visual structure, no imagination needed.

If you find yourself loaded directly (without `implement-to-pixel` as the parent), check the input. If it's a PRD and the user is asking for code, run the workflow below. If it's Figma, redirect the user to attach via `implement-to-pixel`.

## The five visual principles

These are the lens you use to imagine the screen before drawing. Apply them mentally as you read the PRD — the wireframe should reflect them visually.

### 1. Layered, not flat

A Mekari screen is readable as architecture before content. Two background tones alternate across zones — quieter for framing (navigation, page header bands, table headers), brighter for content (work areas, card interiors, table body rows). The eye understands the structure in a single sweep.

When you draw the wireframe, show this layering. Don't make everything one color.

### 2. Quiet by default, loud only when needed

Color is signal, not decoration. Brand color belongs to the primary action. Status colors belong to status indicators. Backgrounds stay neutral. Most of the screen is calm, so the few things that matter pop.

When you draw the wireframe, reserve color for the one or two things on the screen that genuinely deserve attention.

### 3. Accountability is visible

Status fields don't just say "Approved" — they say "Approved by [name], [timestamp]". Trust comes from showing who and when, not hiding it. Essential for finance/HR/payroll/tax tools where audit-ability is built in.

When you imagine a screen with status data, include actor and timestamp in your mental picture.

### 4. Density with breathing room

Mekari is a workplace, not a feed. Users scan hundreds of rows in a sitting. Pack the information, but don't pack the eye. Generous-but-not-luxurious spacing. Rows over cards. Right-aligned numbers.

**No cards for index lists or form sections.** Cards add visual weight and consume vertical space. Index lists use table rows — flat, bordered, scannable. Form sections use dividers and sub-headings — not boxed containers. Cards are reserved for summary/highlight content (like a package summary above a table), not for repeating list items or grouping form fields.

When you imagine the layout, lean dense — multiple rows visible without scrolling, but each row legible.

### 5. Frames hold, content does

Different zones have different jobs. Frames (nav, page header, table headers) orient the user. Content (work area, body rows, card interiors) is where work happens. Frames are quieter; content is brighter. Active state lights up the selected item in the frame.

When you draw the wireframe, make the frame-vs-content distinction visible.

## Workflow

### Step 1: Read the PRD completely

- If Confluence URL → fetch and read the full page
- If pasted text or chat description → use that directly

Capture: goal, user story, scope (in/out), functional requirements, acceptance criteria, copy already specified (labels, helper text, error messages, empty state copy), explicit edge cases.

Detect the product (Expense, Talenta, Qontak, etc.) from logo references, breadcrumb, space name, or repeated mentions. Ask only if no signal is present anywhere.

### Step 2: Identify screens

A PRD often describes multiple screens (list page + detail page + create form, etc.). List each distinct screen. The output will be one wireframe per screen.

### Step 3: For each screen, identify Mekari patterns

Match what the screen does to available pattern references in `references/`:

- Page showing one record with many fields → `detail-view.md`
- Page listing many records → `index-view.md`
- Page for creating/editing a record → `form-view.md`
- "No data yet" state → `empty-state.md`
- File upload UI → `upload-flow.md`
- Multi-row table actions → `bulk-select.md`
- Filter or search toolbar → `filter.md`
- Destructive confirmation → `confirmation.md`

Every screen also uses `layout-shell.md`. Read only the references for patterns actually present. Use `references/foundations.md` for token names.

### Step 3.5: Validate against Pixel MCP before planning

After identifying which patterns and components apply, validate each against the Pixel MCP **before** drafting the plan or generating any code. This ensures reference files don't silently go stale.

| What to validate | MCP call |
|---|---|
| Page-level pattern (index, form, detail, pagination, user-account…) | `get-pattern("<slug>")` |
| Any UI component (button, input, table, modal, popover, badge…) | `get-component("<name>")` |
| Any icon name | `get-icon-name("<name>")` |
| Full page template | `get-template("<name>")` |

If MCP output conflicts with a reference file → **trust MCP**. Note the discrepancy in the plan and flag it to the user so the reference file can be updated.

### Step 4: Imagine the screen using the five principles

Before drawing, think through each screen in order:

1. **Layout shell** — Where does the nav sit? What's in the page header area? Where does the content area start?
2. **Primary pattern** — What's the main content? Detail key-value list, index table, or form?
3. **Nested patterns** — Filter toolbar? Empty state? File upload? Bulk select?
4. **States** — What does this screen look like when data is empty, loading, errored, or has null values? The PRD usually shows only the happy path; fill the rest from Mekari defaults.
5. **Copy gaps** — What labels, helper text, error messages, empty state copy is missing from the PRD?
6. **Accountability** — Any status fields that need actor + timestamp?

### Step 5: Draw the wireframe

Use the visualizer skill (`visualize:show_widget` with SVG mode) to render an SVG wireframe of each screen. The wireframe must show:

- Three distinct background tones to convey frame-vs-content: page background, content area, and frame zones (top bar, nav rail, page header). Use Mekari's surface color for frames, white for content area.
- Top bar with product logo placeholder and right-side cluster (user, notifications)
- Nav rail with abbreviated nav items (active item highlighted)
- Page header area with breadcrumb (if any), H1, status pill (if any), primary CTA (top-right)
- Main content (table rows, key-value list, form fields, etc.) at low fidelity — boxes and labels, not real content
- Annotations for states filled in by default (e.g. small note "empty state: illustrated, will appear when no records")
- Annotations for gaps the user should confirm (e.g. small note "needs PM input: max selection limit")

Keep the wireframe at sketch fidelity — the goal is to validate structure and pattern choice, not visual polish. Use solid neutral colors for layering, brand color sparingly for the one or two things that matter (primary CTA, active nav, status pill).

Wireframe sizing: SVG width 680px (viewBox `0 0 680 H`), height as needed. Render one wireframe per screen.

### Step 6: Draft the implementation plan

After all wireframes are rendered, write a structured plan (in conversational markdown text):

```
## Plan for [Product] — [Feature/Flow name]

### Screens identified
1. [Screen 1 name] — patterns: [list]
2. [Screen 2 name] — patterns: [list]
...

### Per screen

#### Screen 1: [name]
**Patterns**: layout-shell + [primary] + [nested]
**Pixel components to use**: [list main ones — MpButton, MpTable, MpInput, etc.]
**States included**: happy path + [empty, loading, error, null as applicable]
**Copy defaults applied** (PRD didn't specify):
  - [list, bolded keys the user might want to edit]
**Conventions applied** (Mekari taste decisions):
  - [list anything important — e.g. "status shown as actor + timestamp"]
**Open gaps for user input**:
  - [list anything that's a real PM/design decision, not a default]

[Repeat per screen]

### Next step
If the plan and wireframes look right, say "looks good" or "go ahead" and `implement-to-pixel` will generate Vue/Nuxt code from this plan. If anything needs adjustment, tell me what to change — I can revise the plan or redraw a wireframe before code generation.
```

### Step 7: Wait for user confirmation

After rendering wireframes and the plan, **stop and wait**. Do not generate code. Do not call `implement-to-pixel`.

The user will either:
- **Confirm** ("looks good", "go ahead", "yes proceed") → control returns to `implement-to-pixel` to take the plan and generate code
- **Request changes** ("change the empty state", "use a different pattern for screen 2", "add X") → revise the plan, redraw affected wireframes, present again
- **Ask questions** → answer using these principles and references; stay paused until user confirms

## What this skill does not do

- Does not generate Vue/Nuxt code. That's `implement-to-pixel`'s job after user confirmation.
- Does not produce high-fidelity mockups. The wireframe is intentionally low-fi for structural validation.
- Does not invent product names, feature names, or copy that the PRD did not provide. If something is missing, mark it as a gap or apply a Mekari default and surface it in the plan.
- Does not run for Figma input. Figma already provides visual structure — `implement-to-pixel` handles it directly.
- Does not run for non-Mekari products.

## Companion files

- `references/foundations.md` — token names (color, spacing, typography, radius)
- `references/layout-shell.md` — top bar, nav rail, page header, content area anatomy
- `references/detail-view.md`, `index-view.md`, `form-view.md`, `empty-state.md`, `upload-flow.md`, `bulk-select.md`, `filter.md`, `confirmation.md` — pattern anatomy

## Companion skills

- **`implement-to-pixel`** — the orchestrator; loads this skill for Branch A, then takes the confirmed plan and generates code
- **`visualize`** — used in Step 5 to render wireframes via `show_widget`
