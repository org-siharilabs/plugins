# Chatverce Design Plugin v2.0 — Production Stabilization

**Date:** 2026-03-19
**Status:** Approved
**Supersedes:** `2026-03-13-chatverce-design-plugin-design.md` (v1.0)

## Problem

The chatverce-design plugin (v1.0) has critical gaps that prevent production use:

- **No dark mode** — Colors are hardcoded hex values with no theme switching
- **No semantic token architecture** — Code uses raw primitives; no intent-based abstraction
- **11% component coverage** — 8 of 71 standard UI components specified
- **No enforcement** — Plugin teaches rules but doesn't prevent violations during implementation
- **No accessibility system** — WCAG mentioned but no contrast matrix, ARIA patterns, or keyboard specs
- **Missing foundational specs** — No breakpoints, z-index scale, motion tokens, or icon standards

Claude uses this plugin as its "design brain" during feature implementation (brainstorm → plan → build workflow). Every gap in the plugin is a gap in Claude's output.

## Goals

1. Full dark/light theme support via 3-layer token architecture
2. All 71 component.gallery components + 6 Chatverce-specific components specified
3. Enforcement skills that prevent violations during implementation
4. Complete accessibility system (contrast matrix, ARIA, keyboard, focus, reduced-motion)
5. Clean break from v1 — Chatverce codebase will be updated to match

## Non-Goals

- Figma integration or design tool exports
- RTL/internationalization (future scope)
- Component library code (plugin defines specs, not implementations)
- Visual regression testing setup

## Tech Stack

- **Web:** Next.js + Tailwind + shadcn/ui
- **Mobile:** Expo / React Native
- **Token outputs:** CSS custom properties, Tailwind config, React Native constants
- **MUI dropped** — not in use

---

## Architecture: Three-Layer Token System

### Layer 1 — Primitives

Raw values. Never referenced in application code. Only place hex values exist.

Full scales (50–950) for each hue:

| Hue | Purpose |
|-----|---------|
| `cv-navy-*` | Brand primary, dark theme backgrounds |
| `cv-teal-*` | Brand accent, interactive elements |
| `cv-gray-*` | Neutrals, light theme backgrounds, text |
| `cv-red-*` | Error, destructive states |
| `cv-amber-*` | Warning states |
| `cv-green-*` | Success states |

Each scale: `50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950`

### Layer 2 — Semantic

Intent-based tokens that switch values per theme. This is what code references.

**Background tokens:**

| Token | Light | Dark |
|-------|-------|------|
| `--cv-bg-primary` | `cv-gray-50` | `cv-navy-950` |
| `--cv-bg-surface` | `cv-white` | `cv-navy-900` |
| `--cv-bg-surface-elevated` | `cv-white` | `cv-navy-800` |
| `--cv-bg-accent` | `cv-teal-500` | `cv-teal-400` |
| `--cv-bg-accent-subtle` | `cv-teal-50` | `cv-teal-950` |

**Text tokens:**

| Token | Light | Dark |
|-------|-------|------|
| `--cv-text-primary` | `cv-gray-900` | `cv-gray-50` |
| `--cv-text-secondary` | `cv-gray-500` | `cv-gray-400` |
| `--cv-text-tertiary` | `cv-gray-400` | `cv-gray-500` |
| `--cv-text-on-accent` | `cv-white` | `cv-navy-950` |
| `--cv-text-link` | `cv-teal-600` | `cv-teal-400` |

**Border tokens:**

| Token | Light | Dark |
|-------|-------|------|
| `--cv-border-default` | `cv-gray-200` | `cv-navy-700` |
| `--cv-border-strong` | `cv-gray-300` | `cv-navy-600` |
| `--cv-border-accent` | `cv-teal-500` | `cv-teal-400` |

**Status tokens:**

| Token | Light | Dark |
|-------|-------|------|
| `--cv-status-success` | `cv-green-500` | `cv-green-400` |
| `--cv-status-warning` | `cv-amber-500` | `cv-amber-400` |
| `--cv-status-error` | `cv-red-500` | `cv-red-400` |
| `--cv-status-info` | `cv-teal-500` | `cv-teal-400` |

