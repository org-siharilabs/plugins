# CDG v2.0 — Plan A: Core Infrastructure, Philosophy & Foundations

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the complete foundation layer of CDG v2.0 — directory structure, philosophy, token system, and all 10 foundations — so that Plans B/C/D can run in parallel.

**Architecture:** Create the v2 directory tree under `chatverce-design/skills/`, write philosophy + 10 foundation skills with supporting files, generate token outputs for Tailwind (shared web+mobile), CSS (web), and React Native (mobile). The v1 `skills/` directory is left intact until Plan D cleanup.

**Tech Stack:** Markdown (SKILL.md files), JSON (plugin.json), JavaScript (Tailwind config), CSS (custom properties), TypeScript (RN constants)

**Spec:** `docs/specs/2026-03-19-chatverce-design-plugin-v2-design.md`

**Working directory:** `/Users/sandeep/Desktop/dev/org-siharilabs/plugins`

---

## File Map

### New Files (this plan creates)

```
chatverce-design/
  skills/
    philosophy/
      design-ethos/
        SKILL.md                          # Task 2
        ive-philosophy.md                 # Task 3 (copy from v1)

    foundations/
      theme-system/
        SKILL.md                          # Task 4
        primitives.md                     # Task 5
        semantic-light.md                 # Task 6
        semantic-dark.md                  # Task 7
        tokens-tailwind.md               # Task 8
        tokens-css.md                     # Task 9
        tokens-rn.md                      # Task 10
      color/
        SKILL.md                          # Task 11
        contrast-matrix.md               # Task 11 (same task)
      typography/
        SKILL.md                          # Task 12
      layout/
        SKILL.md                          # Task 13
      motion/
        SKILL.md                          # Task 14
      materials-and-depth/
        SKILL.md                          # Task 15
      iconography/
        SKILL.md                          # Task 16
      accessibility/
        SKILL.md                          # Task 17
      privacy/
        SKILL.md                          # Task 18
      writing/
        SKILL.md                          # Task 19
```

### Modified Files

```
chatverce-design/
  .claude-plugin/
    plugin.json                           # Task 1 (version bump, updated metadata)
```

### Unchanged (v1 files kept until Plan D)

```
chatverce-design/
  skills/
    design-soul/...                       # Kept until Plan D cleanup
    brand-identity/...                    # Kept until Plan D cleanup
    component-patterns/...                # Kept until Plan D cleanup
    voice-and-tone/...                    # Kept until Plan D cleanup
```

---

## Tasks

### Task 1: Create v2 Directory Structure + Update plugin.json

**Files:**
- Modify: `chatverce-design/.claude-plugin/plugin.json`
- Create: All v2 directories (empty, populated by subsequent tasks)

- [ ] **Step 1: Create all v2 directories**

```bash
cd /Users/sandeep/Desktop/dev/org-siharilabs/plugins/chatverce-design/skills

mkdir -p philosophy/design-ethos
mkdir -p foundations/theme-system
mkdir -p foundations/color
mkdir -p foundations/typography
mkdir -p foundations/layout
mkdir -p foundations/motion
mkdir -p foundations/materials-and-depth
mkdir -p foundations/iconography
mkdir -p foundations/accessibility
mkdir -p foundations/privacy
mkdir -p foundations/writing
mkdir -p patterns
mkdir -p components
mkdir -p enforcement
```

- [ ] **Step 2: Update plugin.json to v2.0.0**

Write to `chatverce-design/.claude-plugin/plugin.json`:

```json
{
  "name": "chatverce-design",
  "description": "Chatverce Design Guidelines (CDG) — agent-agnostic design system for building Chatverce across web and mobile. Covers philosophy, foundations, patterns, components, and enforcement.",
  "version": "2.0.0",
  "author": {
    "name": "Sihari Labs",
    "email": "admin@siharilabs.com"
  },
  "repository": "https://github.com/org-siharilabs/plugins",
  "license": "Proprietary",
  "keywords": [
    "design-system",
    "design-guidelines",
    "chatverce",
    "ui",
    "ux",
    "dark-mode",
    "accessibility",
    "mobile",
    "web"
  ]
}
```

- [ ] **Step 3: Verify directory structure**

```bash
find chatverce-design/skills -type d | sort
```

Expected: all directories from Step 1 visible plus existing v1 directories.

- [ ] **Step 4: Commit**

```bash
git add chatverce-design/.claude-plugin/plugin.json chatverce-design/skills/
git commit -m "chore: scaffold CDG v2.0 directory structure and bump to 2.0.0"
```

---

### Task 2: Write Philosophy — Design Ethos

**Files:**
- Create: `chatverce-design/skills/philosophy/design-ethos/SKILL.md`

**Reference:** Spec Part 1: Philosophy (lines 72–207)

- [ ] **Step 1: Write SKILL.md**

Write the Design Ethos skill. This is the most important file in the entire plugin — it sets the tone for everything. Content must include:

```markdown
---
name: design-ethos
description: Chatverce design philosophy rooted in Jony Ive's principles — caring, simplicity, breathing room, focus, emotional intention, and thoroughness
user_invocable: false
---
```

