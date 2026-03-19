---
name: design-foundations
description: "Chatverce design foundations: theme system (3-layer tokens, dark/light), color, typography, layout, motion, materials, iconography, accessibility, privacy, and writing. Use when implementing any Chatverce UI."
user_invocable: false
---

# Chatverce Design Foundations

10 foundation layers plus the complete token system. These define every visual and behavioral primitive used across Chatverce web (Next.js + Tailwind + shadcn/ui) and mobile (Expo + React Native + NativeWind).

**The cardinal rule:** All color, spacing, radius, shadow, z-index, font, duration, and easing values must use `cv-*` semantic tokens. No hardcoded values in component code.

## Reference Files

Load specific reference files based on what you're working on:

### Theme System (core)
- [theme-system.md](theme-system.md) — 3-layer token architecture: Primitive → Semantic → Component
- [primitives.md](primitives.md) — Raw color scales (navy, teal, gray, red, amber, green + channels)
- [semantic-light.md](semantic-light.md) — 22 semantic tokens mapped to light mode primitives
- [semantic-dark.md](semantic-dark.md) — 22 semantic tokens mapped to dark mode primitives
- [tokens-tailwind.md](tokens-tailwind.md) — Complete Tailwind config extension (shared web + NativeWind)
- [tokens-css.md](tokens-css.md) — CSS custom properties with :root, dark theme, and prefers-color-scheme
- [tokens-rn.md](tokens-rn.md) — TypeScript constants for React Native StyleSheet.create fallback

### Visual Foundations
- [color.md](color.md) — Color principles, semantic usage, and WCAG AA requirements
- [contrast-matrix.md](contrast-matrix.md) — Verified contrast ratios for all token pair combinations
- [typography.md](typography.md) — Type scale (Display→Code), Inter/JetBrains Mono, fluid clamp() web, Dynamic Type mobile
- [layout.md](layout.md) — Breakpoints (sm-2xl), z-index scale (100-700), spacing (4px base), safe areas
- [motion.md](motion.md) — Duration tokens (150/300/500ms), easing, haptic table, reduced-motion
- [materials-and-depth.md](materials-and-depth.md) — 5 elevation levels, shadow system, translucency
- [iconography.md](iconography.md) — Lucide icons, 20px/1.5px stroke, 44px touch targets

### Behavioral Foundations
- [accessibility.md](accessibility.md) — WCAG 2.1 AA, ARIA patterns, keyboard, focus, VoiceOver/TalkBack
- [privacy.md](privacy.md) — Privacy as design principle, permission patterns, data visibility
- [writing.md](writing.md) — Voice & tone, terminology table (Deploy→Go live, Agent→Assistant, etc.)

## Quick Reference: Semantic Token Map

| Token | Purpose |
|-------|---------|
| `cv-bg-background` | Page-level background |
| `cv-bg-surface` | Card/panel background |
| `cv-bg-surface-elevated` | Elevated panels, popovers |
| `cv-bg-surface-sunken` | Inset/recessed areas |
| `cv-bg-accent` | Brand teal accent |
| `cv-bg-primary` | Midnight navy primary |
| `cv-text-primary` | Main body text |
| `cv-text-secondary` | Supporting text |
| `cv-text-tertiary` | Placeholder/hint text |
| `cv-text-on-accent` | Text on accent backgrounds |
| `cv-text-on-primary` | Text on primary backgrounds |
| `cv-text-link` | Interactive text links |
| `cv-border-default` | Standard borders |
| `cv-border-subtle` | Dividers, separators |
| `cv-status-success` | Success states (green) |
| `cv-status-warning` | Warning states (amber) |
| `cv-status-error` | Error states (red) |

## Tech Stack

- **Web:** Next.js + Tailwind CSS + shadcn/ui
- **Mobile:** Expo + React Native + NativeWind (Tailwind for RN)
- **Shared:** `cv-*` token classes work on both platforms via NativeWind
