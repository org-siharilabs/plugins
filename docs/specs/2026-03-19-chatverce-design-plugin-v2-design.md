# Chatverce Design Guidelines (CDG) v2.0

**Date:** 2026-03-19
**Status:** Draft (revised)
**Supersedes:** `2026-03-13-chatverce-design-plugin-design.md` (v1.0)

## What This Is

The Chatverce Design Guidelines (CDG) are a comprehensive, agent-agnostic design system for building Chatverce across web and mobile. Written as prose guidelines that any AI coding agent can follow — currently delivered as a Claude Code plugin, but the content is tool-independent and will be consumed by Cursor, Gemini CLI, and others in the future.

The guidelines follow Apple's Human Interface Guidelines structure (Philosophy → Foundations → Patterns → Components) enriched with Jony Ive's design philosophy, production-grade token architecture, and enforcement rules.

## Problem

The chatverce-design plugin (v1.0) has critical gaps preventing production use:

- **No dark mode** — Colors are hardcoded hex values with no theme switching
- **No semantic token architecture** — Code uses raw primitives; no intent-based abstraction
- **11% component coverage** — 8 of 71 standard UI components specified
- **No enforcement** — Plugin teaches rules but doesn't prevent violations during implementation
- **No accessibility system** — WCAG mentioned but no contrast matrix, ARIA patterns, or keyboard specs
- **No patterns layer** — Components exist but no behavioral guidance (when to show a toast vs alert, how loading works, onboarding flows)
- **No mobile platform parity** — Web-first assumptions; mobile treated as token format, not design target
- **Philosophy is functional, not emotional** — Ive principles listed as checklist items, missing the deeper ethos of caring, intuition, and focus
- **Missing foundational specs** — No breakpoints, z-index scale, motion tokens, materials/depth, privacy patterns, or icon standards

AI agents use these guidelines as their "design brain" during feature implementation. Every gap is a gap in their output.

## Goals

1. **Agent-agnostic design guidelines** — Written as universal prose, not Claude-specific instructions
2. **Full dark/light theme support** via 3-layer token architecture
3. **Web + Mobile parity** — Both platforms are first-class with platform-specific guidance
4. **Apple HIG-inspired structure** — Philosophy → Foundations → Patterns → Components
5. **Complete component coverage** — All 71 component.gallery components + Chatverce-specific
6. **Patterns layer** — Behavioral guidance for loading, onboarding, feedback, AI interactions, etc.
7. **Enforcement system** — Guards that prevent violations during implementation
8. **Ive philosophy as living ethos** — Not a checklist but a design culture
9. **Clean break from v1** — Chatverce codebase will be updated to match

## Non-Goals

- Figma/Sketch integration or design tool exports
- Component library code (guidelines define specs, not implementations)
- Visual regression testing setup
- Building for other AI agents now (agent-agnostic content, Claude Code delivery)

## Tech Stack

- **Web:** Next.js + Tailwind CSS + shadcn/ui
- **Mobile:** Expo / React Native + NativeWind (Tailwind for RN)
- **Shared tokens:** Tailwind config shared across web and mobile via NativeWind
- **Token outputs:** CSS custom properties (web), Tailwind config (shared), NativeWind-compatible (mobile)

The `cv-*` token classes work identically in both Next.js and Expo thanks to NativeWind.

---

## Architecture Overview

```
Chatverce Design Guidelines (CDG)
│
├── Philosophy                     ← WHY we design this way
│   ├── Design Ethos               (caring, intuition, focus, thoroughness)
│   └── The Ive Test               (5 questions for every screen)
│
├── Foundations                     ← HOW we express design decisions
│   ├── Theme System               (3-layer tokens, dark/light, token rule)
│   ├── Color                      (semantic color, contrast, channel colors)
│   ├── Typography                 (scale, dynamic sizing, hierarchy, platform rules)
│   ├── Layout                     (breakpoints, z-index, grid, spacing, safe areas)
│   ├── Motion                     (duration, easing, reduced-motion, haptics)
│   ├── Materials & Depth          (elevation, layering, translucency)
│   ├── Iconography                (Lucide, sizing, stroke, pairing)
│   ├── Accessibility              (contrast matrix, ARIA, keyboard, focus, screen reader)
│   ├── Privacy                    (data UI, permissions, transparency)
│   └── Writing                    (voice & tone, terminology, copy mechanics)
│
├── Patterns                        ← WHAT behaviors we support
│   ├── Loading                    (skeleton, optimistic UI, progress, AI processing)
│   ├── Onboarding                 (first-run, progressive disclosure, setup wizards)
│   ├── Feedback                   (success/error/warning/info — when, how, where)
│   ├── Searching                  (search UX, filters, results, empty results)
│   ├── Settings                   (preferences, toggles, destructive settings)
│   ├── Modality                   (modal vs inline, interruption hierarchy)
│   ├── Entering Data              (form flows, validation, progressive disclosure)
│   ├── Managing Accounts          (auth, profile, team management)
│   ├── Managing Notifications     (in-app, push, batching, preferences)
│   ├── Undo & Recovery            (undo/redo, error recovery, data loss prevention)
│   ├── AI Interactions            (processing states, uncertainty, correction, transparency)
│   ├── Real-time Communication    (live chat, typing indicators, read receipts, presence)
│   ├── Channel Switching          (WhatsApp ↔ Telegram ↔ Webchat context)
│   └── Going Live                 (deployment flows, preview, status transitions)
│
├── Components                      ← WHICH UI elements we use
│   ├── catalog.md                 (index of all 77 components)
│   └── {component}/SKILL.md      (per-component spec with platform considerations)
│
└── Enforcement                     ← RULES we enforce during implementation
    ├── token-guard                (no hardcoded values)
    ├── theme-guard                (both themes must work)
    ├── a11y-guard                 (accessibility baseline)
    └── pattern-guard              (convention compliance)
```

---

## Part 1: Philosophy

### Design Ethos

The Chatverce design philosophy is rooted in Jony Ive's approach to product design. These are not rules to follow — they are a way of thinking about every decision.

#### 1. Caring Is the X-Factor

> "I think the majority of our manufactured environment is characterized by carelessness." — Jony Ive