**Required sections (from spec):**

1. **Caring Is the X-Factor** — Include Ive quotes: "characterized by carelessness" and "discerning far more than we are capable of articulating." The 285-hour flower shot anecdote. "Would someone sense the care?" test.

2. **Inevitable Simplicity** — Ive quote: "True simplicity is derived from so much more than just the absence of clutter." One primary action per screen. Smart defaults. Complexity absorbed.

3. **Breathing Room** — "When there's no open space, the customer doesn't engage." White space as invitation. Restraint as design strength. What you leave out defines the product.

4. **Focus as Discipline** — Ive quote: "Saying no to something that you, with every bone in your body, think is a phenomenal idea." Jobs's 2×2 grid. Every screen has one job.

5. **Emotional Intention** — Design for the emotional brain. Include the full context-to-feeling table from spec (Dashboard → Calm confidence, Onboarding → Guided ease, Going live → Quiet pride, Error → Reassurance, Empty state → Anticipation, Conversation → Efficient flow, Settings → Trust).

6. **Thoroughness in Every Inch** — Apple Watch "complete accuracy" reference. A button is done when ALL states are considered (hover, loading, disabled with reason, destructive pause, touch target, focus ring, motion respect, terminology).

7. **The Ive Test** — 5 questions: Can anything be removed? Does UI defer? Would a non-technical owner understand in 3 seconds? Feels inevitable? Would someone sense the care?

8. **Anti-Patterns (Hard Rules)** — All 10 anti-patterns from spec.

9. **The Autopilot Promise** — "Put your business on autopilot." Setup flows feel like one-time investment. Dashboard emphasizes AI handling autonomously. Product feels like it's working when user isn't looking.

**Tone:** This file should read like a manifesto, not a spec. Passionate but precise. It's teaching an AI agent to CARE about design, not just follow rules.

**Source material:** Read `chatverce-design/skills/design-soul/SKILL.md` (v1) for existing content to refine and expand. Read `chatverce-design/skills/design-soul/ive-philosophy.md` for quotes and philosophy to weave in.

- [ ] **Step 2: Verify file renders correctly**

```bash
head -5 chatverce-design/skills/philosophy/design-ethos/SKILL.md
wc -l chatverce-design/skills/philosophy/design-ethos/SKILL.md
```

Expected: YAML frontmatter visible, file should be 150-250 lines.

- [ ] **Step 3: Commit**

```bash
git add chatverce-design/skills/philosophy/design-ethos/SKILL.md
git commit -m "feat(cdg): add design ethos — philosophy layer with Ive principles"
```

---

### Task 3: Move Ive Philosophy Reference

**Files:**
- Create: `chatverce-design/skills/philosophy/design-ethos/ive-philosophy.md` (copy from v1)

- [ ] **Step 1: Copy file to v2 location**

```bash
cp chatverce-design/skills/design-soul/ive-philosophy.md chatverce-design/skills/philosophy/design-ethos/ive-philosophy.md
```

Note: Do NOT delete the v1 original — Plan D handles cleanup.

- [ ] **Step 2: Verify**

```bash
diff chatverce-design/skills/design-soul/ive-philosophy.md chatverce-design/skills/philosophy/design-ethos/ive-philosophy.md
```

Expected: no differences.

- [ ] **Step 3: Commit**

```bash
git add chatverce-design/skills/philosophy/design-ethos/ive-philosophy.md
git commit -m "feat(cdg): copy ive-philosophy reference to v2 philosophy layer"
```

---

### Task 4: Write Theme System Foundation

**Files:**
- Create: `chatverce-design/skills/foundations/theme-system/SKILL.md`

**Reference:** Spec Part 2: Foundations > Theme System (lines 210–322)

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: theme-system
description: Three-layer token architecture (primitive → semantic → component) with dark/light theme switching for web and mobile
user_invocable: false
---
```

**Required content:**

1. **Overview of the 3-layer architecture** — Explain the concept: primitives (raw values, never used in code), semantic (intent-based, theme-switchable, what code references), component (specific to each component, defined in component skills).

2. **The Token Rule** — Application code references semantic or component tokens only. Never primitives. Never raw hex/rgb. Document exceptions: primitive definition files, `/* cv-override */`, `/* cv-exception: reason */`.

3. **Theme switching mechanism:**
   - Web: CSS custom properties with `[data-theme="dark"]` or `prefers-color-scheme` media query. Tailwind `dark:` class strategy.
   - Mobile (NativeWind): `useColorScheme()` hook + NativeWind dark mode support. Respect system setting.

4. **How to add a new token** — Step-by-step: add primitive if needed → add semantic mapping for both themes → reference semantic token in code.

5. **Cross-references** — Point to `primitives.md`, `semantic-light.md`, `semantic-dark.md`, `tokens-tailwind.md`, `tokens-css.md`, `tokens-rn.md` as companion files.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/theme-system/SKILL.md
git commit -m "feat(cdg): add theme-system foundation — 3-layer token architecture"
```

---

