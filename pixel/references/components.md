# Components

Use this file to map UI needs to Pixel components, then verify uncertain props with `get-component`.
if component not found in `get-component`, use `get-pattern` instead.

## Component Mapping

### Airene / AI

| Figma Component    | Pixel Component      | Notes                                      |
| ------------------ | -------------------- | ------------------------------------------ |
| Airene Button      | `MpAireneButton`     | Button component for Mekari Airene product |
| Airene Chat Bubble | `MpAireneChatBubble` | Chat Bubble component for Airene           |
| Airene Chat Input  | `MpAireneChatInput`  | Chat Input component for Airene            |

### Components A–F

| Figma Component | Pixel Component                | Notes                                                                                  |
| --------------- | ------------------------------ | -------------------------------------------------------------------------------------- |
| Accordion       | `MpAccordion` + sub-components | Expandable/collapsible content sections                                                |
| Autocomplete    | `MpAutocomplete`               | Input with filtered suggestion dropdown                                                |
| Avatar          | `MpAvatar`, `MpAvatarGroup`    | Displays user image, initials, or icon; supports group stacking                        |
| Badge           | `MpBadge`                      | Small status indicator; use documented `type` and `variant` props only                 |
| Banner          | `MpBanner`                     | Displays a prominent message and related optional actions                              |
| Broadcast       | `MpBroadcast`                  | Full-width announcement bar for announcements and alerts                               |
| Button          | `MpButton`, `MpButtonGroup`    | Triggers actions when clicked, always verify `variant` and`size`props                  |
| Carousel        | `MpCarousel`                   | Cycles through a series of items or slides                                             |
| Chart           | `MpChart`                      | Renders bar, line, pie, doughnut, radar, area, gauge, and mix charts                   |
| Checkbox        | `MpCheckbox`                   | Allows user to toggle between checked, unchecked, and indeterminate states             |
| Collapse        | `MpCollapse`                   | Toggles visibility of content with animation                                           |
| Color Picker    | `MpColorPicker`                | Lets user select or input a color value                                                |
| Date Picker     | `MpDatePicker`                 | Calendar-based input for selecting dates or date ranges                                |
| Divider         | `MpDivider`                    | Separates content horizontally or vertically                                           |
| Drawer          | `MpDrawer` + sub-components    | Panel that slides out from the screen edge without leaving the current page            |
| Dropzone        | `MpDropzone`                   | File upload area with drag-and-drop support                                            |
| Flex            | `MpFlex` / `Pixel.div`         | One-dimensional layout model for arranging children, Prefer these over raw layout HTML |
| Form control    | `MpFormControl`                | Wrapper that passes id, disabled, invalid, and required state to form fields           |

### Components I–R

| Figma Component  | Pixel Component                         | Notes                                                                                       |
| ---------------- | --------------------------------------- | ------------------------------------------------------------------------------------------- |
| Icon             | `MpIcon`                                | SVG icon; always verify name with `get-icon-name` — 403 icons available                     |
| Image            | `MpImage`                               | Renders images with layout, lazy loading, srcset, and placeholder support                   |
| Input            | `MpInput`                               | Text input with variants, sizes, clearable, and group addon support wrap in `MpFormControl` |
| Input Tag        | `MpInputTag`                            | Multi-value tag input with suggestions, free-tag, and infinity scroll                       |
| Loader           | `MpLoader` / `MpSkeleton` / `MpSpinner` | Use Skeleton for content placeholder, Spinner for in-progress, Loader for page-level        |
| Modal            | `MpModal` + sub-components              | Overlay dialog with scroll behavior, size, and focus management                             |
| Popover          | `MpPopover` + sub-components            | Provides supplemental information or action lists anchored to a trigger element             |
| Progress         | `MpProgress`                            | Linear or circular progress indicator                                                       |
| Radio            | `MpRadio`                               | Represents a single option in a mutually exclusive list                                     |
| Rating           | `MpRating`                              | Displays and captures star or heart ratings with optional value description                 |
| Rich text editor | `MpRichTextEditor`                      | Full-featured content editor based on Tiptap.js with mention, attach, and format tools      |

### Components S–Z

| Figma Component   | Pixel Component            | Notes                                                                     |
| ----------------- | -------------------------- | ------------------------------------------------------------------------- |
| Scrollbar         | `MpScrollbar`              | Custom scrollbar component                                                |
| Segmented control | `MpSegmentedControl`       | Alternative to a radio group; supports icon-only, label, or both          |
| Select            | `MpSelect`                 | Dropdown select; wrap in `MpFormControl` for label and validation         |
| Skeleton          | `MpSkeleton`               | Loading placeholder that mirrors content shape; supports color and timing |
| Slider            | `MpSlider`                 | Range input with label, min/max, step, gradient color, and custom slots   |
| Spinner           | `MpSpinner`                | Indicates loading state of a component or page                            |
| Table             | `MpTable` + sub-components | Data table with fixed header, borders, hover, and shadow container        |
| Tabs              | `MpTabs` + sub-components  | Tabbed navigation for switching between related content panels            |
| Tag               | `MpTag`                    | Label that can optionally be selected, unselected, or removed             |
| Text              | `MpText`                   | Typography component; verify valid `size` values before use               |
| Textarea          | `MpTextarea`               | Multi-line text input; wrap in `MpFormControl` for validation             |
| Textlink          | `MpTextlink`               | Button styled as an inline text link                                      |
| Timeline          | `MpTimeline`               | Displays a list of events in chronological order with status indicators   |
| Toast             | `toast` (function)         | Transient notification with position, variant, and duration options       |
| Toggle            | `MpToggle`                 | On/off switch as an alternative appearance to a checkbox                  |
| Tooltip           | `MpTooltip` / `v-tooltip`  | Shows supplemental label on hover; use for supporting context only        |
| Tour              | `MpTour`                   | Step-by-step product tour that highlights UI elements                     |
| Upload            | `MpUpload`                 | File input with upload list, preview, loading state, and validation       |

---

## Validation Rules

1. Run `get-component("ComponentName")` before choosing unfamiliar props.
2. Prefer documented defaults instead of force-setting every prop.
3. Resolve TypeScript errors by checking docs first, not by force-casting.
4. Keep field labeling, help text, and error messaging explicit.

## Common Traps

- Do not assume generic size values like `sm`, `md`, `lg`, or `xl`.
- Do not assume arbitrary icon names exist in the Pixel icon set.
- Do not pass numbers where the component expects strings.
- Do not skip `MpFormControl` when the field needs validation or error messaging.

## Examples

### Validated Input Field

```vue
<MpFormControl id="email" :is-error="isError" is-required>
  <MpFormLabel>Email</MpFormLabel>
  <MpInput v-model="email" type="email" placeholder="name@company.com" />
  <MpFormErrorMessage>Email is required</MpFormErrorMessage>
  <MpFormHelpText>Use company's email</MpFormHelpText>
</MpFormControl>
```

### Modal Action Flow

```vue
<MpModal>
  <MpModalContent>
    <MpModalHeader>
      Confirm action
      <MpModalCloseButton />
    </MpModalHeader>
    <MpModalBody>
      <MpText>Review the impact before continuing.</MpText>
    </MpModalBody>
    <MpModalFooter>
      <MpButtonGroup>
        <MpButton variant="secondary">Cancel</MpButton>
        <MpButton variant="primary">Confirm</MpButton>
      </MpButtonGroup>
    </MpModalFooter>
  </MpModalContent>
  <MpModalOverlay />
</MpModal>
```
