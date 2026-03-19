---
name: design-enforcement
description: "4 CDG enforcement guards: token-guard (no hardcoded values), theme-guard (dark/light compliance), a11y-guard (WCAG AA accessibility), pattern-guard (UX patterns and terminology). Use when implementing or reviewing Chatverce UI code."
user_invocable: false
---

# Chatverce Design Enforcement Guards

4 enforcement guards that actively prevent design system violations during implementation and code review. Each guard is a self-contained reference with complete violation patterns and exceptions.

## Guard Reference Files

- [token-guard.md](token-guard.md) — Catches hardcoded hex, rgb, hsl, z-index, font, shadow, radius, spacing, duration, easing values. All must use `cv-*` tokens.
- [theme-guard.md](theme-guard.md) — Catches light-only backgrounds, missing dark adaptation, OLED pure black, `dark:` prefix in component code.
- [a11y-guard.md](a11y-guard.md) — Web: 10 violation types (labels, alt, touch targets, focus, keyboard). Mobile: 6 violation types (VoiceOver, TalkBack).
- [pattern-guard.md](pattern-guard.md) — Terminology enforcement, multiple primary buttons, modal on mobile (use drawer), floating labels, missing haptics, decorative animation.

## Quick Violation Reference

### Token Guard — Must use `cv-*` tokens
| Violation | Example | Fix |
|-----------|---------|-----|
| Hardcoded hex | `#1B2A4A` | `bg-cv-primary` |
| Hardcoded rgb | `rgb(46, 196, 182)` | `bg-cv-accent` |
| Arbitrary z-index | `z-[999]` | `z-cv-modal` (500) |
| Hardcoded spacing | `p-[13px]` | `p-cv-space-3` (12px) |
| Hardcoded font | `font-['Inter']` | `font-cv-sans` |
| Hardcoded duration | `duration-200` | `duration-cv-normal` (300ms) |
| Hardcoded shadow | `shadow-[custom]` | `shadow-cv-md` |

### Theme Guard — Both themes must work
| Violation | Fix |
|-----------|-----|
| `dark:bg-gray-900` in component | Use semantic token `bg-cv-background` (auto-switches) |
| `bg-white` hardcoded | Use `bg-cv-surface` |
| Light-only background check | Verify with both `data-theme="light"` and `data-theme="dark"` |

### A11y Guard — WCAG 2.1 AA floor
| Violation | Fix |
|-----------|-----|
| Input without label | Add `<label>` with `htmlFor` |
| Image without alt | Add descriptive `alt` text |
| Touch target < 44px | Ensure min 44x44px tap area |
| Missing focus ring | Add `focus-visible:ring-2 ring-cv-accent` |
| Color-only meaning | Add text/icon alongside color |

### Pattern Guard — Chatverce UX rules
| Violation | Fix |
|-----------|-----|
| "Deploy" in UI | Use "Go live" |
| "Agent" in UI | Use "Assistant" |
| Modal on mobile | Use drawer/bottom sheet |
| Floating label | Label above input |
| Multiple primary buttons | One primary CTA per screen |
| Missing empty state | Add welcoming empty state |
| Decorative animation | Remove or make functional |
