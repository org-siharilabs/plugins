
# Breadcrumbs

**Gallery reference:** https://component.gallery/components/breadcrumbs/
**Aliases:** Breadcrumb trail, Location trail, Navigation path

## When to Use

Use Breadcrumbs on pages deeper than one level from the root, where users benefit from knowing their path and may want to navigate back to a parent. Essential in settings pages, agent detail views, and multi-step configuration flows with a persistent hierarchy.

## When NOT to Use

Do not use Breadcrumbs on top-level pages — the trail has no meaningful ancestors to show. Do not use in single-page apps where all navigation is lateral (same level). Do not replace a primary back button with breadcrumbs on mobile — combine them or prefer the back button.

## Anatomy

- **Nav container:** `<nav aria-label="Breadcrumb">` wrapping an `<ol>`.
- **Crumb items:** `<li>` elements, each containing either a `<a>` (ancestor) or `<span>` (current).
- **Separator:** Decorative `/` or `›` character between crumbs. `aria-hidden="true"`.
- **Current page:** Last crumb. `aria-current="page"`. Not a link.

## Variants

| Variant | Description |
|---------|-------------|
| Text-only | Labels only, separator character. Default. |
| Collapsed | Long paths collapse middle crumbs into `…`. Expands on click. |

## Chatverce Styling

- Crumb links: `cv-text-secondary`, 14px, underline on hover
- Current crumb: `cv-text-primary`, 14px, weight 500, no underline
- Separator: `cv-text-tertiary`, `aria-hidden="true"`
- Container: no background, `cv-space-2` vertical padding

## Platform Considerations

Web: Render with semantic `<nav>` + `<ol>`. Mobile: Breadcrumbs are rarely appropriate — show only the immediate parent as a back-link or use a header back arrow instead.

## Accessibility

- Wrap in `<nav aria-label="Breadcrumb">`.
- Use `<ol>` (ordered list) to convey hierarchy.
- Mark the current page with `aria-current="page"` on the last item — do not link it.
- Separator characters must be `aria-hidden="true"`.

## Code Example

```tsx
<nav aria-label="Breadcrumb">
  <ol className="flex items-center gap-1.5 text-sm">
    <li>
      <a href="/dashboard" className="text-[--cv-text-secondary] hover:underline">
        Dashboard
      </a>
    </li>
    <li aria-hidden="true" className="text-[--cv-text-tertiary]">/</li>
    <li>
      <a href="/agents" className="text-[--cv-text-secondary] hover:underline">
        Agents
      </a>
    </li>
    <li aria-hidden="true" className="text-[--cv-text-tertiary]">/</li>
    <li>
      <span aria-current="page" className="text-[--cv-text-primary] font-medium">
        Customer Support Bot
      </span>
    </li>
  </ol>
</nav>
```

## Anti-Patterns

- Never link the current page — it creates a confusing loop.
- Never use `>` as visible text without `aria-hidden` — screen readers will announce "greater than".
- Never use Breadcrumbs as the sole wayfinding mechanism on deeply nested mobile screens.
