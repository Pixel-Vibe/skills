# Design Tokens

Semantic values for colors, spacing, and typography. Always use Design Token 2.4 instead of hardcoded values.

## 1. Colors

### Text Colors

```vue
<MpText>Default text</MpText>
<MpText color="text.primary">Primary text</MpText>
<MpText color="text.secondary">Secondary text</MpText>
<MpText color="text.warning">Warning text</MpText>
<MpText color="text.danger">Danger text</MpText>
<MpText color="text.success">Success text</MpText>
```

### Background Colors

```vue
<MpFlex bg="background.surface">Surface Background</MpFlex>
<MpFlex bg="background.neutral">Neutral Background</MpFlex>
<MpFlex bg="background.neutral.bold">Neutral Background but bolder</MpFlex>
```

### Border Colors

```vue
<Pixel.div borderColor="border.default" borderWidth="1px" borderStyle="solid">
  Default border color
</Pixel.div>
```

## 2. Spacing Scale

Use semantic spacing tokens instead of hardcoded values:

| Semantic | Token        | Value | Use Case                         |
| -------- | ------------ | ----- | -------------------------------- |
| `4xs`    | `0.125`      | 2px   | Tight spacing, compact layouts   |
| `3xs`    | `spacing.1`  | 4px   | Tight spacing, compact layouts   |
| `2xs`    | `0.375`      | 6px   | Tight spacing, compact layouts   |
| `xs`     | `spacing.2`  | 8px   | Tight spacing, compact layouts   |
| `sm`     | `spacing.3`  | 12px  | Default spacing between elements |
| `md`     | `spacing.4`  | 16px  | Default spacing between elements |
| `lg`     | `spacing.5`  | 20px  | Section spacing                  |
| `xl`     | `spacing.6`  | 24px  | Large component spacing          |
| `2xl`    | `spacing.8`  | 32px  | Major section separation         |
| `3xl`    | `spacing.10` | 40px  | Page-level spacing               |
| `4xl`    | `spacing.20` | 80px  | Page-level spacing               |

```vue
<MpFlex direction="column" gap="3xs" padding="2xs" margin="xl">
  <MpText>Title</MpText>
  <MpText>Content</MpText>
</MpFlex>
```

## 3. Typography

### Text Sizes

When use regular text with 14px font size, simply use `<MpText>` without size prop:

```vue
<MpText>Regular text - 14px</MpText>
```

If need to specify size, use the following props:

```vue
<!-- Headings -->
<MpText size="h1">Heading 1 - 24px</MpText>
<MpText size="h2">Heading 2 - 20px</MpText>
<MpText size="h3">Heading 3 - 16px</MpText>

<!-- Regular text -->
<MpText size="label">Regular label text - 14px</MpText>
<MpText size="label-small">Label small text - 12px</MpText>
<MpText size="body">Body with large line height text - 14px</MpText>
<MpText size="body-small">Body small text - 12px</MpText>
<MpText size="overline">Overline - 10px</MpText>

<!-- Truncate -->
<MpText line-clamp="2" is-truncated>
  Lorem ipsum dolor sit amet, consectetur adipisicingelit. Atque consequatur dignissimos ducimus, ea eaque et excepturi iusto, laboriosam nemo
  omnis perferendis praesentium provident quae reiciendis repellendus sunt unde voluptatem voluptates.
</MpText>
```

### Font Weights

```vue
<MpText weight="regular">Regular - 400</MpText>
<MpText weight="semiBold">Semibold - 500</MpText>
```

## 4. Token Resources

Access token documentation via Pixel MCP tools:

- Use `get-docs` with query "design tokens 2.4" or "design tokens 2.1"
- Use `get-docs` with query "semantic color tokens" or "spacing tokens"

## Token Priority Order

**Always follow this hierarchy when selecting tokens:**

1. **Design Token 2.4** (preferred) - Use these if the application uses Design Token 2.4

```vue
<MpText color="text.warning">use semantic token</MpText>
```

2. **Design Token 2.1** (fallback) - Use these if the application uses Design Token 2.1

```vue
<MpText color="orange.400">use foundation colors</MpText>
```

3. **Exact hex from Figma** (last resort) - Only when no semantic token or foundation token available

```vue
<MpText color="#E0AB00">use hex colors</MpText>
```