**Overlay/shadow:**

| Token | Light | Dark |
|-------|-------|------|
| `--cv-shadow-color` | `0,0,0` | `0,0,0` |
| `--cv-overlay` | `cv-gray-900/50%` | `cv-black/60%` |

**Non-color semantic tokens (same in both themes):**

| Category | Tokens |
|----------|--------|
| Typography | `--cv-font-sans: 'Inter', system-ui, sans-serif`, `--cv-font-mono: 'JetBrains Mono', monospace` |
| Radius | `--cv-radius-sm: 6px`, `--cv-radius-md: 8px`, `--cv-radius-lg: 12px`, `--cv-radius-full: 9999px` |
| Spacing | `--cv-space-1: 4px` through `--cv-space-24: 96px` (4px base: 4·8·12·16·20·24·32·40·48·64·80·96) |
| Shadows | `--cv-shadow-sm`, `--cv-shadow-md`, `--cv-shadow-lg` (using `--cv-shadow-color` for theme adaptation) |
| Z-index | `--cv-z-dropdown: 100`, `--cv-z-sticky: 200`, `--cv-z-overlay: 300`, `--cv-z-modal: 400`, `--cv-z-popover: 500`, `--cv-z-toast: 600`, `--cv-z-tooltip: 700` |
| Motion | `--cv-duration-fast: 150ms`, `--cv-duration-normal: 300ms`, `--cv-duration-slow: 500ms` |
| Easing | `--cv-ease-default: cubic-bezier(0.4, 0, 0.2, 1)`, `--cv-ease-out: cubic-bezier(0, 0, 0.2, 1)`, `--cv-ease-in: cubic-bezier(0.4, 0, 1, 1)` |
| Breakpoints | `--cv-breakpoint-sm: 640px`, `--cv-breakpoint-md: 768px`, `--cv-breakpoint-lg: 1024px`, `--cv-breakpoint-xl: 1280px`, `--cv-breakpoint-2xl: 1536px` |

### Layer 3 — Component

Specific to each component, references semantic tokens. Defined within each component's SKILL.md, not centrally.

Example:
```
--cv-button-primary-bg: var(--cv-bg-accent)
--cv-button-primary-text: var(--cv-text-on-accent)
--cv-card-bg: var(--cv-bg-surface)
--cv-card-border: var(--cv-border-default)
--cv-input-focus-border: var(--cv-border-accent)
```

### Token Rule

Code must reference semantic or component tokens. Never primitives. Never raw hex. Only exceptions: primitive definition files, third-party overrides with `/* cv-override */`, one-off values with `/* cv-exception: reason */`.

---

## Plugin File Structure