### Task 5: Write Primitive Tokens

**Files:**
- Create: `chatverce-design/skills/foundations/theme-system/primitives.md`

**Reference:** Spec Layer 1 — Primitives (lines 214–247)

- [ ] **Step 1: Write primitives.md**

Generate full color scales (50–950) for each hue. Use established color science — each step should have consistent perceptual spacing.

**Required scales:**
- `cv-navy-50` through `cv-navy-950` (11 stops)
- `cv-teal-50` through `cv-teal-950` (11 stops)
- `cv-gray-50` through `cv-gray-950` (11 stops)
- `cv-red-50` through `cv-red-950` (11 stops)
- `cv-amber-50` through `cv-amber-950` (11 stops)
- `cv-green-50` through `cv-green-950` (11 stops)

**Anchors (from v1 brand-identity):**
- Navy 700 = `#1B2A4A` (v1 "Midnight Navy")
- Teal 500 = `#2EC4B6` (v1 "Chatverce Teal")
- Gray 50 = `#FAFBFC` (v1 "Snow")
- Gray 100 = `#F1F3F5` (v1 "Cloud")
- Gray 900 = `#1A1D23` (v1 "Ink")
- Gray 500 = `#64748B` (v1 "Slate")
- Gray 400 = `#94A3B8` (v1 "Mist")
- Gray 200 = `#E2E8F0` (v1 "Frost")
- Green 500 = `#10B981` (v1 "Meadow")
- Amber 500 = `#F59E0B` (v1 "Amber")
- Red 500 = `#EF4444` (v1 "Coral")

Generate intermediate values by interpolating between anchors. The 950 values should be very dark (near black), 50 values very light (near white).

**Also include channel colors:**
- `cv-channel-whatsapp: #25D366`
- `cv-channel-telegram: #26A5E4`
- `cv-channel-webchat: cv-gray-500`
- `cv-channel-telephone: cv-navy-700`
- `cv-channel-email: cv-navy-700`

**Format:** Markdown table with token name, hex value, and usage note.

- [ ] **Step 2: Verify all anchors from v1 are present**

```bash
grep -c "cv-" chatverce-design/skills/foundations/theme-system/primitives.md
```

Expected: 66+ color definitions (6 hues × 11 stops = 66, plus 5 channel colors = 71).

- [ ] **Step 3: Commit**

```bash
git add chatverce-design/skills/foundations/theme-system/primitives.md
git commit -m "feat(cdg): add primitive color tokens — full scales for 6 hues + channel colors"
```

---

### Task 6: Write Semantic Light Theme Mappings

**Files:**
- Create: `chatverce-design/skills/foundations/theme-system/semantic-light.md`

**Reference:** Spec Layer 2 — Semantic, Light column (lines 249–313)

- [ ] **Step 1: Write semantic-light.md**

Map every semantic token to its light-theme primitive value. Organize by category:

1. **Backgrounds** — `cv-bg-primary`, `cv-bg-surface`, `cv-bg-surface-elevated`, `cv-bg-surface-sunken`, `cv-bg-accent`, `cv-bg-accent-subtle`, `cv-bg-accent-hover`, `cv-bg-destructive`, `cv-bg-destructive-subtle`
2. **Text** — `cv-text-primary`, `cv-text-secondary`, `cv-text-tertiary`, `cv-text-on-accent`, `cv-text-on-destructive`, `cv-text-link`, `cv-text-success`, `cv-text-warning`, `cv-text-error`
3. **Borders** — `cv-border-default`, `cv-border-strong`, `cv-border-accent`, `cv-border-error`
4. **Status** — `cv-status-success`, `cv-status-warning`, `cv-status-error`, `cv-status-info`
5. **Overlay/shadow** — `cv-shadow-color`, `cv-overlay`

Use exact values from spec tables.

