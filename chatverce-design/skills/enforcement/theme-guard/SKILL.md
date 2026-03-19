---
name: theme-guard
description: Enforcement guard that ensures all UI works correctly in both light and dark themes
user_invocable: false
---

# Theme Guard

Theme guard enforces that every UI surface works correctly in both light and dark themes without exception. A component that works beautifully in light and breaks in dark is a broken component — not a "dark mode issue." Both themes are first-class and must pass simultaneously.

---

## The Core Rule: Use Semantic Tokens

The Chatverce theme system works by mapping semantic tokens to different primitive values per theme. When you use `bg-cv-surface` it renders as a light surface in light mode and a dark surface in dark mode automatically — no extra work, no extra code.

**If using the `dark:` Tailwind prefix in component code, you are almost certainly doing it wrong.**

The `dark:` prefix exists at one level only: the CSS custom property definition layer (in `tokens-css.md` or the equivalent config). At that level, `dark:` is used to swap the raw values each token maps to. Everywhere else — every component, every screen, every layout — use semantic tokens only. The dark adaptation is invisible.

```tsx
// Wrong — manual dark mode handling
<div className="bg-white dark:bg-gray-900 text-gray-900 dark:text-white">

// Correct — semantic tokens handle both themes
<div className="bg-cv-surface text-cv-content-primary">
```

---

## Violation Reference

| Violation | What it looks like | Why it fails |
|---|---|---|
| Light-only background | `bg-white`, `bg-gray-50`, `bg-[#FAFBFC]` in component code | Invisible on a dark surface; no dark counterpart |
| Dark assumption | `bg-gray-900`, `text-white` as the base (not within `dark:`) | Broken in light theme |
| Missing dark adaptation | Semantic token exists but hardcoded value used instead | Dark switch never applies |
| Opacity breaking in dark | Semi-transparent overlays hardcoded for light (`rgba(255,255,255,0.8)`) | Inverts poorly or disappears in dark |
| Shadow invisible in dark | `shadow-sm` with a hardcoded dark color in a dark background | Light-colored shadows disappear; use `shadow-cv-*` which adapts |
| Low contrast in one theme | Text passes contrast in light, fails in dark (or vice versa) | WCAG failure in one theme |
| StatusBar mismatch (mobile) | `StatusBar style="dark"` hardcoded regardless of theme | Dark text on dark background in dark mode |
| OLED pure black (mobile) | `bg-black` or `bg-[#000000]` for dark backgrounds | Use `bg-cv-surface` which uses `#0A0A0A` (near-black) not pure black |
| `dark:` prefix in component code | `className="text-gray-600 dark:text-gray-400"` in a component | Bypasses semantic token system; maintenance burden doubles |
| Hardcoded border color | `border-gray-200` — light-only; invisible or wrong in dark | Use `border-cv-border-default` |

---

## Testing Protocol: Mental Theme Swap

For every element in a UI implementation, mentally swap to the other theme and ask:

1. **Is this element still visible?** — Text, borders, icons, dividers all need contrast in both themes.
2. **Is the hierarchy preserved?** — Background layering (base → surface → elevated) must still read as depth in both themes.
3. **Do interactive states still work?** — Hover, focus, pressed states on both themes.
4. **Are overlays and modals readable?** — Semi-transparent scrims must work in both; use `bg-cv-overlay`.
5. **Are shadows still meaningful?** — Light shadows on dark backgrounds disappear. Elevation in dark theme is often expressed through lighter surface values, not shadows.
6. **Is the StatusBar correct?** — On mobile, StatusBar content (`light-content` / `dark-content`) must match the header background in both themes.
7. **Are all images and icons legible?** — Icons with hardcoded fill colors may need theme-adaptive variants.

This mental swap is mandatory for every PR that touches UI code.

---

## Mobile-Specific Rules

**StatusBar (React Native)**

StatusBar style must be set dynamically based on the active theme. Never hardcode:
```tsx
// Wrong
<StatusBar barStyle="dark-content" />

// Correct — responds to theme
const { theme } = useTheme();
<StatusBar barStyle={theme === 'dark' ? 'light-content' : 'dark-content'} />
```

**OLED pure black**

Do not use pure `#000000` for dark backgrounds. Chatverce's dark theme uses a near-black primitive (`cv-primitive-gray-950` or equivalent) for the base surface. Pure black causes excessive contrast and looks harsh. Use `bg-cv-background` which maps correctly.

**System appearance**

On mobile, use the system's color scheme API to detect theme preference and apply it — do not hard-wire either theme. The app must respond to the user's OS-level preference unless they have overridden it in-app.

---

## Self-Correction Protocol

When a theme violation is detected, correct before saving:

1. **Identify the problematic value** — Is it a hardcoded color that has no dark counterpart?
2. **Find the semantic equivalent** — Reference `foundations/theme-system/SKILL.md` for the correct token.
3. **Replace with the semantic token** — The token must work in both themes without any `dark:` prefix on this element.
4. **Re-run the mental swap** — Confirm the replacement works visually in both themes.
5. **For `dark:` usages** — Remove the `dark:` override and replace both sides with the single semantic token that covers both themes.

---

## Rationale

Chatverce ships to users on devices in every lighting context — offices, commutes, late nights. A user working at night with dark mode enabled experiences a broken product if even one screen is white-only. Theme integrity is not a feature request. It is a baseline quality requirement equivalent to the app not crashing.