> "We are capable of discerning far more than we are capable of articulating." — Jony Ive

Users can **sense** care even when they can't articulate it. The difference between a good product and a great one isn't features — it's the irrational attention to details that no spec document would require. The hover state that feels just right. The loading message that makes you smile instead of wait. The error recovery that guides instead of blames.

Apple shot real flowers for 285 hours (24,000 shots) for the Apple Watch face — not because users would know, but because they would **feel** the difference between that and an animation cranked out in a sprint.

**What this means for Chatverce:** Before shipping any feature, ask: would someone sense the care that went into this? If the answer is "it works, but..." — it's not done.

#### 2. Inevitable Simplicity

> "True simplicity is derived from so much more than just the absence of clutter and ornamentation. It's about bringing order to complexity." — Jony Ive

Solutions should feel like the only way they could possibly be. Not "designed" — discovered. This means:

- One primary action per screen
- Navigation mirrors the user's mental model, not the database schema
- Flows feel like natural conversations, not form submissions
- The product does the hard work so the user doesn't have to

**What this means for Chatverce:** "Go live" instead of "Deploy agent to production endpoint." Smart defaults with progressive disclosure. AI handles formatting and validation silently. Complex workflows presented as simple wizards.

#### 3. Breathing Room — Design the Space, Not Just the Content

> "When there's no open space, the customer doesn't engage. He becomes disconnected." — Mateusz Sinilo on Ive's philosophy

Good design leaves room for the user's **intuition to interact**. Minimalist products encourage a dialogue — the user's mind fills the space, creating personal connection. Too much design = disconnection.

**What this means for Chatverce:**
- White space is not empty — it's an invitation
- Don't fill every pixel with information or controls
- Let dashboards breathe; data surrounded by space reads as confident
- Empty states are opportunities for personality, not apologies
- Restraint is a design strength — what you leave out defines the product as much as what you include

#### 4. Focus as Discipline

> "What focus means is saying 'no' to something that you, with every bone in your body, think is a phenomenal idea, and you wake up thinking about it, but you say no to it because you're focusing on something else." — Jony Ive

Focus isn't time management — it's philosophical discipline. When Jobs returned to Apple, he cut 40 products to 4 using a 2×2 grid. Constraints liberate creativity.

**What this means for Chatverce:**
- Every screen has one job. If you can't state it in one sentence, the screen is doing too much.
- Every feature competes for attention. Adding a feature without removing complexity elsewhere violates the Ive principle.
- Each view deliberately excludes. The absence is intentional.

#### 5. Emotional Intention

> "He won't be able to articulate it, but we hope that he will sense the care that went into it." — Jony Ive

Design for the emotional brain, not just the rational one. Every major interaction should have an intended feeling:

| Context | Intended Feeling |
|---------|-----------------|
| Dashboard | Calm confidence — "everything is running" |
| Onboarding | Guided ease — "this will only take a minute" |
| Going live | Quiet pride — "you built this" |
| Error | Reassurance — "we'll fix this together" |
| Empty state | Anticipation — "something great is about to happen" |
| Conversation view | Efficient flow — "you're in control" |
| Settings | Trust — "your data, your rules" |

#### 6. Thoroughness in Every Inch

> "The product feels like it was designed with complete accuracy in every inch." — on Apple Watch

A button isn't done when it has the right color. It's done when:
- The hover state feels responsive but not jumpy
- The loading state communicates progress without anxiety
- The disabled state explains *why* it's disabled
- The destructive variant makes the user pause just enough
- The touch target works for large fingers on small screens
- The focus ring is visible for keyboard users
- The motion respects `prefers-reduced-motion`
- The label uses approved terminology

### The Ive Test

Apply to every screen, every component, every interaction:

1. **Can anything be removed** without losing function?
2. **Does the UI defer** to the user's content and goal?
3. **Would a non-technical business owner** understand this in under 3 seconds?
4. **Does it feel inevitable** — the only way it could be?
5. **Would someone sense the care** that went into this?

### Anti-Patterns (Hard Rules — Never Do)

1. Never use technical jargon in UI copy
2. Never show raw error messages, stack traces, or error codes to users
3. Never add a feature without removing complexity elsewhere
4. Never use decoration that doesn't serve comprehension
5. Never sacrifice usability for aesthetics
6. Never add controls serving less than 80% of users to the default view
7. Never use placeholder text as the only label
8. Never use color alone to communicate meaning
9. Never ship a screen without considering both themes
10. Never ship a component without considering both platforms

---

## Part 2: Foundations

### Theme System

#### Three-Layer Token Architecture

**Layer 1 — Primitives:** Raw values. Never referenced in application code. Only place hex values exist.

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

**Channel colors** (standalone primitives, not scaled):

| Token | Value | Usage |
|-------|-------|-------|
| `cv-channel-whatsapp` | `#25D366` | WhatsApp channel indicator only |
| `cv-channel-telegram` | `#26A5E4` | Telegram channel indicator only |
| `cv-channel-webchat` | `cv-gray-500` | Webchat channel indicator |
| `cv-channel-telephone` | `cv-navy-700` | Telephone channel indicator |
| `cv-channel-email` | `cv-navy-700` | Email channel indicator |

**Rule:** Channel colors are never used as primary/accent. Chatverce identity is navy + teal, not messaging green.

**Layer 2 — Semantic:** Intent-based tokens that switch values per theme. This is what application code references.

**Background tokens:**

| Token | Light | Dark |
|-------|-------|------|
| `cv-bg-primary` | `cv-gray-50` | `cv-navy-950` |
| `cv-bg-surface` | `cv-white` | `cv-navy-900` |
| `cv-bg-surface-elevated` | `cv-white` | `cv-navy-800` |
| `cv-bg-surface-sunken` | `cv-gray-100` | `cv-navy-950` |
| `cv-bg-accent` | `cv-teal-500` | `cv-teal-400` |
| `cv-bg-accent-subtle` | `cv-teal-50` | `cv-teal-950` |
| `cv-bg-accent-hover` | `cv-teal-600` | `cv-teal-300` |
| `cv-bg-destructive` | `cv-red-500` | `cv-red-400` |
| `cv-bg-destructive-subtle` | `cv-red-50` | `cv-red-950` |

