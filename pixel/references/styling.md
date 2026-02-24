# Styling Hierarchy

How to apply styles with Pixel: CSS Props (primary), CSS Function (secondary), and Design Tokens (all values).

## 1. CSS Props (Primary Method)

Use CSS props with design tokens in components like `gap="sm"`, `backgroundColor="background.surface"`, `padding="4"`

**Use for:** component `MpFlex`, `MpScrollbar`, `MpSkeleton`, and `Pixel` tag components (e.g., `Pixel.div`, `Pixel.span`)

**Important:** Always use full property names, not shorthand:

- Use full prop names: `alignItems`, `justifyContent`, `flexGrow`
- Don't use shorthand props: `align`, `justify`, `grow`, `shrink`, `basis`, `wrap`

```vue
<MpFlex gap="sm" padding="4" backgroundColor="background.surface">
  Hello World
</MpFlex>
```

## 2. CSS Function (Secondary Method)

Use `css()` function for custom styles with design tokens

**Use for:** components that not support CSS Props like `MpButton`, `MpInput`, `MpText`, etc.

```vue
<MpButton :class="css({ marginTop: '4' })">
  Submit
</MpButton>
```

## 3. Use Design Tokens for All Values

Always use Design Token 2.4 semantic tokens instead of hardcoded values.

Refer to [design-tokens.md](./design-tokens.md) for:

- Token priority order (2.4 vs 2.1)
- Color, spacing, typography tokens
- How to enable Design Token 2.4

## 4. Additional Rules

### ❌ NEVER

- Use `css()` function in `MpFlex` or `Pixel.div`
- Use inline `style` attributes (e.g., `style="color: blue;"`)
- Use hard-coded values (e.g., `#FFF`, `16px`, `blue`)
- Use shorthand css flex props: `align`, `justify`, `grow`, `shrink`, `basis`, `wrap`
- Use shorthand css border props: `border`, `borderTop`,

### ❌ WRONG: Using CSS shorthand properties

```vue
<!-- VIOLATION: Never use shorthand properties in css() function -->
<MpDrawerFooter
  :class="css({
    borderTop: '1px solid',
    borderColor: 'border.default'
  })"
>
```

### ✅ CORRECT: Using individual CSS properties

```vue
<!-- Use individual properties with design tokens -->
<MpDrawerFooter
  :class="css({
    borderTopWidth: '1px',
    borderTopStyle: 'solid',
    borderTopColor: 'border.default'
  })"
>
```

### ❌ WRONG: Using css() in MpFlex

```vue
<!-- MpFlex or Pixel.div should ONLY use CSS Props, never css() function -->
<MpFlex direction="column" gap="4" :class="css({ pb: '10' })">
  Content
</MpFlex>

<Pixel.div gap="2" :class="css({ flexWrap: 'wrap', cursor: 'pointer' })">
  Content
</Pixel.div>
```

### ✅ CORRECT: Using CSS Props in MpFlex

```vue
<!-- Use CSS Props directly on MpFlex -->
<MpFlex direction="column" gap="4" pb="10">
  Content
</MpFlex>

<Pixel.div gap="2" flexWrap="wrap" cursor="pointer">
  Content
</Pixel.div>
```

### Rule Summary

| Component Type                        | Styling Method        | Example                                 |
| ------------------------------------- | --------------------- | --------------------------------------- |
| `MpFlex`, `Pixel.div`, `Pixel.*`      | ✅ CSS Props only     | `<MpFlex gap="4" padding="2" flex="1">` |
| `MpButton`, `MpText`, `MpInput`, etc. | ✅ css() function     | `<MpButton :class="css({ mt: '4' })">`  |
| Any component                         | ❌ NEVER inline style | `<div style="color: blue">`             |
