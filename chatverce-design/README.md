# Chatverce Design Guidelines (CDG)

Agent-agnostic design system for building Chatverce across web and mobile. Currently delivered as a Claude Code plugin.

CDG gives AI agents (and the humans working alongside them) a single, authoritative source of truth for every design decision in Chatverce — from philosophy to pixel.

## Installation

```bash
# Add the Sihari Labs marketplace (run inside Claude Code)
/plugin marketplace add org-siharilabs/plugins

# Install the plugin (run inside a Chatverce project)
/plugin install chatverce-design@siharilabs
```

For local development:

```bash
claude --plugin-dir ./chatverce-design
```

## Structure

CDG v2.0 is organized into five layers, applied in order of specificity:

| Layer | Count | Purpose |
|-------|-------|---------|
| **Philosophy** | 1 skill | Design ethos — the "why" behind every decision |
| **Foundations** | 10 skills | Core systems: color, typography, spacing, motion, accessibility, and more |
| **Patterns** | 14 skills | Interaction patterns: onboarding, forms, loading, feedback, real-time, and more |
| **Components** | 66 components | Detailed rules for every UI component in the design system |
| **Enforcement** | 4 guards | Active checks: token-guard, theme-guard, pattern-guard, a11y-guard |

## For AI Agents

Skills activate automatically based on context — no explicit loading required:

- **Philosophy** — always-on; applies to every design decision
- **Foundations** — loaded on Chatverce project detection
- **Patterns** — loaded by feature context (e.g. onboarding flow triggers the onboarding pattern)
- **Components** — loaded by UI file path (e.g. editing a button component loads the button skill)
- **Enforcement** — runs during implementation to catch token misuse, theme violations, accessibility issues, and pattern deviations

## For Developers

### `/chatverce-design:design-review <target>`

Audits UI code against all CDG layers. Accepts a file path, directory, or component name.

```bash
/chatverce-design:design-review src/components/inbox
/chatverce-design:design-review ThreadCard
/chatverce-design:design-review app/(private)/dashboard
```

Produces a structured report with findings grouped by severity: **Must fix** / **Should fix** / **Consider**.

## Design Philosophy

CDG is rooted in six ethos principles, inspired by Jony Ive's approach to craft:

1. **Caring is the X-Factor** — Users perceive care at a level deeper than language; every state and edge case is an opportunity to demonstrate it.
2. **Inevitable Simplicity** — The best solution feels like it was discovered, not designed; the interface disappears and leaves only the user's goal.
3. **Breathing Room** — White space is not empty; it is an invitation that communicates confidence and gives the eye a place to rest.
4. **Focus as Discipline** — Every screen has one job; adding a feature without removing complexity elsewhere is a design failure.
5. **Emotional Intention** — Before designing a screen, name the feeling it should evoke; that feeling is a specification, not a nice-to-have.
6. **Thoroughness in Every Inch** — A component is not done until every state, edge case, and interaction mode has been considered.

The full manifesto lives in `skills/philosophy/design-ethos/SKILL.md`.

## Token System

CDG uses a three-layer token architecture:

```
Primitive tokens   →   Semantic tokens   →   Component tokens
(raw values)           (role-based)           (scoped to component)
e.g. #1B2A4A           e.g. bg-primary         e.g. button-bg-primary
```

**Rule: always use semantic tokens in code.** Never reference primitive values directly. The semantic layer is what makes dark and light mode work automatically — switching themes is a context change, not a code change.

Dark mode and light mode are both first-class citizens. All tokens resolve correctly in both contexts with zero per-component overrides.

## Tech Stack

| Platform | Stack |
|----------|-------|
| **Web** | Next.js + Tailwind CSS + shadcn/ui |
| **Mobile** | Expo + NativeWind |
| **Shared** | Tailwind config as the single source for design tokens |

The Tailwind config is the bridge. Web and mobile consume the same token values — typography, spacing, color, radius, and shadow — through platform-appropriate APIs.

## Brand Quick Reference

| Element | Value |
|---------|-------|
| **Primary** | Midnight Navy `#1B2A4A` |
| **Accent** | Chatverce Teal `#2EC4B6` |
| **Sans-serif** | Inter |
| **Monospace** | JetBrains Mono |
| **Spacing base** | 4px (scale: 4 · 8 · 12 · 16 · 20 · 24 · 32 · 40 · 48 · 64 · 80 · 96) |
| **Radius** | 6px (sm) · 8px (md) · 12px (lg) · 9999px (full) |

Navy signals intelligence and trust. Teal signals action and capability. Together they communicate: *a serious platform that is easy to use.*