**Format:** Markdown table: Token | Primitive Reference | Resolved Hex

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/theme-system/semantic-light.md
git commit -m "feat(cdg): add light theme semantic token mappings"
```

---

### Task 7: Write Semantic Dark Theme Mappings

**Files:**
- Create: `chatverce-design/skills/foundations/theme-system/semantic-dark.md`

**Reference:** Spec Layer 2 — Semantic, Dark column (lines 249–313)

- [ ] **Step 1: Write semantic-dark.md**

Same structure as semantic-light.md but with dark theme values. Key inversions:
- Backgrounds go dark (navy-based): `cv-bg-primary` → `cv-navy-950`
- Text goes light: `cv-text-primary` → `cv-gray-50`
- Accent shifts lighter for visibility: `cv-bg-accent` → `cv-teal-400`
- Borders use navy mid-tones: `cv-border-default` → `cv-navy-700`
- Shadow color stays `0,0,0` but overlay gets more opaque

Use exact values from spec tables.

**Critical:** Every token in semantic-light.md MUST have a corresponding entry in semantic-dark.md. Missing tokens = theme-guard violations.

- [ ] **Step 2: Verify token count matches light theme**

```bash
grep -c "|" chatverce-design/skills/foundations/theme-system/semantic-light.md
grep -c "|" chatverce-design/skills/foundations/theme-system/semantic-dark.md
```

Expected: same count (within 2 lines for headers).

- [ ] **Step 3: Commit**

```bash
git add chatverce-design/skills/foundations/theme-system/semantic-dark.md
git commit -m "feat(cdg): add dark theme semantic token mappings"
```

---

### Task 8: Write Tailwind Token Config

**Files:**
- Create: `chatverce-design/skills/foundations/theme-system/tokens-tailwind.md`

**Reference:** Spec tech stack: Tailwind shared via NativeWind

- [ ] **Step 1: Write tokens-tailwind.md**

This is the **primary token output** — shared by web (Tailwind CSS) and mobile (NativeWind). Contains a complete `tailwind.config.js` extension:

```javascript
// chatverce-design tokens — shared across web (Next.js) and mobile (Expo + NativeWind)
module.exports = {
  theme: {
    extend: {
      colors: {
        cv: {
          // Primitives (for token definitions only — never use directly in components)
          navy: { 50: '#...', 100: '#...', /* ... */ 950: '#...' },
          teal: { /* full scale */ },
          gray: { /* full scale */ },
          red: { /* full scale */ },
          amber: { /* full scale */ },
          green: { /* full scale */ },
          // Channel colors
          channel: {
            whatsapp: '#25D366',
            telegram: '#26A5E4',
            webchat: '#64748B',  // cv-gray-500
            telephone: '#1B2A4A', // cv-navy-700
            email: '#1B2A4A',
          },
        },
        // Semantic tokens (USE THESE in components)
        'cv-bg-primary': 'var(--cv-bg-primary)',
        'cv-bg-surface': 'var(--cv-bg-surface)',
        // ... all semantic tokens as CSS variable references
      },
      fontFamily: {
        'cv-sans': ['Inter', 'ui-sans-serif', 'system-ui', 'sans-serif'],
        'cv-mono': ['JetBrains Mono', 'ui-monospace', 'monospace'],
      },
      borderRadius: {
        'cv-sm': '6px',
        'cv-md': '8px',
        'cv-lg': '12px',
        'cv-full': '9999px',
      },
      boxShadow: {
        'cv-sm': '0 1px 2px rgba(var(--cv-shadow-color), 0.05)',
        'cv-md': '0 4px 12px rgba(var(--cv-shadow-color), 0.08)',
        'cv-lg': '0 12px 32px rgba(var(--cv-shadow-color), 0.12)',
      },
      zIndex: {
        'cv-dropdown': '100',
        'cv-sticky': '200',
        'cv-overlay': '300',
        'cv-modal': '400',
        'cv-popover': '500',
        'cv-toast': '600',
        'cv-tooltip': '700',
      },
      transitionDuration: {
        'cv-fast': '150ms',
        'cv-normal': '300ms',
        'cv-slow': '500ms',
      },
      transitionTimingFunction: {
        'cv-default': 'cubic-bezier(0.4, 0, 0.2, 1)',
        'cv-out': 'cubic-bezier(0, 0, 0.2, 1)',
        'cv-in': 'cubic-bezier(0.4, 0, 1, 1)',
      },
      spacing: {
        'cv-1': '4px',
        'cv-2': '8px',
        'cv-3': '12px',
        'cv-4': '16px',
        'cv-5': '20px',
        'cv-6': '24px',
        'cv-8': '32px',
        'cv-10': '40px',
        'cv-12': '48px',
        'cv-16': '64px',
        'cv-20': '80px',
        'cv-24': '96px',
      },
    },
  },
}
```

Include inline comments explaining NativeWind compatibility notes.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/theme-system/tokens-tailwind.md
git commit -m "feat(cdg): add Tailwind token config — shared web + NativeWind mobile"
```

---

### Task 9: Write CSS Custom Properties

**Files:**
- Create: `chatverce-design/skills/foundations/theme-system/tokens-css.md`

- [ ] **Step 1: Write tokens-css.md**

CSS custom properties for web only. Two blocks:

```css
/* Light theme (default) */
:root {
  --cv-bg-primary: #FAFBFC;        /* cv-gray-50 */
  --cv-bg-surface: #FFFFFF;         /* white */
  --cv-bg-surface-elevated: #FFFFFF;
  --cv-bg-surface-sunken: #F1F3F5; /* cv-gray-100 */
  --cv-bg-accent: #2EC4B6;         /* cv-teal-500 */
  /* ... all semantic tokens with resolved hex values ... */

  --cv-shadow-color: 0, 0, 0;
  --cv-overlay: rgba(26, 29, 35, 0.5);

  /* Non-color tokens */
  --cv-font-sans: 'Inter', ui-sans-serif, system-ui, sans-serif;
  --cv-font-mono: 'JetBrains Mono', ui-monospace, monospace;
  /* ... radius, spacing, shadows, z-index, motion, easing, breakpoints ... */
}

/* Dark theme */
[data-theme="dark"],
.dark {
  --cv-bg-primary: #060A12;        /* cv-navy-950 */
  --cv-bg-surface: #0D1525;        /* cv-navy-900 */
  /* ... all semantic tokens with dark hex values ... */
}

/* System preference (optional — use if not manually toggling) */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme]) {
    --cv-bg-primary: #060A12;
    /* ... dark values ... */
  }
}

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  :root {
    --cv-duration-fast: 0ms;
    --cv-duration-normal: 0ms;
    --cv-duration-slow: 0ms;
  }
}
```