```
chatverce-design/
  .claude-plugin/
    plugin.json

  skills/
    # ─── FOUNDATIONS (always-on for Chatverce repos) ───
    foundations/
      design-soul/
        SKILL.md                    # 5 principles, Ive Test, anti-patterns
        ive-philosophy.md           # Deep Ive reference (kept as-is)
      theme-system/
        SKILL.md                    # 3-layer architecture explained
        primitives.md               # Full primitive color scales
        semantic-light.md           # Light theme semantic mappings
        semantic-dark.md            # Dark theme semantic mappings
        tokens-css.md               # CSS custom properties
        tokens-tailwind.md          # Tailwind config
        tokens-rn.md                # React Native constants
      accessibility/
        SKILL.md                    # WCAG rules, ARIA patterns, keyboard, focus
        contrast-matrix.md          # Verified contrast ratios for all token pairs
      layout/
        SKILL.md                    # Breakpoints, z-index, grid, spacing
      motion/
        SKILL.md                    # Duration/easing tokens, transitions, reduced-motion
      voice-and-tone/
        SKILL.md                    # Writing rules, terminology, tone by context
      iconography/
        SKILL.md                    # Lucide standards, sizing, stroke, pairing

    # ─── COMPONENTS (activated by file path — UI files only) ───
    components/
      catalog.md                    # Index of all 77 components
      accordion/SKILL.md
      alert/SKILL.md
      avatar/SKILL.md
      badge/SKILL.md
      breadcrumbs/SKILL.md
      button/SKILL.md
      button-group/SKILL.md
      card/SKILL.md
      carousel/SKILL.md
      checkbox/SKILL.md
      color-picker/SKILL.md
      combobox/SKILL.md
      date-input/SKILL.md
      datepicker/SKILL.md
      drawer/SKILL.md
      dropdown-menu/SKILL.md
      empty-state/SKILL.md
      fieldset/SKILL.md
      file/SKILL.md
      file-upload/SKILL.md
      footer/SKILL.md
      form/SKILL.md
      header/SKILL.md
      heading/SKILL.md
      hero/SKILL.md
      icon/SKILL.md
      image/SKILL.md
      label/SKILL.md
      link/SKILL.md
      list/SKILL.md
      modal/SKILL.md
      navigation/SKILL.md
      pagination/SKILL.md
      popover/SKILL.md
      progress-bar/SKILL.md
      progress-indicator/SKILL.md
      quote/SKILL.md
      radio-button/SKILL.md
      rating/SKILL.md
      rich-text-editor/SKILL.md
      search-input/SKILL.md
      segmented-control/SKILL.md
      select/SKILL.md
      separator/SKILL.md
      skeleton/SKILL.md
      skip-link/SKILL.md
      slider/SKILL.md
      spinner/SKILL.md
      stack/SKILL.md
      stepper/SKILL.md
      table/SKILL.md
      tabs/SKILL.md
      text-input/SKILL.md
      textarea/SKILL.md
      toast/SKILL.md
      toggle/SKILL.md
      tooltip/SKILL.md
      tree-view/SKILL.md
      video/SKILL.md
      visually-hidden/SKILL.md
      # Chatverce-specific:
      agent-builder-canvas/SKILL.md
      agent-builder-node/SKILL.md
      ai-working-indicator/SKILL.md
      channel-indicator/SKILL.md
      conversation-thread/SKILL.md
      dashboard-stat-card/SKILL.md

    # ─── ENFORCEMENT (activated during implementation only) ───
    enforcement/
      token-guard/SKILL.md
      theme-guard/SKILL.md
      a11y-guard/SKILL.md
      pattern-guard/SKILL.md

  agents/
    design-reviewer.md

  commands/
    design-review.md
```

### Activation Model

| Layer | Activates When | Trigger |
|-------|---------------|---------|
| Foundations | Chatverce project detected (package.json name, repo name, `.chatverce` marker) | Any file opened |
| Components | Working on UI files | `*.tsx`, `*.css`, `components/**`, `app/**/page.tsx` |
| Enforcement | Writing/editing code (not reading/researching/planning) | Edit/Write tool on UI files |

### Catalog.md

Lightweight index (~150 lines) Claude reads to discover components without loading all 77 skills:

```markdown
| Component | Path | Tier | Aliases | Use When |
|-----------|------|------|---------|----------|
| Accordion | components/accordion/ | Standard | Collapse, Collapsible | Collapsible sections, FAQ |
| Alert | components/alert/ | Full | Notification, Banner | System messages, warnings |
| ...
```

Includes a "Common Compositions" section mapping patterns to component groups (e.g., "Form" = Form + Fieldset + Label + Text input + Button).

---

## Enforcement System

Four enforcement skills that teach Claude what to check as it writes code.

### token-guard

Catches hardcoded values that bypass the token system.

| Violation | Example | Fix |
|-----------|---------|-----|
| Raw hex color | `bg-[#2EC4B6]` | `bg-cv-accent` |
| Arbitrary z-index | `z-[999]` | `z-cv-modal` |
| Arbitrary font | `font-['Helvetica']` | `font-cv-sans` |
| Arbitrary shadow | `shadow-[0_4px_12px...]` | `shadow-cv-md` |
| Arbitrary radius | `rounded-[10px]` | `rounded-cv-md` |
| Arbitrary spacing outside scale | `p-[13px]` | `p-3` |
| Arbitrary duration | `duration-[200ms]` | `duration-cv-fast` |
| Arbitrary easing | `ease-[cubic-bezier(...)]` | `ease-cv-default` |

