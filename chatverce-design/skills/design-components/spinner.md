
# Spinner

**Gallery reference:** https://component.gallery/components/spinner/
**Aliases:** Loading spinner, Activity indicator, Loader

## When to Use

Use Spinner for short, indeterminate loading states where the full content layout is not known in advance — fetching data after a button action, authenticating, or navigating between screens with unknown content. It signals "something is happening" without promising a specific layout.

## When NOT to Use

Do not use Spinner when the content layout is predictable — use Skeleton instead, which prevents layout shift and feels less anxious. Do not use Spinner for long processes with measurable completion — use Progress Bar. Do not show Spinner for operations expected to complete in under 300ms — it creates a flash with no benefit.

## Anatomy

- **Spinner graphic:** Circular arc, rotating 360° continuously.
- **Color:** `cv-bg-accent` (teal) by default. White (`cv-text-on-accent`) on dark/accent backgrounds.
- **Size:** 16px (inline), 24px (default), 40px (full-page).
- **Container:** Centers the spinner within its parent context.
- **`role="status"`:** Announces the loading state to screen readers.

## Variants

| Variant | Description |
|---------|-------------|
| Inline | 16px. Inside buttons or tight UI contexts. |
| Default | 24px. Standard loading state for cards, panels, and sections. |
| Full-page | 40px. Centered on a loading screen or modal. |

## Chatverce Styling

- Color: `cv-bg-accent` (standard), `cv-text-on-accent` (on accent backgrounds)
- Rotation animation: 750ms linear infinite — use Tailwind `animate-spin`
- Size: `size-4` (16px), `size-6` (24px), `size-10` (40px)
- The arc stroke typically uses `opacity-25` for the track and full opacity for the active arc

## Platform Considerations

Web: Use `<Loader2>` from `lucide-react` with `animate-spin` or a custom SVG spinner. Mobile: Use React Native's `<ActivityIndicator>` with `color="var(--cv-bg-accent)"`. `ActivityIndicator` natively handles the accessibility role on both iOS and Android.

## Accessibility

- Wrap in a container with `role="status"` and `aria-label="Loading"` (or a more descriptive label).
- Include visually hidden text: `<span className="sr-only">Loading...</span>`.
- Mobile: `ActivityIndicator` announces its state automatically. Add `accessibilityLabel` to the surrounding view if context is needed.
- Remove Spinner from the DOM (or set `aria-hidden="true"`) when loading completes to stop re-announcement.

## Code Example

```tsx
import { Loader2 } from "lucide-react";

{/* Inline — inside a button */}
<button disabled className="flex items-center gap-2 bg-[--cv-bg-accent] text-[--cv-text-on-accent] rounded-[--cv-radius-md] px-4 py-2">
  <Loader2 size={16} className="animate-spin" aria-hidden="true" />
  Saving…
</button>

{/* Section loading state */}
<div role="status" aria-label="Loading conversations" className="flex items-center justify-center py-12">
  <Loader2 size={24} className="animate-spin text-[--cv-bg-accent]" />
  <span className="sr-only">Loading conversations…</span>
</div>
```

## Anti-Patterns

- Never use Spinner when the content layout is predictable — use Skeleton.
- Never omit `role="status"` — screen reader users cannot see the animation.
- Never display a Spinner indefinitely without a timeout and error state.
- Never use motion-based spinners without respecting `prefers-reduced-motion` — use a static pulsing indicator for reduced-motion users.
