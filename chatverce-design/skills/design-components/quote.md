
# Quote

**Gallery reference:** https://component.gallery/components/blockquote/

## When to Use

Use Quote to visually offset a significant excerpt, customer testimonial, or referenced statement from surrounding body text. The left border treatment signals that content is attributed to an external source or carries special emphasis distinct from body copy.

## Chatverce Styling

- Container: `<blockquote>`, `cv-border-accent` left border (3px), `cv-space-4` left padding
- Text: `cv-text-body`, italic, `cv-text-secondary`
- Attribution (`<cite>`): `cv-text-sm`, `cv-text-tertiary`, not italic, preceded by `—`
- Background: none (transparent) — no filled background on quotes

## Accessibility

- Use semantic `<blockquote>` — screen readers identify it as a quotation.
- Attribution goes in `<cite>` inside or after the `<blockquote>`.
- Do not use `<q>` for multi-line pulled quotes — `<blockquote>` is for block-level quotations.

## Code Example

```tsx
<blockquote className="border-l-[3px] border-[--cv-border-accent] pl-4 my-4">
  <p className="text-base italic text-[--cv-text-secondary]">
    "Chatverce reduced our support response time by 60% in the first month. Our customers love the instant replies."
  </p>
  <footer className="mt-2">
    <cite className="text-sm text-[--cv-text-tertiary] not-italic">
      — Maria Santos, Head of Customer Success at Loja Verde
    </cite>
  </footer>
</blockquote>
```