**Exceptions:** Primitive definition files, `/* cv-override */` comments, `/* cv-exception: reason */` comments, SVG/image data.

### theme-guard

Catches patterns that break in one theme.

| Violation | Example | Fix |
|-----------|---------|-----|
| Light-only background | `bg-white` hardcoded | `bg-cv-surface` |
| Missing dark adaptation | `bg-gray-50` without dark variant | `bg-cv-bg-primary` |
| Shadow invisible in dark mode | Raw shadow rgba | `shadow-cv-sm` |
| Low contrast in one theme | Text passes in light, fails in dark | Reference contrast-matrix.md |

### a11y-guard

Catches accessibility violations.

| Violation | Example | Fix |
|-----------|---------|-----|
| Missing input label | `<input>` without `<label>` | Add label above input |
| Image without alt | `<img>` without `alt` | Add descriptive alt |
| Button without name | `<button><Icon /></button>` | Add `aria-label` |
| Touch target < 44px | `w-6 h-6` clickable | Min `w-11 h-11` |
| Color-only meaning | Red text as sole error indicator | Add icon + text |
| Missing focus-visible | Custom component no focus ring | Add `focus-visible:ring-2` |
| Missing keyboard nav | Custom dropdown no arrow keys | Follow WAI-ARIA pattern |
| No skip-link | Page layout without skip-link | Add skip-link component |
| Missing reduced-motion | Animation without `motion-safe:` | Wrap in `motion-safe:` |
| Heading hierarchy skip | `<h1>` then `<h3>` | Sequential levels |

### pattern-guard

Catches convention violations.

| Violation | Example | Fix |
|-----------|---------|-----|
| Multiple primary buttons | Two primary buttons in view | One primary per screen |
| Floating labels | Placeholder as label | Label above, always visible |
| Technical jargon in UI | "Deploy agent" | "Go live" |
| Raw error message | `{error.message}` rendered | Solution-focused copy |
| Decorative animation | `animate-bounce` | Motion serves comprehension |
| Non-catalog component | Custom `<Popup>` | Use Popover or Modal |
| Missing empty state | List with no empty handling | Add empty state |

### Enforcement Flow

```
Claude receives implementation task
  → Foundations load (theme-system, layout, etc.)
  → Claude identifies needed components via catalog
  → Component skills load
  → Claude writes code
  → Enforcement skills activate on write/edit
  → Violations self-corrected before saving
  → Code is clean on first write
```

---

## Accessibility System

### Contrast Matrix

Every semantic text/background pair verified against WCAG AA:

- **4.5:1** minimum for body text (14-16px)
- **3:1** minimum for large text (18px+ or 14px bold)

Pairs that don't meet AA get adjusted at the primitive level. Claude never creates a text/background combination not in the matrix.

### ARIA Pattern Mapping

Each component maps to its WAI-ARIA pattern:

| Component | Pattern | Key Attributes |
|-----------|---------|----------------|
| Accordion | `role="region"`, `aria-expanded` | `aria-controls`, `aria-labelledby` |
| Alert | `role="alert"`, `aria-live="assertive"` | Auto-announces |
| Modal | `role="dialog"`, `aria-modal="true"` | Focus trap |
| Toast | `role="status"`, `aria-live="polite"` | Non-interrupting announcement |
| Tabs | `role="tablist/tab/tabpanel"` | Arrow keys, `aria-selected` |
| Drawer | `role="dialog"` | Focus trap like modal |
| Dropdown menu | `role="menu/menuitem"` | Arrow keys, `aria-expanded` |
| Toggle | `role="switch"` | `aria-checked` |
| Skeleton | `aria-busy="true"` | Container marked busy |
| Progress bar | `role="progressbar"` | `aria-valuenow/min/max` |

Full mapping covers all 77 components.

### Keyboard Interaction

| Key | Universal Behavior |
|-----|-------------------|
| `Tab` / `Shift+Tab` | Move focus forward/backward |
| `Enter` / `Space` | Activate button, link, control |
| `Escape` | Close modal, popover, dropdown, drawer, toast |
| `Arrow keys` | Navigate within composite widgets |
| `Home` / `End` | First/last item in lists |

