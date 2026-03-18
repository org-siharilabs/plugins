---
name: separator
description: A thin horizontal (or vertical) line that visually divides distinct sections of content
user_invocable: false
---

# Separator

**Gallery reference:** https://component.gallery/components/separator/
**Aliases:** Divider, Rule, Horizontal rule

## When to Use

Use Separator to create visual breathing room between logically distinct sections when whitespace alone is insufficient. Common in settings panels between categories, dropdown menus between groups, and cards containing mixed content types.

## When NOT to Use

Do not use Separator between every item in a list — use consistent spacing (`cv-space-*`) instead. Do not use Separator as a substitute for proper grouping via headings or Fieldset. Do not use multiple consecutive separators.

## Anatomy

- **`<hr>` or `role="separator"`:** Single 1px line.
- **Orientation:** Horizontal (default) or vertical (for inline content dividers).
- **Spacing:** `cv-space-3` (12px) or `cv-space-4` (16px) above and below.

## Variants

| Variant | Description |
|---------|-------------|
| Horizontal | Full-width within its container. Default. Between sections, menu groups. |
| Vertical | Between inline elements (e.g., breadcrumb separators, toolbar items). Use `aria-orientation="vertical"`. |

## Chatverce Styling

- Color: `cv-border-default` (1px)
- Horizontal margin: 0 (full bleed within container) or `cv-space-4` for inset variants
- Spacing: `my-[--cv-space-3]` or `my-[--cv-space-4]` depending on section weight
- No shadow, no gradient

## Platform Considerations

Web: Use `<hr>` with CSS reset for semantic separators; use `<div role="separator" aria-hidden="true">` for purely decorative dividers. Mobile: Use a `View` with `height: 1`, `backgroundColor: "var(--cv-border-default)"`. Decorative — no accessibility role needed.

## Accessibility

- Semantic `<hr>` carries `role="separator"` implicitly and is announced by screen readers. Use for meaningful section breaks.
- Purely decorative separators: `<div role="separator" aria-hidden="true">` or `<hr aria-hidden="true">`.
- Do not use `<hr>` only for visual styling when no semantic section break is intended — add `aria-hidden="true"`.

## Code Example

```tsx
{/* Semantic separator between sections */}
<hr className="border-t border-[--cv-border-default] my-4" />

{/* Decorative only — hidden from screen readers */}
<hr aria-hidden="true" className="border-t border-[--cv-border-default] my-3" />

{/* Inset separator (e.g., within a card) */}
<hr className="border-t border-[--cv-border-default] mx-4 my-3" />

{/* Vertical separator between inline items */}
<div
  role="separator"
  aria-orientation="vertical"
  className="w-px h-4 bg-[--cv-border-default] mx-2"
/>
```

## Anti-Patterns

- Never use Separator as a substitute for structural grouping — headings and sections communicate hierarchy better.
- Never use multiple stacked Separators.
- Never style `<hr>` with colors outside the `cv-border-*` scale.
