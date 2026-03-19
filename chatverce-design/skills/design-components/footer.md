
# Footer

**Gallery reference:** https://component.gallery/components/footer/
**Aliases:** Site footer, Page footer

## When to Use

Use Footer at the bottom of full-page layouts on the Chatverce web app. It holds secondary navigation (Terms, Privacy, Help), copyright notice, and optional social links. It provides closure to the page and satisfies legal/compliance requirements.

## When NOT to Use

Do not render Footer inside app screens that are more task than page (dashboards, conversation views). Do not show Footer in mobile app views — mobile apps do not have traditional footers. Do not use Footer for primary navigation.

## Anatomy

- **Container:** Full-width, `cv-bg-surface-sunken`, `cv-border-default` top border.
- **Link group:** Secondary navigation links (Terms, Privacy, Help).
- **Copyright line:** Small text, `cv-text-tertiary`.
- **Optional extras:** Social icons, language selector, logo lockup.

## Variants

| Variant | Description |
|---------|-------------|
| Simple | Single row of links + copyright. Minimal pages and marketing. |
| Full | Multi-column with grouped links and branding. Marketing homepage only. |

## Chatverce Styling

- Background: `cv-bg-surface-sunken`
- Top border: `cv-border-default`, 1px
- Link text: `cv-text-secondary`, 13px, underline on hover
- Copyright text: `cv-text-tertiary`, 12px
- Padding: `cv-space-6` vertical, `cv-space-6` horizontal (desktop), `cv-space-4` (tablet)

## Platform Considerations

Web only. Use semantic `<footer>` element. Mobile applications do not use a footer — navigation is handled by the bottom tab bar (Navigation component).

## Accessibility

- Use the semantic `<footer>` element — it carries `role="contentinfo"` implicitly.
- Wrap navigation links in `<nav aria-label="Footer">`.
- Do not duplicate `role="contentinfo"` — only one `<footer>` per page at the top level.

## Code Example

```tsx
<footer className="w-full bg-[--cv-bg-surface-sunken] border-t border-[--cv-border-default] py-6 px-6">
  <div className="max-w-screen-xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-4">
    <nav aria-label="Footer">
      <ul className="flex items-center gap-4">
        <li><a href="/terms" className="text-xs text-[--cv-text-secondary] hover:underline">Terms</a></li>
        <li><a href="/privacy" className="text-xs text-[--cv-text-secondary] hover:underline">Privacy</a></li>
        <li><a href="/help" className="text-xs text-[--cv-text-secondary] hover:underline">Help</a></li>
      </ul>
    </nav>
    <p className="text-xs text-[--cv-text-tertiary]">
      &copy; {new Date().getFullYear()} Chatverce. All rights reserved.
    </p>
  </div>
</footer>
```

## Anti-Patterns

- Never place critical user actions or primary navigation in the Footer.
- Never use `<div>` instead of `<footer>` — the semantic element provides the `contentinfo` landmark for screen readers.
- Never repeat the main Header navigation verbatim in the Footer — Footer links should be secondary and complementary.
