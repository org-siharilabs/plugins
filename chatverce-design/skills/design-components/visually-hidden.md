
# Visually Hidden

**Gallery reference:** https://component.gallery/components/visually-hidden/

## When to Use

Use Visually Hidden to provide screen reader context that sighted users receive through visual layout — supplementary labels for icon-only buttons, "opens in new tab" notices on external links, row/column context in tables, and descriptive headings for landmark regions. It is one of the most important accessibility primitives in the system.

## Chatverce Styling

The visually-hidden technique uses CSS to position text off-screen without `display: none` or `visibility: hidden` (which would hide it from screen readers too):

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
```

Use Tailwind's built-in `sr-only` class — it implements this pattern exactly.

## Accessibility

- Never use `display: none` or `visibility: hidden` for text intended for screen readers — these hide from all users including assistive technology.
- Use `focus:not-sr-only` alongside `sr-only` when the element should become visible on keyboard focus (e.g., Skip Link).
- On React Native: use `accessibilityLabel` on the parent element instead of a separate hidden text node.

## Code Example

```tsx
{/* Icon-only button — screen reader label */}
<button className="text-[--cv-text-secondary]" aria-label="Open settings">
  <Settings size={20} aria-hidden="true" />
</button>

{/* Alternative: visually hidden span (use aria-label on the button instead when possible) */}
<button className="text-[--cv-text-secondary]">
  <Settings size={20} aria-hidden="true" />
  <span className="sr-only">Open settings</span>
</button>

{/* External link notice */}
<a href="https://docs.chatverce.com" target="_blank" rel="noopener noreferrer" className="text-[--cv-text-link] underline">
  Documentation
  <span className="sr-only"> (opens in new tab)</span>
</a>

{/* Progress context */}
<div role="status">
  <span className="sr-only">Loading conversations…</span>
  <Loader2 size={24} className="animate-spin text-[--cv-bg-accent]" aria-hidden="true" />
</div>
```
