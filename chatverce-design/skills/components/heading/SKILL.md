---
name: heading
description: Hierarchical text headings (h1–h6) that structure page content and drive screen reader navigation
user_invocable: false
---

# Heading

**Gallery reference:** https://component.gallery/components/heading/
**Aliases:** Title, Section heading, Page title

## When to Use

Use Heading to establish a clear visual and semantic content hierarchy on every page and screen. Headings allow screen reader users to navigate a page by structure, and give sighted users clear scanning landmarks. Every page must have exactly one `h1`.

## When NOT to Use

Do not skip heading levels for visual styling reasons (e.g., using `h4` because it looks right while `h2` and `h3` are absent). Do not use Heading for decorative large text — apply a display-scale token to a `<p>` or `<span>` instead. Do not use Heading to mean "bold text" — use `<strong>` for inline emphasis.

## Anatomy

- **Element:** `h1`–`h6` semantic HTML element.
- **Text:** Rendered in the `cv-text-*` scale mapped to the level.
- **Color:** `cv-text-primary` by default.
- **Margin:** Top margin from parent spacing token; no built-in bottom margin (controlled by parent layout).

## Variants

The heading levels map to the Chatverce type scale:

| Level | Token | Size | Weight | Use |
|-------|-------|------|--------|-----|
| h1 | `cv-text-h1` | 30px | 700 | Page title. One per page. |
| h2 | `cv-text-h2` | 24px | 600 | Major section. |
| h3 | `cv-text-h3` | 20px | 600 | Sub-section within h2. |
| h4 | `cv-text-h4` | 16px | 600 | Sub-section within h3. |
| h5 | `cv-text-h5` | 14px | 600 | Rarely used. Card/widget heading. |
| h6 | `cv-text-h6` | 13px | 600 | Only when h5 is already used above. |

## Chatverce Styling

- Color: `cv-text-primary`
- Font family: inherits from `cv-font-sans`
- Letter spacing: `cv-tracking-tight` for h1–h2, normal for h3–h6
- Line height: `cv-leading-tight`

## Platform Considerations

Web: Use native `h1`–`h6` elements. Do not style with heading tokens applied to `<div>` when semantic headings are warranted. Mobile: React Native has no semantic heading elements — use `Text` with `accessibilityRole="header"` to expose the heading role to screen readers.

## Accessibility

- Maintain sequential heading order — never jump from `h1` to `h4`.
- `accessibilityRole="header"` on React Native `Text` for the equivalent of an `h` element.
- Do not use ARIA `role="heading"` on web — use native elements.
- Use exactly one `h1` per page/screen.

## Code Example

```tsx
{/* Web */}
<h1 className="text-[--cv-text-h1] font-bold text-[--cv-text-primary] tracking-tight leading-tight">
  Your Agents
</h1>
<h2 className="text-[--cv-text-h2] font-semibold text-[--cv-text-primary] leading-tight">
  Active Agents
</h2>
<h3 className="text-[--cv-text-h3] font-semibold text-[--cv-text-primary]">
  WhatsApp Channel
</h3>

{/* Mobile (React Native) */}
<Text
  className="text-2xl font-bold text-[--cv-text-primary]"
  accessibilityRole="header"
>
  Your Agents
</Text>
```

## Anti-Patterns

- Never skip heading levels for styling — adjust the type token while maintaining the semantic level.
- Never use multiple `h1` elements on one page.
- Never replace headings with large bold `<p>` tags — screen readers won't surface them in outline navigation.
