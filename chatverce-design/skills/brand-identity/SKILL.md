---
name: brand-identity
description: Chatverce brand identity tokens — colors, typography, spacing, shadows, border radius, and iconography. Use when writing CSS, Tailwind classes, styling components, choosing colors, setting fonts, or implementing any visual design for Chatverce.
user-invocable: false
---

### Color System

Derived from the "C" mark logo (navy + teal), extended for a complete AI product palette:

| Role | Name | Hex | Usage |
|------|------|-----|-------|
| Primary | Midnight Navy | `#1B2A4A` | Headers, primary text, key actions |
| Accent | Chatverce Teal | `#2EC4B6` | CTAs, active states, links, agent status |
| Background | Snow | `#FAFBFC` | Page backgrounds |
| Surface | Cloud | `#F1F3F5` | Cards, panels, input fields |
| Surface Elevated | White | `#FFFFFF` | Modals, dropdowns, floating elements |
| Text Primary | Ink | `#1A1D23` | Body text, headings |
| Text Secondary | Slate | `#64748B` | Descriptions, labels, timestamps |
| Text Tertiary | Mist | `#94A3B8` | Placeholders, disabled text |
| Border | Frost | `#E2E8F0` | Dividers, input borders |
| Success | Meadow | `#10B981` | Agent live, task complete |
| Warning | Amber | `#F59E0B` | Attention needed |
| Error | Coral | `#EF4444` | Failures, destructive actions |
| AI Glow | Teal 10% | `#2EC4B6` at 10% opacity | Subtle AI-is-working indicator |

**Key rule**: No WhatsApp green as primary. Green is only a channel indicator (WhatsApp: `#25D366`) and success state. The identity is navy + teal — intelligence, not messaging.

**Dark mode**: Derive systematically — invert backgrounds (navy-based dark surfaces), keep accent teal, adjust text to light grays. Not in v1 scope but the token system should accommodate it.

Channel colors:
- WhatsApp: `#25D366`
- Telegram: `#26A5E4`
- Webchat: `#64748B`
- Telephone: `#1B2A4A`
- Email: `#1B2A4A`

### Typography

| Role | Font | Weight | Size | Line Height |
|------|------|--------|------|-------------|
| Display | Inter | 600 (Semibold) | 36-48px | 1.1 |
| Heading 1 | Inter | 600 | 28-32px | 1.2 |
| Heading 2 | Inter | 600 | 20-24px | 1.3 |
| Heading 3 | Inter | 500 (Medium) | 16-18px | 1.4 |
| Body | Inter | 400 (Regular) | 14-16px | 1.5 |
| Caption | Inter | 400 | 12-13px | 1.4 |
| Code/Agent | JetBrains Mono | 400 | 13-14px | 1.5 |

**Why Inter**: Geometric, clean, excellent legibility at small sizes, feels premium without being cold. Draws from the same tradition as San Francisco (Apple's system font). Optimized for screens, neutral but warm.

### Spacing Scale

4px base unit, consistent rhythm:
```
4 · 8 · 12 · 16 · 20 · 24 · 32 · 40 · 48 · 64 · 80 · 96
```

### Border Radius

Soft but not bubbly:
- Small (inputs, badges): `6px`
- Medium (cards, buttons): `8px`
- Large (modals, panels): `12px`
- Full (avatars, pills): `9999px`

### Shadows

Minimal, only for elevation:
- `sm`: `0 1px 2px rgba(0,0,0,0.05)` — subtle lift
- `md`: `0 4px 12px rgba(0,0,0,0.08)` — cards, dropdowns
- `lg`: `0 12px 32px rgba(0,0,0,0.12)` — modals

### Iconography

- Lucide icons — thin, consistent stroke weight
- 20px default size, 1.5px stroke
- Never fill icons unless indicating active/selected state
- Channel icons: WhatsApp (green), Telegram (blue), Webchat (slate), Telephone (navy), Email (navy)

### Logo Usage

- The "C" mark (navy + teal) is the primary symbol. Never modify, recolor, or add effects.
- Minimum clear space: the width of the "C" opening on all sides
- On dark backgrounds: white version. On light: navy version.
- Full wordmark "ChatVerce" uses Inter Semibold, capital C and V, rest lowercase

For implementation-ready code, see [tokens.md](tokens.md)