Every token must have a comment showing which primitive it resolves to.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/theme-system/tokens-css.md
git commit -m "feat(cdg): add CSS custom properties — light/dark theme tokens"
```

---

### Task 10: Write React Native Token Constants

**Files:**
- Create: `chatverce-design/skills/foundations/theme-system/tokens-rn.md`

- [ ] **Step 1: Write tokens-rn.md**

TypeScript constants for use when NativeWind classes can't be used (Reanimated interpolation, dynamic styles, StyleSheet.create fallback):

```typescript
// Chatverce Design Tokens — React Native
// Use NativeWind cv-* classes when possible. These constants are for
// StyleSheet.create, Reanimated interpolation, and dynamic styles.

export const primitives = {
  navy: { 50: '#...', /* ... */ 950: '#...' },
  teal: { /* ... */ },
  // ... all primitive scales
} as const;

export const semantic = {
  light: {
    bg: {
      primary: primitives.gray[50],
      surface: '#FFFFFF',
      surfaceElevated: '#FFFFFF',
      // ...
    },
    text: { /* ... */ },
    border: { /* ... */ },
    status: { /* ... */ },
  },
  dark: {
    bg: {
      primary: primitives.navy[950],
      surface: primitives.navy[900],
      // ...
    },
    text: { /* ... */ },
    border: { /* ... */ },
    status: { /* ... */ },
  },
} as const;

export const spacing = {
  1: 4, 2: 8, 3: 12, 4: 16, 5: 20, 6: 24,
  8: 32, 10: 40, 12: 48, 16: 64, 20: 80, 24: 96,
} as const;

export const radii = {
  sm: 6, md: 8, lg: 12, full: 9999,
} as const;

export const duration = {
  fast: 150, normal: 300, slow: 500,
} as const;

export const channelConfig = {
  whatsapp: { color: '#25D366', label: 'WhatsApp' },
  telegram: { color: '#26A5E4', label: 'Telegram' },
  webchat: { color: primitives.gray[500], label: 'Webchat' },
  telephone: { color: primitives.navy[700], label: 'Telephone' },
  email: { color: primitives.navy[700], label: 'Email' },
} as const;

// Helper: get semantic tokens for current theme
export function getThemeTokens(isDark: boolean) {
  return isDark ? semantic.dark : semantic.light;
}
```

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/theme-system/tokens-rn.md
git commit -m "feat(cdg): add React Native token constants — fallback for dynamic styles"
```

---

### Task 11: Write Color Foundation + Contrast Matrix

**Files:**
- Create: `chatverce-design/skills/foundations/color/SKILL.md`
- Create: `chatverce-design/skills/foundations/color/contrast-matrix.md`

**Reference:** Spec Part 2: Color (lines 325–349)

- [ ] **Step 1: Write color SKILL.md**

```markdown
---
name: color
description: Color principles, semantic usage, contrast requirements, and accessibility for Chatverce across web and mobile
user_invocable: false
---
```

**Required sections:**

1. **Color Philosophy** — Color is functional, not decorative. Every color has a purpose. Chatverce identity = navy (intelligence) + teal (energy). Never WhatsApp green as primary.

2. **Semantic Color Usage** — Table mapping each semantic token to its purpose with do/don't examples. E.g., `cv-bg-accent` = CTAs, active states, links. Do: use for primary buttons. Don't: use for backgrounds of large areas.

3. **Contrast Requirements** — 4.5:1 body text, 3:1 large text, 7:1 preferred. Both themes must pass. Reference `contrast-matrix.md`.

4. **Color Accessibility** — Never use color alone to communicate meaning. Always pair with shape/icon/text. Test with color blindness simulation.

5. **Platform Considerations:**
   - Web: Use CSS custom properties via `var()`. Dark mode via `[data-theme="dark"]` or `prefers-color-scheme`.
   - Mobile: NativeWind dark mode classes. System color scheme respected by default. Use `useColorScheme()` for conditional logic.

6. **Adding New Colors** — Process: add primitive → verify contrast in both themes → add semantic mapping → document purpose.

- [ ] **Step 2: Write contrast-matrix.md**

Table of every foreground/background semantic token pair with computed contrast ratios for both themes:

