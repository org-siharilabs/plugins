# Chatverce Design Plugin — Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a Claude Code plugin (`chatverce-design`) that embeds Jony Ive's design philosophy and Chatverce brand identity into Claude's design decisions.

**Architecture:** A content-only Claude Code plugin (no application code) containing 4 agent-invoked skills, 1 user-invoked command, and 1 specialized agent. Lives in a new `org-siharilabs/plugins` GitHub repo.

**Tech Stack:** Claude Code plugin system, Markdown (SKILL.md), JSON (plugin.json, marketplace.json)

**Spec:** `docs/superpowers/specs/2026-03-13-chatverce-design-plugin-design.md`

---

## File Structure

```
/Users/sandeep/Desktop/dev/org-siharilabs/plugins/
├── README.md                                        # Marketplace overview
├── marketplace.json                                 # Plugin registry
├── chatverce-design/
│   ├── .claude-plugin/
│   │   └── plugin.json                              # Plugin manifest
│   ├── skills/
│   │   ├── design-soul/
│   │   │   ├── SKILL.md                             # Core philosophy (~120 lines)
│   │   │   └── ive-philosophy.md                    # Deep reference (~300 lines)
│   │   ├── brand-identity/
│   │   │   ├── SKILL.md                             # Tokens & rules (~150 lines)
│   │   │   └── tokens.md                            # Code-ready tokens (~250 lines)
│   │   ├── component-patterns/
│   │   │   ├── SKILL.md                             # UI patterns (~200 lines)
│   │   │   └── examples.md                          # Code snippets (~300 lines)
│   │   └── voice-and-tone/
│   │       └── SKILL.md                             # Copy guidelines (~120 lines)
│   ├── commands/
│   │   └── design-review.md                         # Review command (~60 lines)
│   ├── agents/
│   │   └── design-reviewer.md                       # Review agent (~80 lines)
│   └── README.md                                    # Plugin docs
```

Each file has one clear responsibility. Skills are self-contained but cross-reference each other via markdown links.

---

## Chunk 1: Repository Scaffolding

### Task 1: Create repo and directory structure

**Files:**
- Create: `plugins/README.md`
- Create: `plugins/marketplace.json`
- Create: `plugins/chatverce-design/.claude-plugin/plugin.json`
- Create: `plugins/chatverce-design/README.md`

**Context:** The `plugins` repo lives at `/Users/sandeep/Desktop/dev/org-siharilabs/plugins`. This is a brand new repo — not inside the chatverce repo.

- [ ] **Step 1: Create the directory structure**

```bash
mkdir -p /Users/sandeep/Desktop/dev/org-siharilabs/plugins/chatverce-design/.claude-plugin
mkdir -p /Users/sandeep/Desktop/dev/org-siharilabs/plugins/chatverce-design/skills/design-soul
mkdir -p /Users/sandeep/Desktop/dev/org-siharilabs/plugins/chatverce-design/skills/brand-identity
mkdir -p /Users/sandeep/Desktop/dev/org-siharilabs/plugins/chatverce-design/skills/component-patterns
mkdir -p /Users/sandeep/Desktop/dev/org-siharilabs/plugins/chatverce-design/skills/voice-and-tone
mkdir -p /Users/sandeep/Desktop/dev/org-siharilabs/plugins/chatverce-design/commands
mkdir -p /Users/sandeep/Desktop/dev/org-siharilabs/plugins/chatverce-design/agents
```

- [ ] **Step 2: Initialize git repo**

```bash
cd /Users/sandeep/Desktop/dev/org-siharilabs/plugins
git init
```

- [ ] **Step 3: Create marketplace.json**

Write `/Users/sandeep/Desktop/dev/org-siharilabs/plugins/marketplace.json`:

```json
{
  "plugins": [
    {
      "name": "chatverce-design",
      "description": "Jony Ive-inspired design system and brand identity for Chatverce",
      "source": "./chatverce-design",
      "version": "1.0.0"
    }
  ]
}
```

- [ ] **Step 4: Create plugin.json**

Write `/Users/sandeep/Desktop/dev/org-siharilabs/plugins/chatverce-design/.claude-plugin/plugin.json`:

```json
{
  "name": "chatverce-design",
  "description": "Jony Ive-inspired design system and brand identity for Chatverce — the no-code AI agent builder platform",
  "version": "1.0.0",
  "author": {
    "name": "Sihari Labs",
    "email": "admin@siharilabs.com"
  },
  "repository": "https://github.com/org-siharilabs/plugins",
  "license": "Proprietary",
  "keywords": ["design-system", "brand-identity", "chatverce", "ui", "ux"]
}
```

- [ ] **Step 5: Create repo README.md**

Write `/Users/sandeep/Desktop/dev/org-siharilabs/plugins/README.md`:

```markdown
# Sihari Labs — Claude Code Plugins

A collection of Claude Code plugins by Sihari Labs.

## Available Plugins

| Plugin | Description | Version |
|--------|-------------|---------|
| [chatverce-design](./chatverce-design/) | Jony Ive-inspired design system and brand identity for Chatverce | 1.0.0 |

## Installation

```bash
# Add this marketplace
claude plugin marketplace add --url https://github.com/org-siharilabs/plugins

# Install a plugin (from inside a Chatverce project)
claude plugin install chatverce-design@org-siharilabs-plugins --scope project
```

## Local Development

```bash
# Run from inside the plugins/ repo root
claude --plugin-dir ./chatverce-design
```
```

- [ ] **Step 6: Create plugin README.md**

