---
name: implement-to-pixel
description: "Entry point and orchestrator for translating Mekari PRDs or Figma key-page designs into Vue/Nuxt code on Pixel 3 (token 2.4 default). Triggered when the user attaches a Confluence PRD URL, a Figma URL, or invokes /implement-to-pixel, for any Mekari product (Expense, Talenta, Qontak, Jurnal, Flex, KlikPajak, Sign). The skill detects input type and routes accordingly: for PRD input it loads mekari-taste to run an imagination phase (read, visualize, draw wireframes, draft plan, wait for user confirmation) before generating code; for Figma input it loads figma-implement-design to fetch the design and generates code directly without an imagination phase, since the Figma file already provides the visual. Output is always Vue/Nuxt code structured for direct team iteration. Do not use for marketing pages, landing pages, or external-facing editorial content."
---

# Implement to Pixel — Orchestrator

This is the entry point for the Mekari PRD/Figma → Vue/Nuxt code pipeline. It detects what the user attached and routes to the right workflow.

## When this triggers

Any of the following:

- User attaches a Confluence PRD link (`*.atlassian.net/wiki/...`)
- User attaches a Figma URL (`figma.com/design/...` or `figma.com/file/...`)
- User invokes `/implement-to-pixel` with or without an attachment
- User says things like "build this for [Mekari product]", "implement this screen", "code this design" along with an attachment

If the user attaches a URL without specifying intent, assume code generation is the goal — Mekari teams default to this flow.

## Token mode

Default to Pixel 2.4 in all cases. Switch only if the user or the source explicitly mentions Pixel 2.1, Enterprise tokens, or a known legacy project.

## Step 1: Detect input type and route

Look at what was attached:

| Input | Route |
|---|---|
| Confluence URL | **Branch A: PRD path** (with imagination phase) |
| Figma URL | **Branch B: Figma path** (no imagination phase) |
| Both Confluence and Figma URLs | Branch B (Figma is more specific), but read the PRD to fill copy/edge case gaps |
| Pasted PRD text or chat description (no URL) | Branch A |
| `/implement-to-pixel` with no attachment | Ask which input the user has |

## Branch A: PRD path (imagination needed)

The PRD describes intent in text; you have no visual yet. You need to imagine the screen before writing code.

### A1. Load `mekari-taste` and run the imagination phase

Load the `mekari-taste` skill. It owns the entire imagination phase:

