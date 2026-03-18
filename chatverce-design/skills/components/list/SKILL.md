---
name: list
description: Semantic ordered, unordered, or description list for presenting grouped items with consistent spacing
user_invocable: false
---

# List

**Gallery reference:** https://component.gallery/components/list/
**Aliases:** Bullet list, Numbered list, Definition list

## When to Use

Use List when presenting multiple related items where sequence or group membership matters — feature lists, step-by-step instructions, key-value definitions. Lists communicate that items belong together and make scanning easier than prose for multiple items.

## When NOT to Use

Do not use List for navigation — use the Navigation component or a `<nav>` with appropriate links. Do not use List for a single item. Do not use an unordered list when order is meaningful — use `<ol>`.

## Anatomy

- **`<ul>` / `<ol>` / `<dl>`:** Semantic container.
- **`<li>` items:** Each item at consistent spacing.
- **Bullet/number:** Browser default or custom marker via `list-style-type`.
- **Nested list:** Indented with `cv-space-4` left padding. Max 2 levels deep.

## Variants

| Variant | Description |
|---------|-------------|
| Unordered (`<ul>`) | Bullet points. Items are equivalent and unsequenced. Default for feature lists. |
| Ordered (`<ol>`) | Numbered. Items have a defined sequence. Use for instructions, steps. |
| Description (`<dl>`) | Key-value pairs. Label (`<dt>`) and value (`<dd>`). Use for metadata, definitions. |

## Chatverce Styling

- Item spacing: `cv-space-2` (8px) between items; `cv-space-3` for more complex items
- Font: `cv-text-body` (16px) for body context; `cv-text-sm` (14px) for dense UI lists
- Color: `cv-text-primary` (primary items), `cv-text-secondary` (supporting detail)
- Bullet color: `cv-text-accent` or inherit from text
- Nested indent: `cv-space-4` left margin
- `<dt>`: `cv-text-sm`, weight 500, `cv-text-secondary`; `<dd>`: `cv-text-sm`, `cv-text-primary`

## Platform Considerations

Web: Use native `<ul>`, `<ol>`, `<dl>` with Tailwind `list-disc` / `list-decimal`. Mobile: React Native has no semantic list element — use `<View>` with mapped items. For accessibility, set `accessibilityRole="list"` on the container and `accessibilityRole="listitem"` on each item.

## Accessibility

- Use `<ul>` vs `<ol>` semantically — screen readers announce "list of N items" differently for ordered vs unordered.
- Do not use CSS `list-style: none` without a `role="list"` restoration if the list is semantic — some browsers remove list semantics when bullets are removed.
- Mobile: `accessibilityRole="list"` + `accessibilityRole="listitem"` provides equivalent semantics.

## Code Example

```tsx
{/* Unordered — feature list */}
<ul className="list-disc list-outside pl-5 space-y-2 text-[--cv-text-primary]">
  <li>Automated responses 24/7</li>
  <li>Multi-channel support (WhatsApp, Telegram, Webchat)</li>
  <li>No code required to get started</li>
</ul>

{/* Ordered — steps */}
<ol className="list-decimal list-outside pl-5 space-y-2 text-[--cv-text-primary]">
  <li>Connect your WhatsApp number</li>
  <li>Build your first agent flow</li>
  <li>Publish and go live</li>
</ol>

{/* Description — metadata */}
<dl className="space-y-1">
  <div className="flex gap-2">
    <dt className="text-sm font-medium text-[--cv-text-secondary] w-32">Channel</dt>
    <dd className="text-sm text-[--cv-text-primary]">WhatsApp</dd>
  </div>
  <div className="flex gap-2">
    <dt className="text-sm font-medium text-[--cv-text-secondary] w-32">Status</dt>
    <dd className="text-sm text-[--cv-text-primary]">Active</dd>
  </div>
</dl>
```

## Anti-Patterns

- Never use `<br>` to simulate list items — use `<li>`.
- Never nest lists more than 2 levels deep.
- Never strip list semantics with `list-style: none` without restoring `role="list"`.