```markdown
| Text Token | Background Token | Light Ratio | Light Pass | Dark Ratio | Dark Pass |
|-----------|-----------------|-------------|-----------|------------|----------|
| cv-text-primary | cv-bg-primary | ~15.4:1 | AA ✓ | ~14.8:1 | AA ✓ |
| cv-text-primary | cv-bg-surface | ~15.4:1 | AA ✓ | ~12.1:1 | AA ✓ |
| cv-text-secondary | cv-bg-primary | ~4.8:1 | AA ✓ | ~5.2:1 | AA ✓ |
| cv-text-secondary | cv-bg-surface | ~4.8:1 | AA ✓ | ~4.6:1 | AA ✓ |
| cv-text-tertiary | cv-bg-primary | ~3.2:1 | AA large ✓ | ~3.5:1 | AA large ✓ |
| cv-text-on-accent | cv-bg-accent | ~4.6:1 | AA ✓ | ~4.8:1 | AA ✓ |
| cv-text-error | cv-bg-surface | ~4.5:1 | AA ✓ | ~4.7:1 | AA ✓ |
```

Compute actual ratios using the resolved hex values from primitives.md. Mark any pair that fails AA as needing adjustment. If any fail, adjust the primitive value and document the change.

**Important:** The ratios shown above are estimates from the spec. The actual implementation must compute real ratios from the final hex values in primitives.md and adjust if needed.

- [ ] **Step 3: Commit**

```bash
git add chatverce-design/skills/foundations/color/
git commit -m "feat(cdg): add color foundation — principles, usage, contrast matrix"
```

---

### Task 12: Write Typography Foundation

**Files:**
- Create: `chatverce-design/skills/foundations/typography/SKILL.md`

**Reference:** Spec Part 2: Typography (lines 351–378)

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: typography
description: Type scale, hierarchy, dynamic sizing, and platform-specific typography rules for web and mobile
user_invocable: false
---
```

**Required sections:**

1. **Type Scale** — Full table from spec (Display through Code/Agent) with font, weight, size range, line height.

2. **Why Inter** — Geometric, clean, screen-optimized. Google Fonts + NativeWind compatible.

3. **Hierarchy Rules** — Use type scale to establish hierarchy. Never skip heading levels. Body text minimum: 14px web, 16px mobile (prevents iOS auto-zoom).

4. **Dynamic Sizing:**
   - Web: Fluid via `clamp()` (e.g., `clamp(14px, 1vw + 12px, 16px)` for body). Support 200% text enlargement (WCAG).
   - Mobile: Support Dynamic Type (iOS) and font scaling (Android). Use `allowFontScaling` prop. Test at maximum accessibility sizes.

5. **Font Loading:**
   - Web: `@font-face` with `font-display: swap`. System font fallback stack.
   - Mobile: `expo-font` for Inter and JetBrains Mono. Load in app root before rendering.

6. **Platform Considerations:**
   - Web: rem units, responsive clamp(), `font-cv-sans` / `font-cv-mono` Tailwind classes
   - iOS: Inter loaded via expo-font. System font used during load. Minimum 16px for inputs (prevents auto-zoom).
   - Android: Inter loaded via expo-font. Respect system font scale setting.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/typography/SKILL.md
git commit -m "feat(cdg): add typography foundation — scale, hierarchy, dynamic sizing"
```

---

### Task 13: Write Layout Foundation

**Files:**
- Create: `chatverce-design/skills/foundations/layout/SKILL.md`

**Reference:** Spec Part 2: Layout (lines 380–427)

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: layout
description: Breakpoints, z-index scale, grid system, spacing, safe areas, and breathing room principles for web and mobile
user_invocable: false
---
```

**Required sections:**

1. **Breakpoints** — Full table from spec (sm through 2xl) with usage descriptions.

2. **Z-index Scale** — Full table from spec (cv-z-dropdown through cv-z-tooltip).

3. **Spacing Scale** — 4px base unit, full scale with Tailwind class mappings.

4. **Breathing Room** — From philosophy: minimum padding between sections (`cv-space-6`), minimum card gap (`cv-space-4`), content never touches edges.

5. **Grid System** — No rigid column grid prescribed. Use Flexbox/CSS Grid on web, Flexbox on mobile. Content-first layout — let content determine the grid, not vice versa.

6. **Safe Areas (mobile):**
   - iOS: Notch, Dynamic Island, home indicator. Use `SafeAreaView` or `useSafeAreaInsets()`.
   - Android: Navigation bar, status bar. Use `react-native-safe-area-context`.
   - Always test on devices with notches AND without.

7. **Platform Considerations:**
   - Web: CSS breakpoints, responsive Tailwind (`sm:`, `md:`, `lg:`, `xl:`, `2xl:`). Full-bleed layouts with max-width containers.
   - Mobile: No CSS breakpoints. Use `useWindowDimensions()` or NativeWind responsive utilities. Orientation-aware layouts.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/layout/SKILL.md
git commit -m "feat(cdg): add layout foundation — breakpoints, z-index, spacing, safe areas"
```

---

### Task 14: Write Motion Foundation

**Files:**
- Create: `chatverce-design/skills/foundations/motion/SKILL.md`

