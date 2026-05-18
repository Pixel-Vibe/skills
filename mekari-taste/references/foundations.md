# Foundations — Pixel 2.4 tokens

Single source of truth for token names and values. Pattern references and the orchestrator skill (`implement-to-pixel`) refer to this file by token name — never by hex value or raw number.

This file is **role-based**, not zone-based. It says what each token _means_, not where each token _goes_. For "where" — which zone uses which token — see the pattern reference for that pattern.

## Token mode

Default: **Pixel 2.4**.

Switch only when the user explicitly mentions "Pixel 2.1", "Enterprise tokens", "the old token names", or names a legacy project on 2.1. Return to 2.4 default for any new task that doesn't mention tokens.

All values below are 2.4. If outputting 2.1 after explicit request, translate from these names — don't silently mix.

---

## Color

Two background tones, used in alternating zones. Pattern references decide which zone gets which.

| Token                      | Hex       | Role                                                                                                     |
| -------------------------- | --------- | -------------------------------------------------------------------------------------------------------- |
| `Color/Background/surface` | `#F1F5F9` | The quieter background. Used for framing zones — wayfinding, headers above tables, structural surrounds. |
| `Color/Background/stage`   | `#FFFFFF` | The brighter background. Used for content surfaces — where the user acts, reads details, or fills forms. |

### Text colors

| Token                  | Hex       | Role                                                                       |
| ---------------------- | --------- | -------------------------------------------------------------------------- |
| `Color/Text/default`   | `#272B32` | Primary text — most labels, values, body copy                              |
| `Color/Text/secondary` | `#656F80` | Secondary text — meta, helpers, captions, labels paired with louder values |
| `Color/Text/link`      | `#4B61DC` | Text links and brand-colored text                                          |
| `Color/Text/selected`  | `#4B61DC` | Text in active/selected state (nav item, tab)                              |
| `Color/Text/inverse`   | `#FFFFFF` | Text on a dark/colored background (e.g. primary button label)              |

### Border and icon colors

| Token                   | Hex       | Role                                             |
| ----------------------- | --------- | ------------------------------------------------ |
| `Color/Border/default`  | `#DCDFE4` | Default border for inputs, cards, table dividers |
| `Color/Border/selected` | `#4B61DC` | Border in active/focus state                     |
| `Color/Icon/default`    | `#656F80` | Default icon color (matches secondary text)      |
| `Color/Icon/brand`      | `#4B61DC` | Icon in active/selected state                    |

### Action and brand colors

| Token                             | Hex       | Role                                                                   |
| --------------------------------- | --------- | ---------------------------------------------------------------------- |
| `Color/Background/brand-bold`     | `#4B61DC` | Primary action background (solid brand button)                         |
| `Color/Background/brand-selected` | `#D6DBF7` | Selected item background (active nav, active tab tint, chip selection) |

### Status colors (use sparingly, as signal)

| Token                           | Hex       | Role                                              |
| ------------------------------- | --------- | ------------------------------------------------- |
| `Color/Background/success-bold` | `#1C8459` | Success indicators (status dots, pill text)       |
| `Color/Background/danger-bold`  | `#C33E35` | Danger/error indicators, required-field asterisks |
| `Color/Background/warning-bold` | `#956400` | Warning indicators (yellow family)                |

For status pill background fills, use lightened variants of the bold colors. Specific lightened values are defined in pattern references that use pills (mainly `detail-view.md` and `index-view.md`).

---

## Spacing

Snap every margin, padding, and gap to this scale. Off-scale values are wrong.

| Token           | Value | Role                                                |
| --------------- | ----- | --------------------------------------------------- |
| `pxl-space-4xs` | 2     | Hairline gaps                                       |
| `pxl-space-3xs` | 4     | Very tight gaps, chip padding                       |
| `pxl-space-2xs` | 6     | Tight gaps, label-asterisk spacing                  |
| `pxl-space-xs`  | 8     | Icon-to-label, button internal padding              |
| `pxl-space-sm`  | 12    | Compact rhythm, input padding                       |
| `pxl-space-md`  | 16    | Default rhythm — most field gaps, table row padding |
| `pxl-space-xl`  | 24    | Generous rhythm — card padding, page padding        |
| `pxl-space-3xl` | 40    | Section breaks                                      |
| `pxl-space-4xl` | 80    | Large vertical rest (e.g. empty state padding)      |

Pattern references specify which step applies in which slot.

---

## Typography

Six roles. The system has only two weights: Regular (400) and Semibold (600). No other weights.

| Token                 | Size / weight / line-height             | Role                                                                       |
| --------------------- | --------------------------------------- | -------------------------------------------------------------------------- |
| `Heading/H1`          | 24 / Semibold / 32, letter-spacing -0.2 | Top-level page identifier. One per screen.                                 |
| `Heading/H2`          | 20 / Semibold / 32                      | Section header                                                             |
| `Heading/H3`          | 16 / Semibold / 24                      | Sub-section header                                                         |
| `Label/Semibold`      | 14 / Semibold / 20                      | Emphasis text in dense UI (labels above inputs, table headers, key labels) |
| `Label/Regular`       | 14 / Regular / 20                       | Default body and UI text                                                   |
| `Label small/Regular` | 12 / Regular / 16                       | Secondary text — meta, helper, caption                                     |
| `Overline/Semibold`   | 10 / Semibold / 12, uppercase           | Categorical label above a grouped list                                     |

Font family: **Inter**, applied globally. No serif, no display font.

---

## Radius

| Token            | Value | Role                                                         |
| ---------------- | ----- | ------------------------------------------------------------ |
| `pxl-radii-md`   | 6     | Solid containers — default for buttons, inputs, cards        |
| `pxl-radii-lg`   | 8     | Slightly larger containers — summary cards, prominent panels |
| `pxl-radii-full` | 999   | Pills and circular shapes — status pills, avatars, badges    |

Use `pxl-radii-md` unless a pattern reference specifies otherwise.

---

## Shadows

**Default: none.** Anything in the page flow is flat. Separation between surfaces comes from background contrast and borders, not shadow.

Floating layers may lift. Use the lightest available Pixel elevation token only on:

- Dropdown / select panels
- Modal dialogs
- Tooltip / popover layers
- Toast notifications

Do not use shadows on cards in the page flow, on tables, on form sections, or on any element that sits in the normal document flow.

---

## How to use this file

1. **When writing a pattern reference**: use token names by reference (e.g. "Background uses `Color/Background/surface`"), never hardcode hex values.
2. **When the orchestrator generates code**: read this file to confirm token names, then emit them as CSS variables or Pixel component props.
3. **When in doubt about which token**: pattern references hold the zone-specific decisions. This file holds the meanings.
4. **When something doesn't fit**: check if you're trying to use an off-scale value (spacing 14, font size 13). If so, snap to the nearest token. If the token genuinely doesn't exist, escalate to the design system team — don't invent.
