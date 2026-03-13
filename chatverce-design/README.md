# chatverce-design

Jony Ive-inspired design system and brand identity plugin for Chatverce — the no-code AI agent builder platform.

## What It Does

This Claude Code plugin embeds Chatverce's design philosophy, brand tokens, component patterns, and voice guidelines into Claude's design decisions. When working on Chatverce UI code, Claude automatically loads the relevant skills and produces code that follows the brand identity.

## Who It's For

- **Claude Code** — auto-loads skills when working on Chatverce UI code
- **Chatverce developers** — use `/chatverce-design:design-review` to audit UI compliance

## Skills

| Skill | Description |
|-------|-------------|
| `design-soul` | Core design philosophy — Jony Ive-inspired principles, the Ive Test, anti-patterns |
| `brand-identity` | Brand tokens — colors, typography, spacing, shadows, radius, iconography |
| `component-patterns` | UI component rules — buttons, cards, forms, navigation, modals, agent builder |
| `voice-and-tone` | Copy guidelines — voice attributes, writing rules, terminology dictionary |

All skills are agent-invoked (Claude loads them automatically based on task context).

## Command

### `/chatverce-design:design-review [target]`

Audits UI code against the full design system. Accepts a file path, directory, or component name.

```bash
/chatverce-design:design-review src/components/inbox
/chatverce-design:design-review ThreadCard
/chatverce-design:design-review app/(private)/dashboard
```

Produces a structured report with findings grouped by severity (Must fix / Should fix / Consider).

## Installation

```bash
# Add the Sihari Labs marketplace (from inside Claude Code)
/plugin marketplace add siharilabs/plugins

# Install (from inside a Chatverce project)
/plugin install chatverce-design@siharilabs
```

For local development:

```bash
claude --plugin-dir ./chatverce-design
```

## Brand Overview

- **Colors**: Midnight Navy (`#1B2A4A`) + Chatverce Teal (`#2EC4B6`) — intelligence, not messaging
- **Typography**: Inter (sans-serif), JetBrains Mono (code)
- **Spacing**: 4px base unit (4 · 8 · 12 · 16 · 20 · 24 · 32 · 40 · 48 · 64 · 80 · 96)
- **Radius**: 6px (sm) · 8px (md) · 12px (lg) · 9999px (full)
- **Shadows**: Minimal, elevation-only (sm / md / lg)

See the `brand-identity` skill for full token tables and implementation-ready code.

## Plugin Conflict Note

The official `frontend-design` plugin advises against Inter as a font. When both plugins are active, `chatverce-design` takes precedence for Chatverce projects — Inter is the deliberate brand choice.

## Migration Path

The plugin provides the source of truth for design tokens. These existing files in the Chatverce repo will eventually be updated to match:

- `web/app/globals.css` — CSS variables
- `web/tailwind.config.js` — Tailwind theme
- `web/lib/themeConfig.ts` — MUI theme
- `mobile/constants/design-system.ts` — Mobile tokens
- `mobile/constants/theme.ts` — Mobile theme

The plugin itself does not modify these files — it provides the reference that developers and Claude use when making changes.