**Reference:** Spec Part 2: Motion (lines 429–466)

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: motion
description: Animation duration tokens, easing curves, reduced-motion support, haptic feedback patterns, and the rule that motion serves comprehension
user_invocable: false
---
```

**Required sections:**

1. **Motion Philosophy** — "Motion serves comprehension, never decoration" (Ive). Purposeful, not gratuitous. Must be optional — never the only way to communicate info.

2. **Duration Tokens** — Table from spec (fast/normal/slow).

3. **Easing Tokens** — Table from spec (default/out/in).

4. **Animation Patterns:**
   - Enter: fade + slight upward translate (8px), `cv-duration-normal`, `cv-ease-out`
   - Exit: fade only, `cv-duration-fast`, `cv-ease-in`
   - Micro-interaction: `cv-duration-fast`, `cv-ease-default`
   - AI working: gentle teal pulse (only allowed "decorative" motion)

5. **Never List** — Bouncing, spinning, confetti, particles, parallax, auto-playing carousel.

6. **Reduced Motion:**
   - Web: `motion-safe:` prefix on Tailwind. `@media (prefers-reduced-motion: reduce)` zeroes all durations.
   - Mobile: `useReducedMotion()` from `react-native-reanimated` or `AccessibilityInfo.isReduceMotionEnabled()`.

7. **Haptics (mobile only)** — Full event-to-haptic-type table from spec. `expo-haptics` library. Never haptics without visual counterpart. Respect system setting.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/motion/SKILL.md
git commit -m "feat(cdg): add motion foundation — duration, easing, haptics, reduced-motion"
```

---

### Task 15: Write Materials & Depth Foundation

**Files:**
- Create: `chatverce-design/skills/foundations/materials-and-depth/SKILL.md`

**Reference:** Spec Part 2: Materials & Depth (lines 468–497)

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: materials-and-depth
description: Visual layering system — elevation levels, shadows, translucency, and platform-specific depth rendering for web and mobile
user_invocable: false
---
```

**Required sections:**

1. **Elevation Levels** — Full table from spec (Level 0–4) with web and mobile rendering per level.

2. **Shadow System** — `cv-shadow-sm/md/lg` with exact values. Dark mode shadows use same `cv-shadow-color` but may need higher opacity for visibility.

3. **Translucency** — Used sparingly for nav/tab bars. Never for content surfaces. `expo-blur` on mobile.

4. **Platform Considerations:**
   - Web: Box shadows create depth. `backdrop-filter: blur()` for translucency.
   - iOS: Minimal shadows preferred. Depth via layering and blur. `BlurView` from `expo-blur`.
   - Android: Material-style `elevation` prop. Shadow rendering differs — test on real devices.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/materials-and-depth/SKILL.md
git commit -m "feat(cdg): add materials & depth foundation — elevation, shadows, translucency"
```

---

### Task 16: Write Iconography Foundation

**Files:**
- Create: `chatverce-design/skills/foundations/iconography/SKILL.md`

**Reference:** Spec Part 2: Iconography (lines 499–512)

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: iconography
description: Lucide icon standards — sizing, stroke weight, touch targets, fill rules, and channel icon specifications
user_invocable: false
---
```

**Required sections:**

1. **Icon Set** — Lucide icons. Thin, consistent stroke weight.

2. **Sizing:**
   - Default: 20px, 1.5px stroke
   - Small (badges, inline): 16px
   - Large (empty states, hero): 48px

3. **Touch Targets** — Icon can be 20px but tappable area must be 44px minimum (add padding).

4. **Fill Rules** — Never fill unless indicating active/selected state. Outline = default, filled = active.

5. **Icon-Text Pairing** — 8px gap between icon and text. Icon vertically centered with text baseline.

6. **Channel Icons** — Each with designated channel color. Channel icons are the ONLY place channel colors appear.

7. **Platform:**
   - Web: `lucide-react`
   - Mobile: `lucide-react-native`

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/iconography/SKILL.md
git commit -m "feat(cdg): add iconography foundation — Lucide standards, sizing, channel icons"
```

---

### Task 17: Write Accessibility Foundation

**Files:**
- Create: `chatverce-design/skills/foundations/accessibility/SKILL.md`

**Reference:** Spec Part 2: Accessibility (lines 514–587)

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: accessibility
description: Complete accessibility system — contrast minimums, touch targets, keyboard interaction, focus management, screen reader patterns, and reduced motion for web and mobile
user_invocable: false
---
```

**Required sections:**

1. **Philosophy** — Accessible from the start, not an afterthought. Perceivable, operable, understandable, robust (POUR).

2. **Contrast Minimums** — 4.5:1 body, 3:1 large, 7:1 preferred. Reference `color/contrast-matrix.md`.

3. **Touch/Click Targets** — iOS 44×44pt, Android 48×48dp, Web 44×44px.

4. **Keyboard Interaction (web)** — Full table from spec (Tab, Enter/Space, Escape, Arrow keys, Home/End).

5. **Focus Management** — Full table from spec (modal open/close, toast, route change, dropdown).

6. **ARIA Pattern Reference** — Table mapping component types to WAI-ARIA patterns (accordion, alert, modal, toast, tabs, etc.). Note: full per-component ARIA is in each component skill.

7. **Screen Reader:**
   - Web: `aria-live` regions, `aria-label`, `aria-describedby`
   - Mobile (iOS): VoiceOver — `accessible`, `accessibilityLabel`, `accessibilityRole`, `accessibilityState`
   - Mobile (Android): TalkBack — `accessibilityLiveRegion`, `importantForAccessibility`

8. **Dynamic Sizing** — Support Dynamic Type (iOS) and font scaling (Android). Test at 200% enlargement.

9. **Reduced Motion** — Reference motion foundation.

10. **Heading Hierarchy** — Sequential levels only. Never skip (h1 → h3).

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/accessibility/SKILL.md
git commit -m "feat(cdg): add accessibility foundation — contrast, targets, keyboard, focus, screen reader"
```