1. Read the PRD end-to-end (fetch Confluence if it's a URL)
2. Detect product (Expense, Talenta, Qontak, etc.)
3. Identify screens
4. For each screen, identify Mekari patterns and read the relevant references
5. Apply the five visual principles to imagine the screen
6. Render one wireframe per screen via the visualizer skill
7. Draft a structured implementation plan covering: patterns used, Pixel components, states included, copy defaults, conventions applied, open gaps

The output of `mekari-taste` is shown to the user — wireframes and plan. The skill then stops.

### A2. Wait for user confirmation

`mekari-taste` ends with a request for confirmation. Possible responses:

- **Confirmation** ("looks good", "go ahead", "yes", "proceed", "generate code") → continue to A3
- **Revision request** ("change the empty state", "use a different pattern", "I want to add X") → loop back into `mekari-taste` to revise the plan and redraw affected wireframes; show again; wait for confirmation
- **Question** → answer using `mekari-taste` principles and references; stay paused until user confirms

Do not generate code until the user confirms.

### A3. Generate Vue/Nuxt code from the confirmed plan

For each screen in the confirmed plan:

**Step 0 — Pixel MCP lookup. This is a hard gate before any code.**

For every UI element you are about to write, call the Pixel MCP tool first:

| Tool | When to call |
|---|---|
| `get-component` | Any component — modal, toast, input, button, table, tag, etc. |
| `get-pattern` | Page-level patterns — form view, index view, empty state, etc. |
| `get-icon-name` | Before using any icon name |
| `get-template` | Full-page template scaffolding |

Read the props table and usage example. Then write code using the exact component name, props, and slot structure from MCP. If `get-component` returns nothing, state it explicitly — do not invent.

**Reference token names** from `mekari-taste/references/foundations.md`. Token names by reference, never raw hex.

**Reference pattern anatomy** from `mekari-taste/references/[pattern].md` for each pattern in the plan.

**Build all states from the plan**: happy path plus empty, loading, error, null where listed.

Proceed to A4.

### A4. Hand off

Proceed to the shared "Output structure" section below.

## Branch B: Figma path (no imagination needed)

The Figma file is the source of truth for visual structure. There's no need to imagine — translate what's there.

### B1. Fetch the Figma design

Load `figma-implement-design`. Extract `fileKey` and `nodeId` from the URL, then:

- `get_design_context(fileKey, nodeId)` — structure, layout, tokens used
- `get_screenshot(fileKey, nodeId)` — visual reference
- `get_variable_defs(fileKey, nodeId)` — confirm token mode (2.4 expected)

For large nodes, use `get_metadata` first, then drill into specific children.

### B2. Detect product and confirm token mode

Read the product from the top bar logo lockup in the screenshot ("mekari expense", "mekari talenta", "mekari qontak", etc.) or from the Figma file name. Ask only if neither is clear.

If variable defs return Pixel 2.1 token shape (`$gray-600`, `Spacing/xs`), inform the user and ask whether to translate to 2.4 or keep 2.1.

### B3. Identify patterns present in the Figma

Look at the screenshot and match against the available pattern references in `mekari-taste/references/`:

- Breadcrumb + H1 + status pill + key-value list → `detail-view.md`
- Toolbar + table + pagination → `index-view.md`
- Fields + section dividers + footer action → `form-view.md`
- "Nothing here yet" frame → `empty-state.md`
- File upload control → `upload-flow.md`
- Multi-row checkboxes + action bar → `bulk-select.md`
- Filter dropdowns or filter drawer → `filter.md`
- Confirmation modal → `confirmation.md`

Read only the references for patterns actually present. Also read `references/foundations.md` for token names. **Do not load `mekari-taste` itself** — the Figma file replaces the imagination phase. The pattern references and foundations carry enough taste guidance for the Figma path.

### B4. Identify states Figma doesn't show

Designer key pages typically show only the happy path. Walk through each pattern's "Edge cases the PRD probably forgot" section AND specifically check for state gaps that Figma key pages omit:

- Hover, active, disabled, focus states for interactive elements
- Loading state (skeleton rows, spinner, shimmer)
- Empty state — is there a frame for "no data yet"? If not, build one
- Error states — failed API call, failed form submit, validation errors
- Null/missing values in detail rows and table cells
- Permission-gated rows or fields
- Conditional fields and sub-sections — what triggers them
- Sticky behavior on scroll

**Use Mekari defaults from the pattern references for all gaps. Do not pause to ask.** Document defaults prominently in the output header so the team can override during code iteration.

### B5. Generate Vue/Nuxt code

For each pattern in use, walk through its anatomy in `mekari-taste/references/[pattern].md` and translate into Vue. Use Pixel MCP tools as in A3 — never guess prop names.

### B6. Validate against the screenshot

Compare the rendered output mentally against the Figma screenshot for layout/typography/color match. Note deliberate deviations (e.g. "Figma used a custom shadow; replaced with flat-card Mekari convention").

### B7. Hand off

Proceed to the shared "Output structure" section below.

## Output structure (both branches)

The deliverable replaces handoff. The team will iterate on this code directly, so structure it for that.

### Section dividers

Inside the template:

```vue
<!-- ═════ Layout shell ═════ -->
<!-- ═════ Page header ═════ -->
<!-- ═════ Filter toolbar ═════ -->
<!-- ═════ Data table ═════ -->
<!-- ═════ Empty state ═════ -->
```

### Toast notifications

- Always use `toast.notify()` from `@mekari/pixel3`. Never build a custom overlay or Teleport div.
- Import: `import { toast } from '@mekari/pixel3'`
- Position: **`top-center`** — horizontally centered on the full screen.
- Duration: **`3000`** ms for `success` and `error`. Do not omit — Pixel's default is not guaranteed.
- Variants: `success` (green), `error` (red), `greeting` (blue).

```ts
toast.notify({ id: 'page-toast', position: 'top-center', variant: 'success', title: 'Team created successfully.', duration: 3000 })
```

### Constants at the top of `<script setup>`

Extract copy strings, helper text, field labels, status labels as named constants. Iteration becomes "change one line" not "find the string in three places".

### Sub-components for states

If empty state, loading skeleton, or error banner exceed ~15 lines, extract as small components.

### Token names inline, never hex

Use Pixel token names as CSS variables or component props. Inline comments explain non-obvious decisions.

## Multi-screen handling

If the plan or Figma has multiple screens, generate them in order. Each screen is one Vue file. Consider extracting shared sub-components (status pill, action footer) when used 3+ times.

## What this skill does not do

- Does not produce wireframes or imagination output — that's `mekari-taste`'s job (Branch A only)
- Does not implement backend, API contracts, or routing — frontend component only
- Does not skip the imagination phase for PRD input — always load `mekari-taste` for Branch A
- Does not load `mekari-taste` for Figma input — Branch B skips it entirely

## Companion skills

- **`mekari-taste`** — imagination phase for Branch A only (read PRD, draw wireframes, draft plan)
- **`figma-implement-design`** — Figma fetching for Branch B only
- **`pixel`** — Pixel 3 component prop API for both branches
- **`visualize`** — used by `mekari-taste` to render wireframes (not called directly by this skill)

## Companion file references

These live in `mekari-taste/references/` and are loaded directly by this skill in both branches:

- `foundations.md` — token names
- `layout-shell.md`, `detail-view.md`, `index-view.md`, `form-view.md`, `empty-state.md`, `upload-flow.md`, `bulk-select.md`, `filter.md`, `confirmation.md` — pattern anatomy