**Text tokens:**

| Token | Light | Dark |
|-------|-------|------|
| `cv-text-primary` | `cv-gray-900` | `cv-gray-50` |
| `cv-text-secondary` | `cv-gray-500` | `cv-gray-400` |
| `cv-text-tertiary` | `cv-gray-400` | `cv-gray-500` |
| `cv-text-on-accent` | `cv-white` | `cv-navy-950` |
| `cv-text-on-destructive` | `cv-white` | `cv-white` |
| `cv-text-link` | `cv-teal-600` | `cv-teal-400` |
| `cv-text-success` | `cv-green-700` | `cv-green-400` |
| `cv-text-warning` | `cv-amber-700` | `cv-amber-400` |
| `cv-text-error` | `cv-red-600` | `cv-red-400` |

**Border tokens:**

| Token | Light | Dark |
|-------|-------|------|
| `cv-border-default` | `cv-gray-200` | `cv-navy-700` |
| `cv-border-strong` | `cv-gray-300` | `cv-navy-600` |
| `cv-border-accent` | `cv-teal-500` | `cv-teal-400` |
| `cv-border-error` | `cv-red-500` | `cv-red-400` |

**Status tokens:**

| Token | Light | Dark |
|-------|-------|------|
| `cv-status-success` | `cv-green-500` | `cv-green-400` |
| `cv-status-warning` | `cv-amber-500` | `cv-amber-400` |
| `cv-status-error` | `cv-red-500` | `cv-red-400` |
| `cv-status-info` | `cv-teal-500` | `cv-teal-400` |

**Overlay/shadow:**

| Token | Light | Dark |
|-------|-------|------|
| `cv-shadow-color` | `0,0,0` | `0,0,0` |
| `cv-overlay` | `cv-gray-900/50%` | `cv-black/60%` |

**Non-color semantic tokens (same in both themes):**

| Category | Tokens |
|----------|--------|
| Typography (families) | `cv-font-sans: 'Inter', system-ui, sans-serif`, `cv-font-mono: 'JetBrains Mono', monospace` |
| Typography (scale) | `cv-text-display: 600 36-48px/1.1`, `cv-text-h1: 600 28-32px/1.2`, `cv-text-h2: 600 20-24px/1.3`, `cv-text-h3: 500 16-18px/1.4`, `cv-text-body: 400 14-16px/1.5`, `cv-text-caption: 400 12-13px/1.4`, `cv-text-code: 400 13-14px/1.5 (mono)` |
| Radius | `cv-radius-sm: 6px`, `cv-radius-md: 8px`, `cv-radius-lg: 12px`, `cv-radius-full: 9999px` |
| Spacing | `cv-space-1: 4px` through `cv-space-24: 96px` (4px base: 4·8·12·16·20·24·32·40·48·64·80·96) |
| Shadows | `cv-shadow-sm`, `cv-shadow-md`, `cv-shadow-lg` (using `cv-shadow-color` for theme adaptation) |
| Z-index | `cv-z-dropdown: 100`, `cv-z-sticky: 200`, `cv-z-overlay: 300`, `cv-z-modal: 400`, `cv-z-popover: 500`, `cv-z-toast: 600`, `cv-z-tooltip: 700` |
| Motion | `cv-duration-fast: 150ms`, `cv-duration-normal: 300ms`, `cv-duration-slow: 500ms` |
| Easing | `cv-ease-default: cubic-bezier(0.4, 0, 0.2, 1)`, `cv-ease-out: cubic-bezier(0, 0, 0.2, 1)`, `cv-ease-in: cubic-bezier(0.4, 0, 1, 1)` |
| Breakpoints | `cv-breakpoint-sm: 640px`, `cv-breakpoint-md: 768px`, `cv-breakpoint-lg: 1024px`, `cv-breakpoint-xl: 1280px`, `cv-breakpoint-2xl: 1536px` |

**Layer 3 — Component:** Specific to each component. References semantic tokens. Defined within each component's guideline file, not centrally.

```
cv-button-primary-bg: var(cv-bg-accent)
cv-button-primary-text: var(cv-text-on-accent)
cv-button-primary-hover-bg: var(cv-bg-accent-hover)
cv-card-bg: var(cv-bg-surface)
cv-card-border: var(cv-border-default)
cv-input-bg: var(cv-bg-surface)
cv-input-focus-border: var(cv-border-accent)
```

**The Token Rule:** Application code references semantic or component tokens only. Never primitives. Never raw hex/rgb. Exceptions: primitive definition files, third-party overrides with `/* cv-override */`, one-off values with `/* cv-exception: reason */`.

### Color

Full color foundation specification. Defines: primitive palette generation (how scales are derived), semantic color assignment rationale, contrast requirements, color accessibility matrix, platform-specific color behavior.

**Key principles:**
- Use color consistently — same color must never mean different things
- Ensure all colors work in light, dark, and increased-contrast contexts
- Test under varied lighting conditions and on multiple devices
- Never rely solely on color to differentiate objects or communicate meaning (use shape/icon + color)
- Minimum contrast: 4.5:1 for body text, 3:1 for large/bold text, 7:1 preferred

**Contrast matrix:** Every semantic text/background pair verified against WCAG AA in both themes. Pairs that don't meet AA get adjusted at the primitive level. Any agent implementing UI must never create a text/background combination not in the matrix.

### Typography

| Role | Font | Weight | Size | Line Height |
|------|------|--------|------|-------------|
| Display | Inter | 600 | 36-48px | 1.1 |
| Heading 1 | Inter | 600 | 28-32px | 1.2 |
| Heading 2 | Inter | 600 | 20-24px | 1.3 |
| Heading 3 | Inter | 500 | 16-18px | 1.4 |
| Body | Inter | 400 | 14-16px | 1.5 |
| Caption | Inter | 400 | 12-13px | 1.4 |
| Code/Agent | JetBrains Mono | 400 | 13-14px | 1.5 |

**Why Inter:** Geometric, clean, excellent legibility at small sizes, optimized for screens. Available on Google Fonts and supported natively by NativeWind.