---

### Task 18: Write Privacy Foundation

**Files:**
- Create: `chatverce-design/skills/foundations/privacy/SKILL.md`

**Reference:** Spec Part 2: Privacy (lines 589–605)

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: privacy
description: Privacy as a design principle — data UI patterns, permission requests, transparency, and user data control for Chatverce
user_invocable: false
---
```

**Required sections:**

1. **Philosophy** — Privacy is a design principle, not a legal checkbox. Adapted from Apple HIG.

2. **Principles:**
   - Explain what data is collected and why, in plain language
   - Request only permissions needed, at the moment needed
   - Never collect data silently
   - Provide clear data deletion paths
   - On-device processing preferred

3. **Permission Request Pattern:**
   - Explain benefit BEFORE asking ("Enable notifications so you know when customers message")
   - Never request on first launch without context
   - Graceful degradation if denied

4. **Data Visibility:**
   - Show users what data their assistants can access
   - Visual indicator for AI-processed conversations
   - Clear distinction between stored and transient data

5. **Destructive Data Actions:**
   - Require explicit confirmation with consequences stated
   - "This will permanently delete..." pattern
   - Cooling-off period for account deletion

6. **Platform Considerations:**
   - iOS: Follow App Tracking Transparency guidelines. Privacy nutrition labels.
   - Android: Runtime permissions. Data safety section.
   - Web: Cookie consent (if applicable). Clear data practices page.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/privacy/SKILL.md
git commit -m "feat(cdg): add privacy foundation — data UI, permissions, transparency"
```

---

### Task 19: Write Writing Foundation

**Files:**
- Create: `chatverce-design/skills/foundations/writing/SKILL.md`

**Reference:** Spec Part 2: Writing (lines 607–694)

**Source material:** Read `chatverce-design/skills/voice-and-tone/SKILL.md` (v1) for existing content to refine and consolidate.

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: writing
description: Voice and tone, canonical terminology, and copy mechanics for all Chatverce UI text across web and mobile
user_invocable: false
---
```

**Required sections:**

1. **Voice & Tone**
   - The voice principle from spec: "calm, competent friend who happens to be brilliant at AI"
   - Voice attributes table (Clear, Confident, Warm, Brief) with do/don't examples
   - Tone by context table (Onboarding, Success, Error, Empty state, Destructive, Loading, Dashboard stats)
   - Button & action labels table

2. **Terminology** — Full canonical mapping table from spec (Deploy → Go live, Agent → Assistant, etc.). This is the SINGLE SOURCE OF TRUTH for terminology. All other skills reference this.

3. **Copy Mechanics:**
   - Lead with outcome, not mechanism
   - Use "you" and "your"
   - Present tense
   - No exclamation marks (except first-time milestones)
   - Error messages are solutions
   - Numbers over adjectives
   - Never blame the user
   - Sentence case for headings
   - No period at end of button labels

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/foundations/writing/SKILL.md
git commit -m "feat(cdg): add writing foundation — voice, tone, terminology, copy mechanics"
```

---

## Plan A Complete Checklist

After all 19 tasks, verify:

- [ ] `plugin.json` is at version `2.0.0`
- [ ] All v2 directories exist under `skills/`
- [ ] Philosophy layer: 2 files (design-ethos SKILL.md + ive-philosophy.md)
- [ ] Theme system: 7 files (SKILL.md + primitives + semantic-light + semantic-dark + 3 token outputs)
- [ ] Foundations: 10 SKILL.md files + 1 contrast-matrix.md
- [ ] v1 files still intact (cleanup is Plan D)
- [ ] All files have proper YAML frontmatter
- [ ] No hardcoded hex values in any SKILL.md (only in primitives.md and token output files)

```bash
find chatverce-design/skills/philosophy chatverce-design/skills/foundations -name "*.md" | wc -l
```

Expected: 19 files (2 philosophy + 7 theme-system + 2 color + 8 other foundations)

---

## Next Plans

After Plan A completes, these three plans can run **in parallel**:

- **Plan B: Patterns** — 14 behavioral pattern skills (`docs/superpowers/plans/2026-03-19-cdg-v2-plan-b-patterns.md`)
- **Plan C: Components** — 77 component skills + catalog (`docs/superpowers/plans/2026-03-19-cdg-v2-plan-c-components.md`)
- **Plan D: Enforcement & Tooling** — 4 guards + reviewer + command + v1 cleanup (`docs/superpowers/plans/2026-03-19-cdg-v2-plan-d-enforcement.md`)
