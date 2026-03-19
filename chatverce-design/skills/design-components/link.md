
# Link

**Gallery reference:** https://component.gallery/components/link/
**Aliases:** Anchor, Hyperlink, Text link

## When to Use

Use Link for navigation — moving to a different page, section, or external resource. Link is the correct element when the destination can be bookmarked, opened in a new tab, or shared. Use Link within body text, lists, footers, and navigation areas.

## When NOT to Use

Do not use Link to trigger actions like submitting a form, deleting data, or opening a modal — use Button instead. Do not style a `<button>` as a link or a `<a>` as a button; the semantic element must match the behavior.

## Anatomy

- **`<a>` element:** With `href`. Navigates on click and on Enter key.
- **Label:** Descriptive text that makes sense out of context (for screen reader link lists).
- **External indicator (optional):** `ExternalLink` icon (16px, `aria-hidden`) + ` (opens in new tab)` visually hidden text.
- **Visited state (optional):** `cv-text-link-visited` when tracking visited pages aids orientation.

## Variants

| Variant | Description |
|---------|-------------|
| Inline | Inside body text. Underlined. `cv-text-link`. Default. |
| Standalone | List of links, footer links. Underline on hover only. |

## Chatverce Styling

- Default color: `cv-text-link` (teal, matches accent)
- Underline: present by default on inline links; on hover only for standalone
- Hover: underline (inline), `cv-text-link` color maintained
- Visited: `cv-text-link-visited` (optional — use only when visit history aids navigation)
- Focus ring: 2px `cv-border-accent` offset 2px
- External link icon: 16px, `aria-hidden="true"`

## Platform Considerations

Web: Use Next.js `<Link>` for internal navigation (prefetches routes). Use `<a>` with `target="_blank" rel="noopener noreferrer"` for external links. Mobile: There is no inline link concept in native apps — use `Pressable` with `onPress` calling `Linking.openURL()` for external URLs. Style with `cv-text-link` token.

## Accessibility

- Link text must be descriptive out of context — avoid "click here" or "read more".
- External links: include visually hidden text `(opens in new tab)` or equivalent.
- `target="_blank"` links must have `rel="noopener noreferrer"`.
- Mobile: use `accessibilityRole="link"` on Pressable used as a link.

## Code Example

```tsx
import Link from "next/link";
import { ExternalLink } from "lucide-react";

{/* Internal link */}
<Link
  href="/agents/new"
  className="text-[--cv-text-link] underline hover:no-underline focus:outline-none focus-visible:ring-2 focus-visible:ring-[--cv-border-accent]"
>
  Create a new agent
</Link>

{/* External link */}
<a
  href="https://docs.chatverce.com"
  target="_blank"
  rel="noopener noreferrer"
  className="inline-flex items-center gap-1 text-[--cv-text-link] underline hover:no-underline"
>
  Documentation
  <ExternalLink size={14} aria-hidden="true" />
  <span className="sr-only">(opens in new tab)</span>
</a>
```

## Anti-Patterns

- Never use `<a href="#">` as a button trigger — use `<button>` instead.
- Never write link labels like "here", "click here", or "more" — they are meaningless to screen reader users navigating by links.
- Never open external links in a new tab without warning the user.