**Platform considerations:**
- **Web:** System font stack fallback: `'Inter', ui-sans-serif, system-ui, sans-serif`
- **Mobile:** Load Inter via `expo-font`. Minimum body text: 16px (prevents iOS auto-zoom on inputs). Use platform-aware scaling for accessibility (Dynamic Type on iOS, font scale on Android).

**Dynamic sizing:** Support user font size preferences. Never use fixed pixel sizes for text — use rem/scale-aware units. Test at 200% text enlargement (WCAG requirement).

### Layout

**Breakpoints (web):**

| Name | Min Width | Usage |
|------|-----------|-------|
| `sm` | 640px | Large phones landscape |
| `md` | 768px | Tablets portrait |
| `lg` | 1024px | Tablets landscape, small laptops |
| `xl` | 1280px | Desktops |
| `2xl` | 1536px | Large desktops |

**Mobile layout (Expo/NativeWind):**
- No CSS breakpoints — use `useWindowDimensions()` or NativeWind responsive prefixes
- Always account for safe areas (notch, Dynamic Island, home indicator, Android navigation bar)
- Use `SafeAreaView` or `useSafeAreaInsets()` from `react-native-safe-area-context`
- Status bar: themed to match current screen (light-content on dark bg, dark-content on light bg)

**Z-index scale:**

| Token | Value | Usage |
|-------|-------|-------|
| `cv-z-dropdown` | 100 | Dropdown menus, combobox lists |
| `cv-z-sticky` | 200 | Sticky headers, fixed navigation |
| `cv-z-overlay` | 300 | Overlays, backdrops |
| `cv-z-modal` | 400 | Modals, dialogs |
| `cv-z-popover` | 500 | Popovers, floating elements |
| `cv-z-toast` | 600 | Toast notifications |
| `cv-z-tooltip` | 700 | Tooltips (always on top) |

**Mobile:** Z-index mostly handled by React Navigation's modal stack and portal-based components. Use `react-native-portalize` or `@gorhom/portal` for elements that need to escape their parent hierarchy.

**Spacing scale (4px base unit):**
```
4 · 8 · 12 · 16 · 20 · 24 · 32 · 40 · 48 · 64 · 80 · 96
```

**Breathing room principle:** Content-dense screens (dashboards, tables, conversation lists) must have deliberate whitespace. Minimum padding between sections: `cv-space-6` (24px). Cards never touch edges or each other — minimum gap: `cv-space-4` (16px).

### Motion

**Duration tokens:**

| Token | Value | Usage |
|-------|-------|-------|
| `cv-duration-fast` | 150ms | Micro-interactions (hover, focus, toggle) |
| `cv-duration-normal` | 300ms | Panel transitions, page changes |
| `cv-duration-slow` | 500ms | Complex animations (only when justified) |

**Easing tokens:**

| Token | Value | Usage |
|-------|-------|-------|
| `cv-ease-default` | `cubic-bezier(0.4, 0, 0.2, 1)` | General transitions |
| `cv-ease-out` | `cubic-bezier(0, 0, 0.2, 1)` | Elements entering |
| `cv-ease-in` | `cubic-bezier(0.4, 0, 1, 1)` | Elements exiting |