Write `/Users/sandeep/Desktop/dev/org-siharilabs/plugins/chatverce-design/README.md` with:
- Plugin overview (what it does, who it's for)
- Skills list with descriptions
- Command usage (`/chatverce-design:design-review`)
- Installation instructions
- Brand overview (colors, fonts — brief, link to full skills)
- **Plugin conflict note**: The official `frontend-design` plugin advises against Inter as a font. When both plugins are active, `chatverce-design` takes precedence for Chatverce projects — Inter is the deliberate brand choice.
- **Migration path**: List the existing design system files that the tokens replace:
  - `web/app/globals.css` — CSS variables
  - `web/tailwind.config.js` — Tailwind theme
  - `web/lib/themeConfig.ts` — MUI theme
  - `mobile/constants/design-system.ts` — Mobile tokens
  - `mobile/constants/theme.ts` — Mobile theme

- [ ] **Step 7: Commit scaffolding**

```bash
cd /Users/sandeep/Desktop/dev/org-siharilabs/plugins
git add -A
git commit -m "chore: scaffold plugins repo with chatverce-design structure"
```

---

## Chunk 2: Design Soul Skill

### Task 2: Write the Design Soul skill

**Files:**
- Create: `chatverce-design/skills/design-soul/SKILL.md`
- Create: `chatverce-design/skills/design-soul/ive-philosophy.md`

**Source:** Spec Sections 4 and 5

- [ ] **Step 1: Write SKILL.md**

Write `chatverce-design/skills/design-soul/SKILL.md` with:

Frontmatter:
```yaml
---
name: design-soul
description: Chatverce design philosophy and principles. Use when designing UI components, creating screens, building user interfaces, making layout decisions, choosing visual approaches, or writing user-facing features for the Chatverce product. Provides the core design ethos inspired by Jony Ive.
user-invocable: false
---
```

Body content (from Spec Section 5):
- The Chatverce Design Ethos — all 5 principles with Ive quotes
- Anti-Patterns — the 8 "never do" rules
- The Autopilot Promise
- The Ive Test — 5-question checklist
- Cross-reference: `For the complete philosophy with quotes and case studies, see [ive-philosophy.md](ive-philosophy.md)`
- Cross-reference: `For brand tokens, see the brand-identity skill. For component rules, see the component-patterns skill. For copy guidelines, see the voice-and-tone skill.`

Keep under 500 lines. Every word must earn its place.

- [ ] **Step 2: Write ive-philosophy.md**

Write `chatverce-design/skills/design-soul/ive-philosophy.md` with:

This is the deep reference file. Content from the Jony Ive research:

1. **Ive's Core Philosophy** — organized by the 5 Chatverce principles, with 3-5 quotes each and explanation of how they apply to SaaS UI
2. **Dieter Rams' 10 Principles** — each principle mapped to a specific Chatverce design decision:
   - Innovative → Push manufacturing boundaries (custom tokens, purposeful animation)
   - Useful → "A beautiful product that doesn't work very well is ugly"
   - Aesthetic → Inseparable from function
   - Understandable → iPhone single button → one primary CTA per screen
   - Unobtrusive → UI defers to content
   - Honest → No false decoration, material honesty
   - Long-lasting → Timeless forms over trends
   - Thorough → iPod click sound, watch clasp → attention to micro-interactions
   - Environmentally friendly → Performance-conscious, no bloat
   - As little design as possible → "Less, but better"
3. **Case Studies** — Apple design decisions translated to SaaS:
   - iPod one button → One primary CTA per screen
   - iOS 7 deference → Dashboard foregrounds user data
   - iPhone eliminating on-off → Remove unnecessary confirmations
   - Apple Watch personalization → Assistant personality customization
4. **Evolved Philosophy (2024-2026)**:
   - "Just because something is uncluttered doesn't mean it's good. It can be cold, it can be soulless."
   - Minimalism must have warmth and humanity
   - The OpenAI work: "calmer vibe, like sitting in the most beautiful cabin by a lake"
5. **The Fragility of Ideas** — Why early design concepts need protection

- [ ] **Step 3: Commit**

```bash
git add chatverce-design/skills/design-soul/
git commit -m "feat: add design-soul skill — core philosophy and Ive reference"
```

---

## Chunk 3: Brand Identity Skill

### Task 3: Write the Brand Identity skill

**Files:**
- Create: `chatverce-design/skills/brand-identity/SKILL.md`
- Create: `chatverce-design/skills/brand-identity/tokens.md`

**Source:** Spec Section 6

- [ ] **Step 1: Write SKILL.md**

Write `chatverce-design/skills/brand-identity/SKILL.md` with:

Frontmatter:
```yaml
---
name: brand-identity
description: Chatverce brand identity tokens — colors, typography, spacing, shadows, border radius, and iconography. Use when writing CSS, Tailwind classes, styling components, choosing colors, setting fonts, or implementing any visual design for Chatverce.
user-invocable: false
---
```

Body content (from Spec Section 6):
- Color System table (all 13 roles with name, hex, usage)
- Key rule about WhatsApp green
- Dark mode note
- Typography table (7 roles with font, weight, size, line-height)
- Why Inter explanation
- Spacing Scale
- Border Radius
- Shadows (sm, md, lg)
- Iconography rules
- Logo Usage rules
- Cross-reference: `For implementation-ready code, see [tokens.md](tokens.md)`

- [ ] **Step 2: Write tokens.md**

Write `chatverce-design/skills/brand-identity/tokens.md` with four sections:

**Section 1: CSS Custom Properties**
```css
:root {
  /* Colors */
  --cv-color-primary: #1B2A4A;
  --cv-color-accent: #2EC4B6;
  --cv-color-background: #FAFBFC;
  --cv-color-surface: #F1F3F5;
  --cv-color-surface-elevated: #FFFFFF;
  --cv-color-text-primary: #1A1D23;
  --cv-color-text-secondary: #64748B;
  --cv-color-text-tertiary: #94A3B8;
  --cv-color-border: #E2E8F0;
  --cv-color-success: #10B981;
  --cv-color-warning: #F59E0B;
  --cv-color-error: #EF4444;
  --cv-color-ai-glow: rgba(46, 196, 182, 0.1);

  /* Channel Colors */
  --cv-channel-whatsapp: #25D366;
  --cv-channel-telegram: #26A5E4;
  --cv-channel-webchat: #64748B;
  --cv-channel-telephone: #1B2A4A;
  --cv-channel-email: #1B2A4A;

  /* Typography */
  --cv-font-sans: 'Inter', system-ui, -apple-system, sans-serif;
  --cv-font-mono: 'JetBrains Mono', monospace;

  /* Spacing */
  --cv-space-1: 4px;
  --cv-space-2: 8px;
  --cv-space-3: 12px;
  --cv-space-4: 16px;
  --cv-space-5: 20px;
  --cv-space-6: 24px;
  --cv-space-8: 32px;
  --cv-space-10: 40px;
  --cv-space-12: 48px;
  --cv-space-16: 64px;
  --cv-space-20: 80px;
  --cv-space-24: 96px;

  /* Radius */
  --cv-radius-sm: 6px;
  --cv-radius-md: 8px;
  --cv-radius-lg: 12px;
  --cv-radius-full: 9999px;

  /* Shadows */
  --cv-shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --cv-shadow-md: 0 4px 12px rgba(0, 0, 0, 0.08);
  --cv-shadow-lg: 0 12px 32px rgba(0, 0, 0, 0.12);
}
```

**Section 2: Tailwind Config Extension**
```js
// Add to tailwind.config.js theme.extend
module.exports = {
  theme: {
    extend: {
      colors: {
        cv: {
          primary: '#1B2A4A',
          accent: '#2EC4B6',
          background: '#FAFBFC',
          surface: '#F1F3F5',
          'surface-elevated': '#FFFFFF',
          'text-primary': '#1A1D23',
          'text-secondary': '#64748B',
          'text-tertiary': '#94A3B8',
          border: '#E2E8F0',
          success: '#10B981',
          warning: '#F59E0B',
          error: '#EF4444',
          'ai-glow': 'rgba(46, 196, 182, 0.1)',
        },
        channel: {
          whatsapp: '#25D366',
          telegram: '#26A5E4',
          webchat: '#64748B',
          telephone: '#1B2A4A',
          email: '#1B2A4A',
        },
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', '-apple-system', 'sans-serif'],
        mono: ['JetBrains Mono', 'monospace'],
      },
      borderRadius: {
        cv: {
          sm: '6px',
          md: '8px',
          lg: '12px',
        },
      },
      boxShadow: {
        'cv-sm': '0 1px 2px rgba(0, 0, 0, 0.05)',
        'cv-md': '0 4px 12px rgba(0, 0, 0, 0.08)',
        'cv-lg': '0 12px 32px rgba(0, 0, 0, 0.12)',
      },
    },
  },
};
```

**Section 3: TypeScript Constants (Mobile)**
```typescript
// Replaces mobile/constants/design-system.ts
export const colors = {
  primary: '#1B2A4A',
  accent: '#2EC4B6',
  background: '#FAFBFC',
  surface: '#F1F3F5',
  surfaceElevated: '#FFFFFF',
  textPrimary: '#1A1D23',
  textSecondary: '#64748B',
  textTertiary: '#94A3B8',
  border: '#E2E8F0',
  success: '#10B981',
  warning: '#F59E0B',
  error: '#EF4444',
  aiGlow: 'rgba(46, 196, 182, 0.1)',
  // Channel colors
  channelWhatsapp: '#25D366',
  channelTelegram: '#26A5E4',
  channelWebchat: '#64748B',
  channelTelephone: '#1B2A4A',
  channelEmail: '#1B2A4A',
} as const;

export const spacing = {
  xs: 4, sm: 8, md: 12, lg: 16, xl: 20, '2xl': 24,
  '3xl': 32, '4xl': 40, '5xl': 48, '6xl': 64, '7xl': 80, '8xl': 96,
} as const;

export const radii = {
  sm: 6, md: 8, lg: 12, full: 9999,
} as const;

export const shadows = {
  sm: '0 1px 2px rgba(0, 0, 0, 0.05)',
  md: '0 4px 12px rgba(0, 0, 0, 0.08)',
  lg: '0 12px 32px rgba(0, 0, 0, 0.12)',
} as const;

export const avatarColors = [
  '#ef4444', '#f97316', '#eab308', '#22c55e',
  '#06b6d4', '#3b82f6', '#8b5cf6', '#ec4899',
];

export const channelConfig = {
  whatsapp: { icon: 'logo-whatsapp' as const, color: '#25D366', label: 'WhatsApp' },
  telegram: { icon: 'paper-plane' as const, color: '#26A5E4', label: 'Telegram' },
  webchat: { icon: 'globe-outline' as const, color: '#64748B', label: 'Webchat' },
  telephone: { icon: 'call-outline' as const, color: '#1B2A4A', label: 'Telephone' },
  email: { icon: 'mail-outline' as const, color: '#1B2A4A', label: 'Email' },
} as const;

// Utility functions: getAvatarColor, getInitials (same as current design-system.ts)
```

**Section 4: MUI Theme Override**
```typescript
import { createTheme } from '@mui/material/styles';

export const chatverceTheme = createTheme({
  palette: {
    primary: { main: '#1B2A4A' },
    secondary: { main: '#2EC4B6' },
    background: { default: '#FAFBFC', paper: '#FFFFFF' },
    text: { primary: '#1A1D23', secondary: '#64748B' },
    success: { main: '#10B981' },
    warning: { main: '#F59E0B' },
    error: { main: '#EF4444' },
    divider: '#E2E8F0',
  },
  typography: {
    fontFamily: "'Inter', system-ui, -apple-system, sans-serif",
    h1: { fontSize: '2rem', fontWeight: 600, lineHeight: 1.2 },
    h2: { fontSize: '1.5rem', fontWeight: 600, lineHeight: 1.3 },
    h3: { fontSize: '1.125rem', fontWeight: 500, lineHeight: 1.4 },
    body1: { fontSize: '1rem', fontWeight: 400, lineHeight: 1.5 },
    body2: { fontSize: '0.875rem', fontWeight: 400, lineHeight: 1.5 },
    caption: { fontSize: '0.8125rem', fontWeight: 400, lineHeight: 1.4 },
  },
  shape: { borderRadius: 8 },
  shadows: [
    'none',
    '0 1px 2px rgba(0, 0, 0, 0.05)',   // elevation 1 = cv-sm
    '0 4px 12px rgba(0, 0, 0, 0.08)',   // elevation 2 = cv-md
    '0 12px 32px rgba(0, 0, 0, 0.12)',  // elevation 3 = cv-lg
    // remaining 21 entries: repeat cv-lg for higher elevations
    ...Array(21).fill('0 12px 32px rgba(0, 0, 0, 0.12)'),
  ],
});
```

- [ ] **Step 3: Verify token consistency**

Cross-check every hex value in tokens.md against the SKILL.md color table. They must match exactly. Verify:
- All 13 color roles present in all 4 formats
- Font names identical across formats
- Spacing and radius values identical
- Shadow values identical

- [ ] **Step 4: Commit**

```bash
git add chatverce-design/skills/brand-identity/
git commit -m "feat: add brand-identity skill — tokens in CSS, Tailwind, TS, MUI"
```

---

## Chunk 4: Component Patterns Skill

### Task 4: Write the Component Patterns skill

**Files:**
- Create: `chatverce-design/skills/component-patterns/SKILL.md`
- Create: `chatverce-design/skills/component-patterns/examples.md`

**Source:** Spec Section 7

- [ ] **Step 1: Write SKILL.md**

Write `chatverce-design/skills/component-patterns/SKILL.md` with:

Frontmatter:
```yaml
---
name: component-patterns
description: Chatverce UI component patterns — buttons, cards, forms, navigation, modals, tables, empty states, the agent builder canvas, and motion/animation rules. Use when building React components, creating pages, implementing UI interactions, or working on the agent builder interface for Chatverce.
user-invocable: false
---
```

Body content (from Spec Section 7):
- Component Checklist (quick Ive Test reference)
- Buttons (primary, secondary, ghost, destructive, loading, 44px touch target)
- Cards (white, sm shadow, 8px radius, content-first, hover elevation)
- Forms & Inputs (single-column, labels above, focus state, error placement)
- Navigation (sidebar, active state, breadcrumbs, mobile tab bar)
- Empty States (invitation, illustration + headline + CTA)
- Tables & Lists (minimal borders, row hover, pagination)
- Modals & Dialogs (white, lg shadow, 12px radius, one action, overlay)
- Agent Builder (nodes, connections, properties panel, canvas)
- Terminology Mapping table (15 rows)
- Motion & Animation rules (150ms/300ms, enter/exit, prefers-reduced-motion)
- Cross-reference: `For implementation code snippets, see [examples.md](examples.md)`

- [ ] **Step 2: Write examples.md**

Write `chatverce-design/skills/component-patterns/examples.md` with working code snippets using shadcn + Tailwind + Chatverce tokens (`cv-*` classes):

1. **Primary Button**
```tsx
<Button className="bg-cv-primary text-white hover:bg-cv-primary/90 h-11 px-6 rounded-cv-md">
  Create assistant
</Button>
```

2. **Secondary Button**
```tsx
<Button variant="outline" className="border-cv-accent text-cv-accent hover:bg-cv-accent/5 h-11 px-6 rounded-cv-md">
  View details
</Button>
```

3. **Ghost Button**
```tsx
<Button variant="ghost" className="text-cv-text-secondary hover:text-cv-text-primary hover:bg-cv-surface h-11 px-4">
  Cancel
</Button>
```

4. **Destructive Button**
```tsx
<Button className="bg-cv-error text-white hover:bg-cv-error/90 h-11 px-6 rounded-cv-md">
  Delete assistant
</Button>
```

5. **Card**
```tsx
<div className="bg-cv-surface-elevated shadow-cv-sm rounded-cv-md p-5 hover:shadow-cv-md transition-shadow duration-150 ease-out">
  {/* Content-first: no decorative header */}
  <h3 className="font-inter font-medium text-base text-cv-text-primary">Card Title</h3>
  <p className="text-sm text-cv-text-secondary mt-1">Description</p>
</div>
```

6. **Form Input with Focus State**
```tsx
<div className="space-y-1.5">
  <label className="text-sm font-medium text-cv-text-primary">Assistant name</label>
  <input
    className="w-full h-11 px-3 text-sm rounded-cv-md border border-cv-border bg-cv-surface-elevated
    focus:border-cv-accent focus:ring-2 focus:ring-cv-accent/10 outline-none transition-colors"
    placeholder="e.g. Customer Support"
  />
</div>
```

7. **Form Error State**
```tsx
<div className="space-y-1.5">
  <label className="text-sm font-medium text-cv-text-primary">Email</label>
  <input className="w-full h-11 px-3 text-sm rounded-cv-md border border-cv-error bg-cv-surface-elevated" />
  <p className="text-xs text-cv-error">Enter a valid email address</p>
</div>
```

8. **Navigation Sidebar Active Item**
```tsx
<nav className="space-y-0.5">
  <a className="flex items-center gap-3 px-3 py-2 rounded-cv-md text-cv-accent bg-cv-accent/10 text-sm font-medium">
    <BotIcon size={20} strokeWidth={1.5} />
    Assistants
  </a>
  <a className="flex items-center gap-3 px-3 py-2 rounded-cv-md text-cv-text-secondary hover:text-cv-text-primary hover:bg-cv-surface text-sm">
    <InboxIcon size={20} strokeWidth={1.5} />
    Conversations
  </a>
</nav>
```

9. **Empty State**
```tsx
<div className="flex flex-col items-center justify-center py-16 px-4 text-center">
  {/* Simple line-art illustration */}
  <BotIcon size={48} strokeWidth={1} className="text-cv-text-tertiary mb-4" />
  <h2 className="text-lg font-semibold text-cv-text-primary">Create your first assistant</h2>
  <p className="text-sm text-cv-text-secondary mt-1 max-w-sm">
    Your assistant will handle customer conversations across all your channels.
  </p>
  <Button className="bg-cv-primary text-white h-11 px-6 rounded-cv-md mt-6">
    Create assistant
  </Button>
</div>
```

10. **Modal**
```tsx
<Dialog>
  <DialogContent className="bg-cv-surface-elevated shadow-cv-lg rounded-cv-lg p-6 animate-in fade-in zoom-in-95 duration-300">
    <DialogHeader>
      <DialogTitle className="text-lg font-semibold text-cv-text-primary">Delete assistant</DialogTitle>
      <DialogDescription className="text-sm text-cv-text-secondary mt-1">
        This will permanently delete the assistant and its conversation history.
      </DialogDescription>
    </DialogHeader>
    <DialogFooter className="mt-6 flex gap-3 justify-end">
      <Button variant="ghost">Cancel</Button>
      <Button className="bg-cv-error text-white">Delete assistant</Button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

11. **Agent Builder Node Card**
```tsx
<div className="bg-cv-surface-elevated shadow-cv-sm rounded-cv-md p-4 border-l-3 border-l-cv-accent w-56">
  <div className="flex items-center gap-2">
    <ZapIcon size={16} className="text-cv-accent" />
    <span className="text-xs font-medium text-cv-text-secondary uppercase tracking-wide">When this happens</span>
  </div>
  <p className="text-sm font-medium text-cv-text-primary mt-2">New WhatsApp message</p>
</div>
```

12. **AI Working Indicator (Teal Pulse)**
```tsx
<div className="flex items-center gap-2">
  <span className="relative flex h-2.5 w-2.5">
    <span className="animate-ping absolute inline-flex h-full w-full rounded-full bg-cv-accent opacity-75" />
    <span className="relative inline-flex rounded-full h-2.5 w-2.5 bg-cv-accent" />
  </span>
  <span className="text-sm text-cv-text-secondary">Setting things up...</span>
</div>
```

- [ ] **Step 3: Verify examples use correct token classes**

Check every `cv-*` class in examples.md matches a token defined in brand-identity tokens.md. No rogue values.

- [ ] **Step 4: Commit**

```bash
git add chatverce-design/skills/component-patterns/
git commit -m "feat: add component-patterns skill — UI rules and code examples"
```

---

## Chunk 5: Voice & Tone Skill

### Task 5: Write the Voice & Tone skill

**Files:**
- Create: `chatverce-design/skills/voice-and-tone/SKILL.md`

**Source:** Spec Section 8

- [ ] **Step 1: Write SKILL.md**

Write `chatverce-design/skills/voice-and-tone/SKILL.md` with:

Frontmatter:
```yaml
---
name: voice-and-tone
description: Chatverce voice, tone, and terminology guidelines. Use when writing user-facing text, button labels, error messages, empty states, onboarding copy, tooltips, notifications, or any UI strings for Chatverce. Also use when reviewing existing copy.
user-invocable: false
---
```

Body content (from Spec Section 8):
- The Voice Principle (Apple Store analogy)
- Voice Attributes table (Clear, Confident, Warm, Brief — with Do/Don't)
- Writing Rules (7 rules)
- Tone by Context table (7 contexts)
- Button & Action Labels table (7 patterns)
- Terminology Dictionary table (15 mappings — same as in component-patterns, but this is the canonical location for copy guidance)

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/voice-and-tone/
git commit -m "feat: add voice-and-tone skill — copy guidelines and terminology"
```

---

## Chunk 6: Design Review Command & Agent

### Task 6: Write the Design Review command

**Files:**
- Create: `chatverce-design/commands/design-review.md`

**Source:** Spec Section 9

- [ ] **Step 1: Write design-review.md**

Write `chatverce-design/commands/design-review.md` with:

Frontmatter:
```yaml
---
description: Audit UI code against the Chatverce design system — brand identity, Ive-inspired principles, component patterns, voice & tone, and accessibility
disable-model-invocation: true
context: fork
agent: design-reviewer
argument-hint: [file, directory, or component name]
allowed-tools: Read, Grep, Glob, Bash(find *)
---
```

Body (the task prompt — this is the exact markdown to write below the frontmatter):

```markdown
# Chatverce Design Review

Review the following target for compliance with the Chatverce design system: $ARGUMENTS

## Step 1: Identify Files
Use Glob and Grep to find all relevant component files, style files, and UI strings in the target path. If $ARGUMENTS is a component name rather than a path, search for it.

## Step 2: Read the Design System
Read the following reference files from this plugin:
- [Design Soul](../skills/design-soul/SKILL.md) — philosophy and principles
- [Brand Identity](../skills/brand-identity/SKILL.md) — color, type, spacing tokens
- [Component Patterns](../skills/component-patterns/SKILL.md) — UI rules
- [Voice & Tone](../skills/voice-and-tone/SKILL.md) — copy guidelines

## Step 3: Evaluate Against Six Dimensions
For each file, check:
1. **Philosophy** — Does the UI pass the Ive Test? Can anything be removed?
2. **Brand identity** — Correct tokens? No rogue hex values? Typography on-scale?
3. **Component patterns** — Buttons follow hierarchy? Cards content-first? Forms single-column?
4. **Voice & tone** — Approved terminology? No jargon? Error messages are solutions?
5. **Accessibility** — Touch targets >= 44px? WCAG AA contrast? Labels on inputs?
6. **Motion** — Approved durations? No decorative animation?

## Step 4: Produce Report
Output a structured report with findings grouped by severity:
- **Must fix** — Violates core principles (jargon, wrong colors, missing a11y)
- **Should fix** — Deviates from patterns (non-standard spacing, inconsistent radius)
- **Consider** — Opportunities to better embody the philosophy

Each finding must include:
- File path and line number
- What's wrong
- Which principle it violates
- The specific fix (code suggestion when possible)
```

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/commands/
git commit -m "feat: add design-review command — /chatverce-design:design-review"
```

### Task 7: Write the Design Reviewer agent

**Files:**
- Create: `chatverce-design/agents/design-reviewer.md`

**Source:** Spec Section 10

- [ ] **Step 1: Write design-reviewer.md**

Write `chatverce-design/agents/design-reviewer.md` with:

Frontmatter:
```yaml
---
name: design-reviewer
description: Reviews UI code against Chatverce design system — brand identity, Ive-inspired design principles, component patterns, voice & tone, and accessibility. Use when reviewing frontend code, PRs with UI changes, or auditing existing screens.
tools: Read, Grep, Glob, Bash(find *)
model: sonnet
---
```

Body (system prompt — the exact markdown to write below the frontmatter):

```markdown
You are the Chatverce Design Reviewer. You audit UI code against the Chatverce design system.

## Your Knowledge

You have deep knowledge of the Chatverce design system, which is based on Jony Ive's design philosophy. The core principles are:

1. **Inevitable Simplicity** — every screen should feel like the only way it could have been
2. **Care Is Visible** — users sense thoughtful vs thoughtless design
3. **Deference to the User's Goal** — UI recedes, content is the hero
4. **Complexity Absorbed, Not Transferred** — the interface is never complex
5. **Quiet Confidence** — premium doesn't shout

## Brand Tokens (Quick Reference)

- Primary: #1B2A4A (Midnight Navy) | Accent: #2EC4B6 (Teal)
- Background: #FAFBFC | Surface: #F1F3F5 | Elevated: #FFFFFF
- Text: #1A1D23 / #64748B / #94A3B8 | Border: #E2E8F0
- Success: #10B981 | Warning: #F59E0B | Error: #EF4444
- Font: Inter (400/500/600) | Code: JetBrains Mono
- Radius: 6/8/12/9999px | Shadows: minimal, elevation only
- Touch target minimum: 44px
- No WhatsApp green as primary — green is channel indicator only

## Key Rules

- Single-column forms, labels above inputs, never floating
- One primary button per screen, no gradients on buttons
- Cards: white, sm shadow, 8px radius, content-first
- Motion: 150ms micro, 300ms panels, ease-out, no decorative animation
- Terminology: "Assistant" not "Agent", "Go live" not "Deploy", "Step" not "Node"
- Error messages must be solutions, never blame the user
- No technical jargon in any user-facing string

For the full design system reference, read the skill files in this plugin's skills/ directory.
```

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/agents/
git commit -m "feat: add design-reviewer agent — specialized review subagent"
```

---

## Chunk 7: Validation & Ship

### Task 8: Validate the complete plugin

- [ ] **Step 1: Verify directory structure**

```bash
cd /Users/sandeep/Desktop/dev/org-siharilabs/plugins
find chatverce-design -type f | sort
```

Expected output (11 files):
```
chatverce-design/.claude-plugin/plugin.json
chatverce-design/README.md
chatverce-design/agents/design-reviewer.md
chatverce-design/commands/design-review.md
chatverce-design/skills/brand-identity/SKILL.md
chatverce-design/skills/brand-identity/tokens.md
chatverce-design/skills/component-patterns/SKILL.md
chatverce-design/skills/component-patterns/examples.md
chatverce-design/skills/design-soul/SKILL.md
chatverce-design/skills/design-soul/ive-philosophy.md
chatverce-design/skills/voice-and-tone/SKILL.md
```

- [ ] **Step 2: Validate plugin structure**

```bash
claude plugin validate ./chatverce-design
```

Expected: No errors.

- [ ] **Step 3: Cross-reference token consistency**

Grep all hex values across all files and verify no rogue colors:

```bash
grep -roh '#[0-9A-Fa-f]\{6\}' chatverce-design/ | sort -u
```

Every hex value must appear in the brand-identity color table. No extras.

- [ ] **Step 4: Cross-reference terminology consistency**

Verify the terminology mapping in component-patterns matches voice-and-tone:
- "Assistant" not "Agent" in all user-facing contexts
- "Go live" not "Deploy"
- "Step" not "Node"

- [ ] **Step 5: Test plugin loading**

```bash
claude --plugin-dir ./chatverce-design
```

In the Claude session:
- Type: "What design skills are available?"
- Verify all 4 skills are mentioned
- Type: `/chatverce-design:design-review` and verify it appears
- Type: `/agents` and verify design-reviewer appears

- [ ] **Step 6: Create GitHub repo and push**

```bash
cd /Users/sandeep/Desktop/dev/org-siharilabs/plugins
gh repo create org-siharilabs/plugins --private --source=. --push
```

- [ ] **Step 7: Final commit with version tag**

```bash
git tag v1.0.0
git push origin main --tags
```
