---
name: accessibility
description: Complete accessibility system — contrast minimums, touch targets, keyboard interaction, focus management, screen reader patterns, and reduced motion for web and mobile
user_invocable: false
---

## Philosophy

Accessibility is built in from the start — never retrofitted. Every Chatverce surface is designed for the full spectrum of users: those using keyboards, assistive technologies, low-vision displays, or motor accommodations.

The POUR principles guide every decision:

- **Perceivable** — Information and UI components are presentable to users in ways they can perceive (visible, audible, or tactile).
- **Operable** — UI components and navigation are operable without a mouse or touch.
- **Understandable** — Information and UI operation are understandable to all users.
- **Robust** — Content is robust enough to be interpreted by a wide variety of assistive technologies, current and future.

Accessibility is not a checklist. It is a quality bar equivalent to visual design and performance.

---

## Contrast Minimums

All text and interactive elements must meet WCAG 2.1 AA as the floor. WCAG AAA is the preferred target wherever achievable.

| Use case | Minimum ratio | Preferred ratio |
|---|---|---|
| Body text (< 18px regular or < 14px bold) | 4.5:1 | 7:1 |
| Large text (≥ 18px regular or ≥ 14px bold) | 3:1 | 4.5:1 |
| UI components and graphical objects | 3:1 | 4.5:1 |
| Decorative elements (no information) | No requirement | — |

All color pairings are documented in `color/contrast-matrix.md`. Never introduce a new text/background pairing without verifying it against the matrix or running it through a contrast checker.

---

## Touch and Click Targets

Minimum interactive target sizes ensure usability for users with motor impairments and in low-precision contexts (moving, gloves, small screens).

| Platform | Minimum target size | Notes |
|---|---|---|
| iOS | 44 × 44 pt | Apple HIG requirement |
| Android | 48 × 48 dp | Material Design requirement |
| Web | 44 × 44 px | WCAG 2.5.5 (AAA); 24 × 24 px is AA floor |

When a visual element is smaller than the minimum (e.g., a 24px icon), extend the tap/click area using padding, an invisible overlay, or a larger hit area — do not change the visual size.

Spacing between adjacent targets: at minimum 8px to prevent mis-taps.

---

## Keyboard Interaction (Web)

All web surfaces must be fully operable by keyboard alone. No interaction may be mouse-only.

| Key | Action |
|---|---|
| `Tab` | Move focus forward through interactive elements |
| `Shift + Tab` | Move focus backward through interactive elements |
| `Enter` | Activate a button, link, or focused item |
| `Space` | Activate a button; toggle a checkbox or switch |
| `Escape` | Close overlay, modal, dropdown, or popover |
| `Arrow keys` | Navigate within a widget (tabs, menus, radio groups, sliders) |
| `Home` | Move to first item in a list or widget |
| `End` | Move to last item in a list or widget |

Focus order follows DOM order. Visual order and DOM order must match — do not use `tabindex` values greater than 0 to reorder focus.

All focusable elements must have a visible focus indicator. Use the design system focus ring (2px offset, brand accent color) — never suppress `:focus-visible` styles without a replacement.

---

## Focus Management

Focus must be deliberately managed at key interaction moments. Unmanaged focus leaves keyboard and screen reader users stranded.

| Trigger | Focus behavior |
|---|---|
| Modal opens | Trap focus within modal; move focus to first interactive element or the modal heading |
| Modal closes | Return focus to the element that triggered the modal |
| Toast / snackbar appears | Do not steal focus; use `aria-live` to announce |
| Route / page change (web SPA) | Move focus to main content area or page heading |
| Dropdown opens | Move focus to first item in the list |
| Dropdown closes (Escape) | Return focus to the trigger element |
| Inline form validation | Focus the first field with an error |
| Step-based flow advances | Move focus to the new step heading or first input |

Focus trapping in modals: use `inert` attribute on background content (preferred) or a focus trap utility. Tab and Shift+Tab must cycle only within the modal while it is open.

---

## ARIA Pattern Reference

Use WAI-ARIA roles and attributes to communicate component semantics to assistive technologies. Never use ARIA to override correct native HTML semantics — prefer native elements first.

