
# A11y Guard

A11y guard enforces Chatverce's accessibility standards across web (WCAG 2.1 AA + ARIA) and mobile (iOS VoiceOver, Android TalkBack). Accessibility violations are never "nice to fix" — they lock out users who depend on assistive technologies. All violations in this guard are "must fix."

Full reference: `foundations/accessibility/SKILL.md`.


## Web Violations

| Violation | Pattern | Fix |
|---|---|---|
| Missing input label | `<input>` or `<textarea>` without an associated `<label>`, `aria-label`, or `aria-labelledby` | Add `<label htmlFor="...">` or `aria-label` |
| Image without alt | `<img>` without `alt` attribute; `<img alt="">` on informational image | Add descriptive `alt` text; `alt=""` for decorative only |
| Button without accessible name | `<button>` or `role="button"` containing only an icon with no text, `aria-label`, or `aria-labelledby` | Add `aria-label="[action description]"` |
| Touch/click target below 44px | Interactive element with `w-` or `h-` less than 44px, no padding to compensate | Pad to meet 44×44px minimum without changing visual size |
| Color as the only meaning | Error state shown purely by turning text red with no icon, label, or pattern change | Add icon or text label alongside color |
| Missing `focus-visible` | `outline-none` or `focus:outline-none` with no replacement focus ring | Use design system focus ring: `focus-visible:ring-2 focus-visible:ring-cv-accent focus-visible:ring-offset-2` |
| Missing keyboard navigation | Interactive element not reachable by Tab; `onClick` on a non-interactive element with no keyboard handler | Use `<button>` or `<a>` natively; add `onKeyDown` if custom element |
| No skip link | Page or app shell with no `<a href="#main-content" className="sr-only focus:not-sr-only">Skip to content</a>` | Add skip link as first child of `<body>` |
| Missing reduced-motion | Animated component with no `motion-safe:` guard or `@media (prefers-reduced-motion: reduce)` override | Wrap in `motion-safe:` Tailwind prefix; add CSS fallback |
| Heading hierarchy skip | `<h1>` followed directly by `<h3>` with no `<h2>` | Restore sequential heading levels: `h1` → `h2` → `h3` |
| Missing `aria-live` on dynamic content | Toast, status message, counter, or validation feedback updated without `aria-live` | Add `role="status"` (polite) or `role="alert"` (assertive) |
| Icon-only link | `<a>` wrapping only an icon SVG with no text or `aria-label` | Add `aria-label` describing the destination |
| Missing form error association | Form validation error displayed visually but not associated with input via `aria-describedby` | Add `aria-describedby="error-id"` to the input; `id="error-id"` on error element |
| Modal without focus trap | Dialog/modal that doesn't trap Tab focus within its bounds | Implement focus trap using `inert` on background or a focus-trap utility |
| `tabindex` greater than 0 | `tabIndex={2}`, `tabindex="3"` used to reorder focus | Remove positive tabindex; fix DOM order to match visual order |


## Mobile Violations (React Native)

| Violation | Pattern | Fix |
|---|---|---|
| Missing accessible role | Interactive custom element with no `accessibilityRole` | Add `accessibilityRole="button"`, `"link"`, `"switch"`, etc. |
| Missing accessibility label | Touchable or interactive element with only icon content and no `accessibilityLabel` | Add `accessibilityLabel="[what this does]"` |
| Missing live region | Dynamic content (status update, counter, toast) with no `accessibilityLiveRegion` | Add `accessibilityLiveRegion="polite"` or `"assertive"` |
| Touch target below minimum | Interactive element with effective tap area below 44×44pt (iOS) or 48×48dp (Android) | Add padding to extend tap area; do not reduce visual size |
| Missing accessibility state | Toggle, checkbox, or expandable element missing `accessibilityState` | Add `accessibilityState={{ checked: bool }}` or `{ expanded: bool }` |
| Image without label | `<Image>` conveying information with no `accessibilityLabel` | Add `accessibilityLabel="[what the image shows]"` or `accessible={false}` if decorative |
| Adjacent targets too close | Interactive elements spaced less than 8px apart | Add minimum 8px spacing to prevent mis-taps |
| Missing hint for non-obvious actions | Icon-only controls where the result of activation is not self-evident | Add `accessibilityHint="[what happens when activated]"` |
| Incorrect grouping | Related label + control rendered as separate accessibility nodes increasing swipe count | Group with `accessible={true}` on container and single `accessibilityLabel` |
| Missing `importantForAccessibility` on decorative | Decorative icon or image picked up by TalkBack unnecessarily | Add `importantForAccessibility="no"` on Android decorative elements |


## Contrast Violations

These apply to both web and mobile:

| Violation | Threshold | Fix |
|---|---|---|
| Body text below 4.5:1 contrast | Text smaller than 18px regular or 14px bold | Use higher-contrast token; reference `foundations/color/contrast-matrix.md` |
| Large text below 3:1 contrast | Text at 18px regular or 14px bold and above | Use higher-contrast token |
| UI component below 3:1 contrast | Input borders, icons, focus rings against background | Use border or icon token with sufficient contrast |
| Placeholder text used as label | Placeholder disappears on focus; never meets contrast requirements as primary label | Replace with visible label above the input |


## Self-Correction Protocol

When an accessibility violation is detected:

1. **Classify the violation** — Is it ARIA/semantic, contrast, target size, or keyboard?
2. **Apply the fix from the table** — The correct fix is always listed. Do not invent alternatives.
3. **For mobile** — Verify the fix works for both VoiceOver (iOS) and TalkBack (Android) semantics.
4. **Do not rely solely on visual review** — Accessibility issues are often invisible in a visual pass. Always check implementation code for missing attributes.
5. **Correct in the current file before saving** — Do not defer accessibility fixes.


## Rationale

Accessibility failures are not edge cases. Screen reader users, keyboard-only users, and users with motor impairments rely on correct semantics and sufficient contrast every time they use the product. A missing `aria-label` makes a button unusable to someone who cannot see the screen. A 40px touch target on a shaking bus misses on every attempt. These failures are as serious as a crash.

Chatverce treats WCAG 2.1 AA as the floor, not the goal. Where AAA is achievable without design compromise, prefer it.