**Rules (Ive's principle: motion serves comprehension, never decoration):**
- Enter: fade + slight upward translate (8px)
- Exit: fade only
- AI-working: gentle teal pulse (only allowed "decorative" motion)
- Never: bouncing, spinning, confetti, particles, parallax

**Reduced motion:**
```css
@media (prefers-reduced-motion: reduce) {
  --cv-duration-fast: 0ms;
  --cv-duration-normal: 0ms;
  --cv-duration-slow: 0ms;
}
```
- Web: always `motion-safe:` prefix on Tailwind animations
- Mobile: check `AccessibilityInfo.isReduceMotionEnabled()` or use `useReducedMotion()` from `react-native-reanimated`

**Haptics (mobile only):**

| Event | Haptic Type | When |
|-------|------------|------|
| Button press | Light impact | Primary action buttons |
| Toggle switch | Light impact | On state change |
| Destructive action confirm | Warning notification | Before irreversible action |
| Success | Success notification | Task completion (going live, save) |
| Error | Error notification | Form validation failure |
| Pull to refresh | Selection changed | At refresh threshold |
| Long press menu | Medium impact | On menu appearance |

Use `expo-haptics` for haptic feedback. Never use haptics without a visual counterpart. Respect system haptics settings.

### Materials & Depth

Visual layering system that goes beyond shadows to communicate spatial hierarchy.

**Elevation levels:**

| Level | Usage | Web | Mobile |
|-------|-------|-----|--------|
| 0 (Base) | Page background | `cv-bg-primary` | `cv-bg-primary` |
| 1 (Surface) | Cards, panels, input fields | `cv-bg-surface` + `cv-shadow-sm` | `cv-bg-surface` (no shadow on Android, subtle on iOS) |
| 2 (Elevated) | Dropdowns, popovers | `cv-bg-surface-elevated` + `cv-shadow-md` | `cv-bg-surface-elevated` + platform shadow |
| 3 (Overlay) | Modals, drawers | `cv-bg-surface-elevated` + `cv-shadow-lg` + overlay backdrop | Full-screen modal or bottom sheet |
| 4 (Toast) | Toasts, tooltips | `cv-bg-surface-elevated` + `cv-shadow-lg` | System-level overlay |

**Platform considerations:**
- **Web:** Shadows create depth. Dark mode shadows use darker `cv-shadow-color`.
- **iOS:** Minimal shadows. Depth communicated through layering, blur, and translucency. Use `BlurView` from `expo-blur` for elevated surfaces when appropriate.
- **Android:** Material-style elevation with `elevation` prop. Shadow rendering differs from iOS — use `cv-shadow-*` tokens which adapt per platform.

**Translucency:** Used sparingly for navigation bars and tab bars where content scrolls underneath. Never for content surfaces (readability first).

### Iconography

- **Icon set:** Lucide icons (thin, consistent stroke weight)
- **Default size:** 20px, 1.5px stroke
- **Touch target:** Icon buttons always have minimum 44px hit area (the icon can be 20px, but the tappable area is 44px)
- **Never fill** icons unless indicating active/selected state
- **Mobile:** Use `lucide-react-native` for Expo compatibility

**Channel icons:** WhatsApp, Telegram, Webchat, Telephone, Email — each with their designated channel color. Channel icons are the only place those colors appear.

### Accessibility

**Contrast minimums:**
- Body text (< 18px): 4.5:1 ratio (WCAG AA)
- Large text (≥ 18px or ≥ 14px bold): 3:1 ratio
- Preferred: 7:1 for all text

**Touch/click targets:**
- iOS: Minimum 44×44pt
- Android: Minimum 48×48dp
- Web: Minimum 44×44px for interactive elements

**Keyboard interaction (web):**

| Key | Behavior |
|-----|----------|
| `Tab` / `Shift+Tab` | Move focus forward/backward |
| `Enter` / `Space` | Activate button, link, control |
| `Escape` | Close modal, popover, dropdown, drawer, toast |
| `Arrow keys` | Navigate within composite widgets |
| `Home` / `End` | First/last item in lists |

**Focus management:**

| Event | Behavior |
|-------|----------|
| Modal/Drawer open | Focus trapped inside, starts at first focusable element |
| Modal/Drawer close | Focus returns to trigger element |
| Toast appears | No focus steal; `aria-live` announces |
| Route change | Focus moves to main content or page heading |
| Dropdown open | Focus moves to first item |

**Screen reader:**
- All interactive elements have accessible names
- State changes announced via `aria-live` regions (web) / `accessibilityLiveRegion` (mobile)
- Images have descriptive `alt` text or `accessibilityLabel`
- Dynamic content changes announced without stealing focus

**Mobile accessibility:**
- Support VoiceOver (iOS) and TalkBack (Android)
- Use `accessible`, `accessibilityLabel`, `accessibilityRole`, `accessibilityState` props
- Group related elements with `accessibilityElementsHidden` and `importantForAccessibility`
- Support Dynamic Type (iOS) and font scaling (Android)
- Test with screen readers on both platforms

**ARIA pattern mapping:** Each component maps to its WAI-ARIA pattern (web) and corresponding React Native accessibility role (mobile). Defined per component in the Components layer.

### Privacy

Chatverce handles business conversations and customer data. Privacy is a design principle, not a legal checkbox.

**Principles (adapted from Apple HIG):**
- Explain what data is collected and why, in plain language
- Request only the permissions you need, at the moment you need them
- Never collect data silently — visible indicators when recording/processing
- Provide clear data deletion paths
- On-device processing preferred where possible

**UI patterns:**
- Permission requests: explain benefit before asking (e.g., "Enable notifications so you know when customers message")
- Data visibility: show users what data their assistants can access
- Destructive data actions: require explicit confirmation with clear consequences stated
- Conversation data: visual indicator of which conversations are AI-processed vs human

### Writing

#### Voice & Tone

**The voice:** Chatverce speaks like a calm, competent friend who happens to be brilliant at AI — but never makes you feel stupid. Think: the best Apple Store employee. They explain what matters, not how it works.

**Voice attributes:**

| Attribute | Do | Don't |
|-----------|-----|-------|
| Clear | "Your assistant is live" | "Agent deployed to production endpoint" |
| Confident | "This will reply automatically" | "This should help with automating replies" |
| Warm | "Nice — your first assistant is ready" | "Agent creation successful" |
| Brief | "Replies in 2 seconds" | "Average response latency is approximately 2 seconds" |

**Tone by context:**

| Context | Tone | Example |
|---------|------|---------|
| Onboarding | Encouraging, guiding | "Let's set up your first assistant — takes about 3 minutes" |
| Success | Warm, understated | "All set. Your assistant is handling conversations now." |
| Error | Calm, solution-focused | "Something went wrong connecting to WhatsApp. Reconnect in Settings." |
| Empty state | Inviting, forward-looking | "No conversations yet. They'll show up here once your assistant is live." |
| Destructive | Clear, no drama | "This will permanently delete the assistant and its conversation history." |
| Loading | Transparent | "Setting things up..." |
| Dashboard stats | Factual, proud | "Your assistants handled 3,421 conversations this week" |

#### Terminology

Single source of truth. Every AI agent must use these terms:

| Technical | Chatverce Says |
|-----------|---------------|
| Deploy | Go live |
| Agent | Assistant |
| Node | Step |
| Trigger | "When this happens" |
| Action | "Do this" |
| Conditional/Branch | "Check if" |
| Webhook | Connection |
| API key | Secret key |
| Workflow | Flow |
| NLP/Intent | "Understands" |
| Fallback | "If unsure, do this" |
| Variable | Field |
| Endpoint | *(never show to user)* |
| Latency | Speed / response time |
| Uptime | Availability / "always on" |

#### Copy Mechanics

- Lead with outcome, not mechanism
- Use "you" and "your"
- Present tense
- No exclamation marks in UI (except first-time milestones)
- Error messages are solutions, not descriptions of failure
- Numbers over adjectives ("Replies in 2 seconds" not "Replies quickly")
- Never blame the user
- Sentence case for headings (not Title Case)
- No period at end of button labels

---

## Part 3: Patterns

Patterns describe **how users accomplish tasks**. They're the behavioral layer between foundations (design elements) and components (UI elements). Each pattern answers: what is the user trying to do, what should they feel, and which components work together to support them?

### Pattern Skill Template

Each pattern follows this structure:

```markdown
# {Pattern Name}

## What the User Is Doing
{Behavioral description — the task, not the UI}

## Emotional Intention
{What the user should feel during this pattern}

## How It Works
{Step-by-step flow with decision points}

## Components Used
{Which catalog components participate in this pattern}

## Platform Considerations
### Web
### iOS
### Android

## Do / Don't
{Specific guidance with examples}

## Chatverce-Specific
{How this pattern applies to Chatverce's product domain}
```

### Pattern Inventory (14 patterns)

| Pattern | Description | Key Components |
|---------|-------------|----------------|
| **Loading** | How Chatverce communicates progress and processing time | Skeleton, Spinner, Progress bar, AI working indicator |
| **Onboarding** | First-run experience, progressive disclosure, setup wizards | Progress indicator, Modal, Form, Empty state, Hero |
| **Feedback** | Success, error, warning, info — when to use which and where | Toast, Alert, Badge, Modal |
| **Searching** | Search UX, filters, results display, empty results | Search input, List, Empty state, Badge, Skeleton |
| **Settings** | Preference management, destructive settings, account settings | Toggle, Select, Form, Modal (confirm), Navigation |
| **Modality** | When to use modal vs inline, interruption hierarchy | Modal, Drawer, Popover, Alert |
| **Entering Data** | Form flows, inline validation, progressive disclosure, multi-step | Form, Text input, Textarea, Select, Checkbox, Radio button, File upload, Progress indicator |
| **Managing Accounts** | Auth, profile editing, team management, roles | Form, Avatar, Badge, Table, Modal |
| **Managing Notifications** | In-app, push, batching, preference controls | Toast, Badge, Toggle, List, Alert |
| **Undo & Recovery** | Undo/redo, error recovery, data loss prevention | Toast (with undo action), Alert, Modal |
| **AI Interactions** | AI processing states, uncertainty, correction, transparency | AI working indicator, Spinner, Toast, Alert, Progress bar |
| **Real-time Communication** | Live chat, typing indicators, read receipts, presence | Avatar, Badge, Spinner, List, Conversation thread |
| **Channel Switching** | Cross-channel context (WhatsApp ↔ Telegram ↔ Webchat) | Channel indicator, Badge, Tabs, Segmented control |
| **Going Live** | Deployment flow, preview, status transitions | Progress indicator, Button, Modal, Toast, Alert |

### AI Interactions Pattern (expanded — Chatverce-critical)

Since Chatverce is an AI product, this pattern is specified at full depth:

**Processing states:**

| State | Visual | Label | Duration |
|-------|--------|-------|----------|
| Thinking | Teal pulse animation | "Thinking..." | 0-3 seconds |
| Working | Teal pulse + progress text | "Setting things up..." / "Connecting to WhatsApp..." | 3-30 seconds |
| Long-running | Progress bar + detail | "Training your assistant (2 of 5 steps complete)" | 30+ seconds |
| Complete | Checkmark fade-in | "All set." | Flash then dismiss |
| Failed | Coral icon + solution | "Couldn't connect. [Try again]" | Persists until action |

**Uncertainty/confidence:**
- Never show confidence scores to users (they don't know what "87% confidence" means)
- Instead: use language like "This looks like a question about billing" (confident) vs "I'm not sure what this is about — can you help?" (uncertain)
- AI suggestions should be clearly distinguishable from confirmed data

**Correction:**
- Always let users correct AI outputs (edit, regenerate, reject)
- Show what the AI did and make it easy to undo
- "Smart suggestions" pattern: AI proposes, user confirms

**Transparency (from Apple HIG Generative AI):**
- Communicate where AI is involved — never trick users into thinking AI content is human
- Explain AI capabilities and limitations in onboarding
- Make it clear when a response is AI-generated vs from a human agent

---

## Part 4: Components

### Component Skill Template (updated with platform considerations)

```markdown
---
name: {component-name}
description: {one-liner}
---

# {Component Name}

**Gallery reference:** https://component.gallery/components/{name}/
**Aliases:** {alt names from gallery}

## When to Use
- {use case}

## When NOT to Use
- {anti-use case — use {alternative} instead}

## Anatomy
{Parts of the component with semantic token references}

## Variants
{Variant table — name, visual description, when to use}

## Interaction States
{Default, hover, active/pressed, focus, disabled (with reason), loading, error, success}
{Emotional intention for key states}

## Chatverce Styling
{Component tokens referencing semantic layer}

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
{Web-specific behavior, keyboard interaction, hover states}

### iOS (Expo + NativeWind)
{iOS-specific behavior, gestures, haptics, safe areas}

### Android (Expo + NativeWind)
{Android-specific behavior, back button, material elevation}

## Accessibility
{ARIA roles/attributes (web), accessibilityRole/Label (mobile), keyboard, focus, screen reader}

## Code Examples

### Web
{shadcn + Tailwind + cv-tokens}

### Mobile
{React Native + NativeWind + cv-tokens}

## Anti-Patterns
- Never: {thing}
```

### Component Depth Tiers

| Tier | Depth | Components |
|------|-------|------------|
| **Full** | All sections, multiple variants, both platform examples, interaction states with emotional intention | Button, Card, Modal, Form, Text input, Textarea, Select, Table, Navigation, Tabs, Toast, Drawer, Alert, Dropdown menu, Empty state, Skeleton, Popover, Checkbox, Radio button, Toggle, Combobox, Datepicker, File upload, Progress indicator, Search input, Avatar, Badge, Tooltip, Agent builder canvas, Agent builder node, Conversation thread |
| **Standard** | All sections concise, 1-2 variants, primary platform example | Accordion, Breadcrumbs, Button group, Fieldset, Footer, Header, Heading, Hero, Icon, Image, Label, Link, List, Pagination, Progress bar, Segmented control, Separator, Slider, Spinner, Stepper, Tree view, AI working indicator, Channel indicator, Dashboard stat card |
| **Minimal** | When to use, styling, accessibility, single example per platform | Carousel, Color picker, Date input, File, Quote, Rating, Rich text editor, Skip link, Stack, Video, Visually hidden |

### Catalog.md

Lightweight index that any AI agent reads first to discover available components:

```markdown
| Component | Path | Tier | Aliases | Platform Notes |
|-----------|------|------|---------|---------------|
| Accordion | components/accordion/ | Standard | Collapse, Collapsible | Web: details/summary. Mobile: Animated.View |
| Alert | components/alert/ | Full | Notification, Banner | Web: div[role=alert]. Mobile: View + accessibilityLiveRegion |
| Modal | components/modal/ | Full | Dialog, Popup | Web: dialog element. iOS: presentationStyle. Android: Modal component |
| Drawer | components/drawer/ | Full | Sheet, Tray, Flyout | Web: side panel. Mobile: Bottom sheet (react-native-bottom-sheet) |
| Toast | components/toast/ | Full | Snackbar | Web: portal + aria-live. Mobile: react-native-toast-message or custom |
| ...
```

**Common Compositions** (which components work together):

| Pattern | Components |
|---------|-----------|
| Form | Form + Fieldset + Label + Text input/Textarea/Select/Checkbox/Radio button + Button |
| Data view | Table + Pagination + Search input + Empty state + Skeleton |
| Settings page | Navigation + Tabs + Form + Toggle + Accordion |
| Overlay | Modal or Drawer + Button + Form (optional) |
| Feedback | Toast or Alert + Icon + Button (optional) |
| Agent builder | Agent builder canvas + Agent builder node + Drawer + Dropdown menu + Popover |
| Conversation | Conversation thread + Avatar + Badge + Channel indicator + Skeleton |
| Dashboard | Dashboard stat card + Card + Table + Progress bar + Heading |

### Platform-Specific Component Mapping

Some components render fundamentally differently per platform:

| Component | Web | iOS | Android |
|-----------|-----|-----|---------|
| Modal | Centered overlay with backdrop | `presentationStyle: "pageSheet"` bottom card | Full-screen or bottom sheet |
| Drawer | Side panel sliding from edge | Bottom sheet (`@gorhom/bottom-sheet`) | Bottom sheet |
| Navigation | Sidebar (desktop), bottom tabs (mobile web) | `TabNavigator` with bottom tabs | `TabNavigator` with bottom tabs |
| Select | Native `<select>` or custom dropdown | `ActionSheet` or wheel picker | Material dropdown |
| Toast | Fixed position top-right or bottom-center | Top of screen, below Dynamic Island | Bottom of screen, above nav |
| Datepicker | Calendar dropdown | Native `DateTimePicker` wheel | Material date picker |
| Alert | `div[role=alert]` inline | `Alert.alert()` for system alerts, inline for in-page | `Alert.alert()` or inline |
| Popover | Floating element positioned relative to trigger | `Popover` (iOS native) or custom | Custom positioned view |

---

## Part 5: Enforcement

Four enforcement guards that teach AI agents what to check during implementation.

### token-guard

Catches hardcoded values that bypass the token system.

| Violation | Example | Fix |
|-----------|---------|-----|
| Raw hex color | `bg-[#2EC4B6]` or `style={{color: '#1B2A4A'}}` | `bg-cv-accent` or `className="text-cv-primary"` |
| Arbitrary z-index | `z-[999]` or `zIndex: 50` | `z-cv-modal` |
| Arbitrary font | `font-['Helvetica']` | `font-cv-sans` |
| Arbitrary shadow | `shadow-[0_4px_12px...]` | `shadow-cv-md` |
| Arbitrary radius | `rounded-[10px]` | `rounded-cv-md` |
| Arbitrary spacing | `p-[13px]` or `padding: 13` | Nearest scale value |
| Arbitrary duration | `duration-[200ms]` | `duration-cv-fast` |
| Arbitrary easing | `ease-[cubic-bezier(...)]` | `ease-cv-default` |
| React Native inline color | `style={{backgroundColor: '#fff'}}` | NativeWind class `bg-cv-surface` |

**Exceptions:** Primitive definition files, `/* cv-override */` comments, `/* cv-exception: reason */` comments, SVG/image data.

### theme-guard

Catches patterns that break in one theme.

| Violation | Example | Fix |
|-----------|---------|-----|
| Light-only background | `bg-white` hardcoded | `bg-cv-surface` |
| Missing dark adaptation | `bg-gray-50` without dark variant | `bg-cv-bg-primary` |
| Shadow invisible in dark | Raw shadow rgba | `shadow-cv-sm` |
| Low contrast in one theme | Text passes in light, fails in dark | Reference contrast-matrix |
| StatusBar mismatch (mobile) | Light status bar on light background | Theme-aware StatusBar component |
| Hard-to-read on OLED (mobile) | Pure black (#000) backgrounds | `cv-navy-950` (slightly warm black) |

### a11y-guard

Catches accessibility violations.

| Violation | Example | Fix |
|-----------|---------|-----|
| Missing input label | `<TextInput>` without label | Add label component above |
| Image without alt | `<Image>` without `alt`/`accessibilityLabel` | Add descriptive label |
| Button without name | Icon-only button without label | Add `aria-label` / `accessibilityLabel` |
| Touch target < 44px | `w-6 h-6` clickable (24px) | Min `w-11 h-11` (44px) |
| Color-only meaning | Red text as sole error indicator | Add icon + text |
| Missing focus-visible (web) | Custom component no focus ring | Add `focus-visible:ring-2` |
| Missing keyboard nav (web) | Custom dropdown no arrow keys | Follow WAI-ARIA pattern |
| Missing accessible role (mobile) | Touchable without `accessibilityRole` | Add `accessibilityRole="button"` |
| Missing reduced-motion | Animation without motion check | `motion-safe:` (web), `useReducedMotion()` (mobile) |
| Heading hierarchy skip | `h1` then `h3` | Sequential levels |
| Missing live region | Dynamic content without announcement | `aria-live` (web), `accessibilityLiveRegion` (mobile) |

### pattern-guard

Catches convention violations.

| Violation | Example | Fix |
|-----------|---------|-----|
| Multiple primary buttons | Two primary buttons in view | One primary per screen |
| Floating labels | Placeholder as label | Label above, always visible |
| Technical jargon in UI | "Deploy agent" | "Go live" (check terminology table) |
| Raw error message | `{error.message}` rendered | Solution-focused copy |
| Decorative animation | `animate-bounce` | Motion serves comprehension |
| Non-catalog component | Custom `<Popup>` | Use Popover or Modal from catalog |
| Missing empty state | List with no empty handling | Add empty state pattern |
| Modal on mobile | Centered overlay modal on phone | Use bottom sheet (Drawer) instead |
| No haptic on key action (mobile) | Primary button without haptic feedback | Add light impact haptic |
| Ignoring safe areas (mobile) | Content behind notch/home indicator | Use SafeAreaView |

### Enforcement Flow

```
AI agent receives implementation task
  → Foundations load (theme-system, layout, motion, etc.)
  → Agent identifies target platform (web, iOS, Android, or shared)
  → Agent identifies needed components via catalog
  → Component guidelines load (with platform-specific sections)
  → Relevant patterns load (loading, feedback, etc.)
  → Agent writes code
  → Enforcement guards check on write/edit:
      token-guard  → scans for hardcoded values
      theme-guard  → scans for theme-breaking patterns
      a11y-guard   → scans for accessibility gaps
      pattern-guard → scans for convention violations
  → Violations self-corrected before saving
  → Code is clean on first write
```

---

## Part 6: Plugin File Structure

```
chatverce-design/
  .claude-plugin/
    plugin.json                              # v2.0.0 manifest

  skills/
    # ─── PHILOSOPHY ───
    philosophy/
      design-ethos/
        SKILL.md                             # Caring, simplicity, breathing room, focus, emotion, thoroughness
        ive-philosophy.md                    # Deep Ive reference (kept from v1)

    # ─── FOUNDATIONS (always-on for Chatverce repos) ───
    foundations/
      theme-system/
        SKILL.md                             # 3-layer architecture, token rule
        primitives.md                        # Full color scales
        semantic-light.md                    # Light theme mappings
        semantic-dark.md                     # Dark theme mappings
        tokens-tailwind.md                   # Shared Tailwind config (web + NativeWind)
        tokens-css.md                        # CSS custom properties (web-only)
        tokens-rn.md                         # React Native constants (fallback)
      color/
        SKILL.md                             # Color principles, usage, contrast rules
        contrast-matrix.md                   # Verified ratios for all token pairs
      typography/
        SKILL.md                             # Scale, hierarchy, dynamic sizing, platform rules
      layout/
        SKILL.md                             # Breakpoints, z-index, grid, spacing, safe areas
      motion/
        SKILL.md                             # Duration, easing, reduced-motion, haptics
      materials-and-depth/
        SKILL.md                             # Elevation, layering, translucency, platform shadows
      iconography/
        SKILL.md                             # Lucide, sizing, stroke, channel icons
      accessibility/
        SKILL.md                             # Full a11y system (web + mobile)
      privacy/
        SKILL.md                             # Data UI, permissions, transparency
      writing/
        SKILL.md                             # Voice, tone, terminology, copy mechanics

    # ─── PATTERNS (activated by context) ───
    patterns/
      loading/SKILL.md
      onboarding/SKILL.md
      feedback/SKILL.md
      searching/SKILL.md
      settings/SKILL.md
      modality/SKILL.md
      entering-data/SKILL.md
      managing-accounts/SKILL.md
      managing-notifications/SKILL.md
      undo-and-recovery/SKILL.md
      ai-interactions/SKILL.md
      real-time-communication/SKILL.md
      channel-switching/SKILL.md
      going-live/SKILL.md

    # ─── COMPONENTS (activated by file path — UI files only) ───
    components/
      catalog.md
      accordion/SKILL.md
      alert/SKILL.md
      ... (71 component.gallery components)
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
    design-reviewer.md                       # Enhanced — knows all layers

  commands/
    design-review.md                         # Enhanced audit command
```

### Activation Model

| Layer | Activates When | Trigger |
|-------|---------------|---------|
| Philosophy | Always loaded for Chatverce repos | Foundational context |
| Foundations | Chatverce project detected | `package.json` name, repo name, `.chatverce` marker |
| Patterns | Working on feature implementation | Feature-related files, planning context |
| Components | Working on UI files | `*.tsx`, `*.css`, `components/**`, `screens/**`, `app/**/page.tsx` |
| Enforcement | Writing/editing code | Edit/Write tool usage on UI files |

---

## Part 7: Transition from v1.0

### Kept As-Is

| File | Destination |
|------|-------------|
| `ive-philosophy.md` | `philosophy/design-ethos/ive-philosophy.md` |

### Refined

| Current | Destination | Changes |
|---------|-------------|---------|
| `design-soul/SKILL.md` | `philosophy/design-ethos/SKILL.md` | Expanded with caring, breathing room, emotional intention, focus discipline. Enriched with Ive article insights. |
| `voice-and-tone/SKILL.md` | `foundations/writing/SKILL.md` | Split into voice & tone, terminology, copy mechanics. Single source of truth for terminology. |

### Replaced

| Current | Absorbed Into |
|---------|--------------|
| `brand-identity/SKILL.md` | `foundations/theme-system/` + `foundations/color/` + `foundations/iconography/` + `foundations/layout/` |
| `brand-identity/tokens.md` | `foundations/theme-system/tokens-*.md` (rebuilt for NativeWind sharing) |
| `component-patterns/SKILL.md` | Individual component skills + `foundations/motion/` + `foundations/writing/` + `patterns/*` |
| `component-patterns/examples.md` | Embedded in each component SKILL.md (web + mobile examples) |

### Updated

| File | Changes |
|------|---------|
| `.claude-plugin/plugin.json` | Version `2.0.0`, description updated, activation rules expanded |
| `.claude-plugin/marketplace.json` (repo root) | Version bumped to match |

### New

| Addition | Count | Purpose |
|----------|-------|---------|
| Philosophy layer | 2 files | Design ethos + Ive reference |
| Foundation skills | 10 skills | Theme, color, typography, layout, motion, materials, icons, a11y, privacy, writing |
| Pattern skills | 14 skills | Behavioral guidance layer |
| Component skills | 77 skills | Individual component specs with platform considerations |
| Enforcement skills | 4 skills | Guards for implementation |

### Metrics

| | v1.0 | v2.0 |
|-|------|------|
| Philosophy | Implicit (in design-soul) | Explicit layer (2 files) |
| Foundations | 4 skills | 10 skills |
| Patterns | 0 | 14 skills |
| Component specs | 12 (1 file) | 77 (individual, with platform sections) |
| Enforcement | 0 | 4 skills |
| Token layers | 1 (flat) | 3 (primitive → semantic → component) |
| Themes | Light only | Light + Dark |
| Platforms | Web-first | Web + iOS + Android (co-equal) |
| Token sharing | Separate formats | Unified via NativeWind/Tailwind |
| Accessibility | Mentioned | Full system (web + mobile) |
| AI-specific patterns | 0 | Dedicated AI Interactions pattern |
| Total files | 12 | ~115 |
| Agent compatibility | Claude Code only | Agent-agnostic content, Claude Code delivery |