| Component | Role / Attribute |
|---|---|
| Accordion | `role="region"` on panel, `aria-expanded` on trigger |
| Alert / banner | `role="alert"`, `aria-live="assertive"` |
| Modal / dialog | `role="dialog"`, `aria-modal="true"`, `aria-labelledby` pointing to heading |
| Toast / snackbar | `role="status"`, `aria-live="polite"` |
| Tabs | `role="tablist"` on container, `role="tab"` on tab, `role="tabpanel"` on panel |
| Drawer | `role="dialog"`, `aria-modal="true"` (treat as modal) |
| Dropdown menu | `role="menu"` on list, `role="menuitem"` on items |
| Toggle / switch | `role="switch"`, `aria-checked="true|false"` |
| Skeleton loader | `aria-busy="true"` on the container being loaded |
| Progress bar | `role="progressbar"`, `aria-valuenow`, `aria-valuemin`, `aria-valuemax`, `aria-label` |
| Combobox / autocomplete | `role="combobox"`, `aria-controls`, `aria-expanded`, `aria-autocomplete` |
| Tooltip | `role="tooltip"`, referenced via `aria-describedby` on trigger |

---

## Screen Reader Support

### Web

- Use `aria-live` regions for dynamic content updates (toasts, status messages, counters).
- Provide `aria-label` for icon-only buttons and controls without visible text.
- Use `aria-describedby` to associate supplementary descriptions (e.g., password rules, field hints) with inputs.
- Form inputs must have associated `<label>` elements (not placeholder text alone).
- Images conveying information need descriptive `alt` text. Decorative images use `alt=""`.

### iOS — VoiceOver

| Property | Purpose |
|---|---|
| `accessible={true}` | Marks element as an accessible target |
| `accessibilityLabel` | The spoken label (replaces visual text when needed) |
| `accessibilityHint` | Describes the outcome of activating an element |
| `accessibilityRole` | Communicates element type: `button`, `link`, `header`, `image`, `switch`, etc. |
| `accessibilityState` | Communicates states: `{ disabled, selected, checked, expanded, busy }` |
| `accessibilityLiveRegion` | For dynamic updates: `"polite"` or `"assertive"` |

### Android — TalkBack

| Property | Purpose |
|---|---|
| `accessibilityLiveRegion` | Equivalent to `aria-live`; `"polite"` or `"assertive"` |
| `importantForAccessibility` | `"yes"`, `"no"`, or `"no-hide-descendants"` to control TalkBack focus |
| `accessibilityLabel` | Overrides the spoken label |
| `accessibilityRole` | Communicates element type (shared with iOS in React Native) |

Group related elements into a single accessibility node where it reduces cognitive load. Use `accessible={true}` with `accessibilityLabel` on the container.

---

## Dynamic Sizing

Text must scale with user preferences. Never lock font sizes.

- **iOS:** Support Dynamic Type. Use semantic text styles (`UIFont.preferredFont(forTextStyle:)` or SwiftUI equivalents). Test at all Dynamic Type sizes including Accessibility sizes.
- **Android:** Use `sp` units for text. Test with font scaling set to largest in system settings.
- **Web:** Use `rem`/`em` units — never `px` for font sizes. Respect the user's browser font size preference.

Test all layouts at 200% text enlargement. Content must not be clipped, truncated without disclosure, or require horizontal scrolling to read.

---

## Reduced Motion

Users who enable "Reduce Motion" in system settings must receive a functionally equivalent experience with no vestibular-triggering animations.

Refer to the motion foundation for implementation specifics (`foundations/motion/SKILL.md`). The core rule: all motion is gated behind a reduced motion check. When reduced motion is active, transitions are instant or use simple opacity fades — no translation, scaling, or rotation.

Do not use motion to convey meaning exclusively. Any state change communicated through animation must also be communicated through a label, icon, or text update.

---

## Heading Hierarchy

Headings communicate page structure to screen reader users and define the document outline.

- Use headings sequentially: `h1` → `h2` → `h3`. Never skip levels (e.g., `h1` directly to `h3`).
- One `h1` per page or view.
- In React Native, use `accessibilityRole="header"` to mark section headings.
- Do not use heading elements purely for visual styling — use the typography system's size/weight tokens on a `<p>` or `<span>` instead.
- Modals and drawers should have their own heading (the modal title) at `h2` or the appropriate level within the document hierarchy.
