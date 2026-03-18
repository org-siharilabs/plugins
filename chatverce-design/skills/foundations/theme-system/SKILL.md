---
name: theme-system
description: Three-layer token architecture (primitive → semantic → component) with dark/light theme switching for web and mobile
user_invocable: false
---

# Theme System — Three-Layer Token Architecture

Chatverce uses a strict three-layer token architecture to ensure consistent theming
across web and mobile platforms. All color decisions flow through this system. No
raw hex values or primitive tokens appear in application code.

---

## Architecture Overview

### Layer 1 — Primitives

Raw color values expressed as hex. Full scales (50–950) are defined for:

- `navy` — primary brand color family
- `teal` — accent color family
- `gray` — neutral color family
- `red` — error / destructive family
- `amber` — warning family
- `green` — success family

Channel colors (for platform integrations) are also defined at the primitive layer.

**Rule: Primitives are NEVER referenced in application code.** They exist only in
primitive definition files. They are the source of truth for exact color values, not
a palette for developers to pick from.

### Layer 2 — Semantic

Intent-based tokens that resolve to different primitive values per theme (light or
dark). Semantic token names describe **purpose**, not appearance.

Correct: `cv-bg-primary`, `cv-text-muted`, `cv-border-focus`
Incorrect: `cv-gray-50`, `cv-navy-800`, `#1a2b3c`

This is the layer application code references for all color decisions. Semantic
tokens are defined once and switch automatically when the theme changes.

### Layer 3 — Component

Tokens specific to individual UI components. Each component's guideline file defines
its own component tokens using semantic tokens as inputs. Component tokens are not
defined centrally — they live with the component they belong to.

Example: a `Button` component guideline defines `cv-btn-bg`, `cv-btn-text`, and
`cv-btn-border` in terms of semantic tokens.

---

## The Token Rule

> Application code references **semantic or component tokens only**.
> Never primitives. Never raw hex or rgb values.

The only permitted exceptions are:

- **Primitive definition files** — where primitive values are declared
- **`/* cv-override */` comment** — marks an intentional, reviewed deviation
- **`/* cv-exception: reason */` comment** — marks a case where a token does not
  exist yet, with an explanation and a follow-up obligation to add it

If neither exception applies, the correct path is to define a new semantic token and
reference that.

---

## Theme Switching Mechanism

### Web

Semantic tokens are implemented as CSS custom properties declared on `:root` for the
light theme. The dark theme overrides are applied via:

1. `[data-theme="dark"]` attribute selector — for explicit user preference
2. `@media (prefers-color-scheme: dark)` — for system-level preference

Tailwind integration uses the `dark:` class strategy **at the CSS variable level
only**. Component code never uses `dark:` prefix directly. All dark-mode behavior
flows through the CSS custom property swap.

### Mobile (NativeWind)

- The `useColorScheme()` hook reads the device color scheme setting.
- NativeWind dark mode support is enabled and reads from the same `cv-*` token
  namespace.
- The system setting is respected by default; user override is managed at the app
  shell level.
- The same `cv-*` class names that work on web work in NativeWind — no
  platform-specific token names.

---

## How to Add a New Token

Follow this sequence exactly. Do not skip steps.

1. **Add a primitive (if needed)** — Only if the exact hex value does not already
   exist in the primitive scale. Add it to `primitives.md`.

2. **Add semantic mappings for BOTH themes** — Add the token to `semantic-light.md`
   and `semantic-dark.md`. Both entries are required before the token is usable.

3. **Verify contrast in both themes** — Check WCAG AA contrast ratios for the
   light and dark values before merging. Use the token name and both resolved values
   in the PR description.

4. **Reference the semantic token in code** — Use the `cv-*` class name or CSS
   custom property in component code. Do not reference the primitive name.

5. **Never reference the primitive directly** — Even if the primitive was just
   added. The semantic layer exists to decouple intent from implementation.

---

## Companion Files

These sibling files in this directory complete the theme system definition:

| File | Purpose |
|---|---|
| `primitives.md` | Full primitive color scales (all hex values, all steps 50–950) |
| `semantic-light.md` | Light theme semantic token mappings |
| `semantic-dark.md` | Dark theme semantic token mappings |
| `tokens-tailwind.md` | Shared Tailwind config (used by both web and NativeWind) |
| `tokens-css.md` | CSS custom properties declaration (web implementation) |
| `tokens-rn.md` | React Native constants (fallback for `StyleSheet.create` contexts) |

---

*For color primitive values, see `primitives.md`. For how components consume tokens,
see each component's guideline file in the components skill. For enforcement rules,
see the enforcement skill.*
