# Components Catalog

Complete reference of Pixel components with mapping from Figma elements and common usage patterns.

## 1. Component Mapping (if working with Figma designs)

Map Figma elements to Pixel components for implementation:

| Figma Element  | Pixel Component             | Use Case                             |
| -------------- | --------------------------- | ------------------------------------ |
| Text/Heading   | `<MpText>`                  | All text and headings                |
| Icon           | `<MpIcon>`                  | Icons and visual indicators          |
| Button         | `<MpButton>`                | Primary, secondary, tertiary actions |
| Text Link      | `<MpTextlink>`              | Inline and standalone links          |
| Input Field    | `<MpInput>`                 | Text inputs, email, password fields  |
| Dropdown       | `<MpSelect>`                | Select dropdowns and pickers         |
| Checkbox/Radio | `<MpCheckbox>`, `<MpRadio>` | Boolean and option selection         |
| Modal/Dialog   | `<MpModal>`                 | Overlay dialogs and confirmations    |
| Drawer         | `<MpDrawer>`                | Side panels and navigation           |
| Flex Layout    | `<MpFlex>`                  | Flexible layouts with gap/alignItems |
| Box Layout     | `<Pixel.div>`               | Custom containers with tokens        |

## 2. Prop Validation Guidelines

**⚠️ CRITICAL: Always validate prop values against Pixel documentation**

Many TypeScript errors come from using invalid prop values. Before using ANY component:

1. Use `get-component` MCP tool to retrieve documentation
2. Check the actual prop types and valid values
3. Never assume generic values like `"md"`, `"xl"`, `"sm"` will work

### Common Invalid Props to Avoid

#### ❌ MpText - Invalid size values

```vue
<!-- WRONG: These will cause TypeScript errors -->
<MpText size="md">Text</MpText>
<MpText size="xl">Large text</MpText>
<MpText size="sm">Small text</MpText>
<MpText size="lg">Text</MpText>
```

#### ✅ MpText - Valid size values

```vue
<!-- CORRECT: Use documented size values -->
<MpText size="h1">Heading 1</MpText>
<MpText size="h2">Heading 2</MpText>
<MpText size="h3">Heading 3</MpText>
<MpText size="label">Label text (default)</MpText>
<MpText size="label-small">Small label</MpText>
<MpText size="body">Body text</MpText>
<MpText size="body-small">Small body text</MpText>
<MpText size="overline">Overline text</MpText>
<MpText>Text with default size</MpText>
<!-- Omit prop for default -->
```

#### ❌ MpIcon - Invalid icon names

```vue
<!-- WRONG: These icons don't exist in Pixel -->
<MpIcon name="sort" />
<MpIcon name="document" />
<MpIcon name="text-editor" />
<MpIcon name="setting" />
<!-- Missing 's' -->
```

#### ✅ MpIcon - Valid icon names

```vue
<!-- CORRECT: Use valid Pixel icon names -->
<MpIcon name="filter" />
<MpIcon name="settings" />
<!-- Note the 's' -->
<MpIcon name="edit" />
<MpIcon name="help" />
<MpIcon name="info" />
<MpIcon name="search" />
<MpIcon name="delete" />
```

#### ❌ MpProgress - Invalid value type

```vue
<!-- WRONG: Number instead of string -->
<MpProgress :value="82" />
```

#### ✅ MpProgress - Valid value type

```vue
<!-- CORRECT: String value -->
<MpProgress value="82" />
```

### Validation Workflow

1. **Get component docs**: `get-component("MpText")` → Check props table
2. **Verify prop types**: Look for valid values in documentation examples
3. **Use TypeScript hints**: If TypeScript complains, the value is invalid
4. **When in doubt**: Omit optional props, use defaults

## 3. Common Usage Patterns

### Form with Validation

```vue
<template>
  <MpFlex direction="column" gap="4">
    <MpFormControl id="email-input" :is-error="isError" is-required>
      <MpFormLabel>Email</MpFormLabel>
      <MpInput v-model="email" type="email" placeholder="name@company.com" />
      <MpFormErrorMessage>You must fill in email</MpFormErrorMessage>
      <MpFormHelpText>The email field is required</MpFormHelpText>
    </MpFormControl>
    <MpButton variant="primary" size="md" @click="onSubmit">Submit</MpButton>
  </MpFlex>
</template>

<script setup lang="ts">
import { ref } from "vue";
import {
  MpButton,
  MpFlex,
  MpFormControl,
  MpFormLabel,
  MpFormHelpText,
  MpFormErrorMessage
  MpInput,
} from "@mekari/pixel3";

const email = ref("");
const isError = ref(false);

const onSubmit = () => {
  console.log("Form submitted:", email.value);
};
</script>
```

### Card Layout

```vue
<template>
  <MpFlex direction="column" gap="4">
    <MpText size="h3" color="text.primary">Card Title</MpText>
    <MpText size="body" color="text.secondary">
      Card description text goes here
    </MpText>
    <MpButton variant="outline" size="sm" @click="onAction">Action</MpButton>
  </MpFlex>
</template>

<script setup lang="ts">
import { MpButton, MpFlex, MpText } from "@mekari/pixel3";

const onAction = () => {
  console.log("Action clicked");
};
</script>
```

### Modal Dialog

```vue
<template>
  <MpButton @click="onOpenModal">Open Modal</MpButton>

  <MpModal v-model="isOpen">
    <MpModalHeader>
      Modal title goes here

      <MpModalCloseButton />
    </MpModalHeader>
    <MpModalContent>
      <MpModalBody>
        <MpText>Modal content goes here</MpText>
      </MpModalBody>
      <MpModalFooter>
        <MpButtonGroup>
          <MpButton variant="secondary" @click="onClose"> Cancel </MpButton>
          <MpButton> Approve </MpButton>
        </MpButtonGroup>
      </MpModalFooter>
    </MpModalContent>
  </MpModal>
</template>

<script setup lang="ts">
import { ref } from "vue";
import {
  MpButton,
  MpModal,
  MpModalContent,
  MpModalHeader,
  MpModalCloseButton,
  MpModalBody,
  MpModalFooter,
  MpText
} from "@mekari/pixel3";

const isOpen = ref(false);

const onOpenModal = () => {
  isOpen.value = true;
};
</script>
```

### Icon with Text

```vue
<template>
  <MpButton left-icon="search" variant="primary" size="md" @click="onSearch">
    Search
  </MpButton>

  <MpFlex alignItems="center" gap="2">
    <MpIcon name="delete" />
    <MpText color="text.danger">Delete item</MpText>
  </MpFlex>
</template>

<script setup lang="ts">
import { MpButton, MpFlex, MpIcon, MpText } from "@mekari/pixel3";

const onSearch = () => {
  console.log("Search clicked");
};
</script>
```

## 4. Additional Rules

### ✅ ALWAYS

- Use `<MpIcon>` for all icons, even when Figma provides custom SVG assets.
- Prefer the closest matching Pixel icon name instead of importing or inlining SVGs.

### ❌ NEVER

- Use raw HTML elements (e.g., `<div>`, `<span>`, `<button>`) instead of Pixel components.
- Use icons from Figma assets, inline SVGs, or external icon packs.
