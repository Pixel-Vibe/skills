# Code Structure

Code structure for Vue SFC block order, script setup code order, and TypeScript best practices.

## 1. Vue SFC Block Order

**CRITICAL:** In all `.vue` files, the `<template>` block **must** be defined first, before the `<script setup>` block.

```vue
<!-- ✅ Correct Order -->
<template>
  <!-- Template content -->
</template>

<script setup lang="ts">
// Script content
</script>
```

```vue
<!-- ❌ Wrong Order - NEVER DO THIS -->
<script setup lang="ts">
// This is incorrect!
</script>

<template>
  <!-- Template content -->
</template>
```

## 2. Script Setup Code Order

Code within `<script setup lang="ts">` **must** follow this order:

1. **Imports** - Vue core, composables, and components
2. **Props** - `defineProps` with TypeScript interface
3. **Emits** - `defineEmits` with event types
4. **Reactive state** - `ref`, `reactive`, `shallowRef`
5. **Computed properties** - `computed`
6. **Watchers** - `watch`, `watchEffect`
7. **Lifecycle hooks** - `onMounted`, `onBeforeUnmount`, etc.
8. **Functions** - Event handlers and utility methods

### Complete Example

```vue
<template>
  <MpFlex direction="column" gap="4">
    <MpText size="h2">{{ computedTitle }}</MpText>
    <MpInput
      id="input"
      v-model="inputValue"
      placeholder="Enter value"
      :is-error="isError"
    />
    <MpButton variant="primary" @click="onSubmit"> Submit </MpButton>
  </MpFlex>
</template>

<script setup lang="ts">
// 1. Imports
import { ref, computed, watch, onMounted } from "vue";
import { MpFlex, MpText, MpInput, MpButton } from "@mekari/pixel3";

// 2. Props
interface Props {
  title: string;
  maxLength?: number;
}

const props = withDefaults(defineProps<Props>(), {
  maxLength: 100
});

// 3. Emits
interface Emits {
  submit: [value: string];
  error: [message: string];
}

const emit = defineEmits<Emits>();

// 4. Reactive state
const inputValue = ref("");
const isError = ref(false);

// 5. Computed properties
const computedTitle = computed(() => {
  return props.title.toUpperCase();
});

const isValid = computed(() => {
  return (
    inputValue.value.length > 0 && inputValue.value.length <= props.maxLength
  );
});

// 6. Watchers
watch(inputValue, (newVal) => {
  isError.value = newVal.length > props.maxLength;

  if (isError.value) {
    emit("error", `Maximum ${props.maxLength} characters allowed`);
  }
});

// 7. Lifecycle hooks
onMounted(() => {
  console.log("Component mounted");
  // Initialize state or fetch data
});

// 8. Functions
const onSubmit = () => {
  if (!isValid.value) {
    isError.value = true;
    return;
  }

  emit("submit", inputValue.value);
  inputValue.value = ""; // Reset
};
</script>
```

## 3. Import Organization

Group imports by category with blank lines between:

```typescript
// 1. Vue core
import { ref, computed, watch, onMounted } from "vue";

// 2. Composables and utilities
import { usePixelTheme } from "@mekari/pixel3";
import { useRouter } from "vue-router";

// 3. Components (grouped alphabetically)
import { MpButton, MpFlex, MpInput, MpText } from "@mekari/pixel3";

// 4. Types
import type { FormState, ValidationError } from "@/types";
```

## 4. TypeScript Best Practices

### Props with Defaults

```typescript
interface Props {
  title: string;
  variant?: "primary" | "secondary";
  isDisabled?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  variant: "primary",
  isDisabled: false
});
```

### Typed Emits

```typescript
interface Emits {
  update: [id: string, value: unknown];
  delete: [id: string];
  close: [];
}

const emit = defineEmits<Emits>();

// Usage
emit("update", "123", { name: "John" });
emit("delete", "456");
emit("close");
```

### Ref Types

```typescript
// Explicit typing
const count = ref<number>(0);
const user = ref<User | null>(null);
const items = ref<string[]>([]);

// Type inference (when initial value is clear)
const isOpen = ref(false); // inferred as Ref<boolean>
const message = ref(""); // inferred as Ref<string>
```