### Focus Management

| Event | Focus Behavior |
|-------|---------------|
| Modal/Drawer open | Focus → first focusable element, trapped |
| Modal/Drawer close | Focus → triggering element |
| Toast appears | No focus steal, `aria-live` announces |
| Route change | Focus → main content or page heading |
| Dropdown open | Focus → first item, arrow nav |

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  --cv-duration-fast: 0ms;
  --cv-duration-normal: 0ms;
  --cv-duration-slow: 0ms;
}
```

Tailwind: always `motion-safe:` prefix on animations.

---

## Component Depth Tiers

### Full (~400-600 words)

All template sections, multiple variants, detailed code example, interaction states.

**Components:** Accordion, Alert, Avatar, Badge, Button, Card, Checkbox, Combobox, Datepicker, Drawer, Dropdown menu, Empty state, File upload, Form, Modal, Navigation, Popover, Progress indicator, Radio button, Search input, Select, Skeleton, Table, Tabs, Text input, Textarea, Toast, Toggle, Tooltip, Agent builder canvas, Agent builder node

### Standard (~200-350 words)

All template sections, concise. 1-2 variants, shorter example.

**Components:** Breadcrumbs, Button group, Fieldset, Footer, Header, Heading, Hero, Icon, Image, Label, Link, List, Pagination, Progress bar, Segmented control, Separator, Slider, Spinner, Stepper, Tree view, AI working indicator, Channel indicator, Conversation thread, Dashboard stat card

### Minimal (~100-150 words)

When to use, styling, accessibility, single example.

**Components:** Carousel, Color picker, Date input, File, Quote, Rating, Rich text editor, Skip link, Stack, Video, Visually hidden

### Component Skill Template

```markdown
---
name: {component-name}
description: {one-liner}
user_invocable: false
---

# {Component Name}

**Gallery reference:** https://component.gallery/components/{name}/
**Aliases:** {alt names}

## When to Use
## When NOT to Use
## Anatomy
## Variants
## Chatverce Styling (component tokens)
## Accessibility (ARIA, keyboard, focus)
## Code Example (shadcn + Tailwind + cv-tokens)
## Anti-Patterns
```

### Code Example Standard

Every example uses:
- shadcn/ui base + Tailwind + cv-* semantic tokens
- Theme-aware (no hardcoded light/dark — tokens handle it)
- Accessible (ARIA, keyboard, focus-visible)
- Chatverce terminology in visible text
- `motion-safe:` prefix on transitions
- Minimum 44px touch targets

---

## Transition from v1.0

### Kept As-Is

| File | Destination |
|------|-------------|
| `ive-philosophy.md` | `foundations/design-soul/ive-philosophy.md` |

### Refined

| Current | Destination | Changes |
|---------|-------------|---------|
| `design-soul/SKILL.md` | `foundations/design-soul/SKILL.md` | Add token system references, remove hardcoded colors |
| `voice-and-tone/SKILL.md` | `foundations/voice-and-tone/SKILL.md` | Deduplicate terminology, single source of truth |

### Replaced

| Current | Absorbed Into |
|---------|--------------|
| `brand-identity/SKILL.md` | `theme-system/` + `iconography/` + `layout/` |
| `brand-identity/tokens.md` | `theme-system/tokens-*.md` (rebuilt, MUI dropped) |
| `component-patterns/SKILL.md` | Individual component skills + `motion/` + `voice-and-tone/` |
| `component-patterns/examples.md` | Embedded in each component SKILL.md |

### Metrics

| | v1.0 | v2.0 |
|-|------|------|
| Foundation skills | 4 | 7 |
| Component specs | 12 (1 file) | 77 (individual) |
| Enforcement skills | 0 | 4 |
| Token layers | 1 (flat) | 3 (primitive → semantic → component) |
| Themes | Light | Light + Dark |
| Token formats | CSS, Tailwind, TS, MUI | CSS, Tailwind, React Native |
| Accessibility | Mentioned | Full system |
| Total files | 12 | ~95 |
