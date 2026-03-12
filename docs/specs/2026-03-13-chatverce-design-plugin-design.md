# Chatverce Design Plugin — Specification

**Date**: 2026-03-13
**Status**: Approved
**Scope**: Claude Code plugin for Chatverce brand identity & design system

---

## 1. Overview

### What We're Building

A Claude Code plugin called `chatverce-design` that embeds Jony Ive's design philosophy as the operating system for all design decisions in the Chatverce product. The plugin lives in a new GitHub repo `org-siharilabs/plugins` that will house all future Sihari Labs plugins.

### Why

Chatverce is rebranding from a WhatsApp management tool to a **no-code AI Agent builder platform** ("Put your business on autopilot"). The current identity is fragmented — green WhatsApp-focused colors on web, teal on mobile, inconsistent typography, and messaging-tool branding that doesn't match the AI product positioning.

The plugin ensures every developer (human or AI) working on Chatverce produces UI that is:
- Aligned with the new brand identity (navy + teal, not WhatsApp green)
- Following Jony Ive's design principles (simplicity, care, deference, quiet confidence)
- Accessible to non-technical business owners (the target user)
- Consistent across web and mobile platforms

### Target Users

- Claude Code (the primary consumer — auto-loads skills when working on UI code)
- Developers on the Chatverce team (via `/chatverce-design:design-review`)

---

## 2. Repository Structure

```
org-siharilabs/plugins/              # GitHub repo
├── README.md                        # Marketplace overview
├── marketplace.json                 # Plugin marketplace manifest
├── chatverce-design/                # First plugin
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── skills/                      # Agent-invoked skills (Claude loads automatically)
│   │   ├── design-soul/
│   │   │   ├── SKILL.md             # Core philosophy
│   │   │   └── ive-philosophy.md    # Deep reference (loaded on demand)
│   │   ├── brand-identity/
│   │   │   ├── SKILL.md             # Colors, type, spacing
│   │   │   └── tokens.md            # Implementation-ready tokens
│   │   ├── component-patterns/
│   │   │   ├── SKILL.md             # UI patterns & rules
│   │   │   └── examples.md          # Code snippets
│   │   └── voice-and-tone/
│   │       └── SKILL.md             # Copy & terminology
│   ├── commands/                    # User-invoked slash commands
│   │   └── design-review.md         # /chatverce-design:design-review
│   ├── agents/
│   │   └── design-reviewer.md       # Specialized review subagent
│   └── README.md                    # Plugin documentation
└── (future plugins)
```

### Marketplace Manifest

The repo root contains `marketplace.json` for plugin discoverability:

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

To install, teams first add the marketplace then install the plugin:

```bash
claude plugin marketplace add --url https://github.com/org-siharilabs/plugins
claude plugin install chatverce-design@org-siharilabs-plugins
```

For local development during implementation:

```bash
claude --plugin-dir ./plugins/chatverce-design
```

---

## 3. Plugin Manifest

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

---

## 4. How Skills Are Invoked

The four `skills/` entries are **agent-invocable**: Claude loads them automatically when it determines they are relevant, based on each skill's `description` field. This is NOT guaranteed to happen every time — Claude decides based on task context. The `description` must be precise enough to trigger on UI/frontend work.

Each skill uses this frontmatter pattern:

```yaml
---
name: <skill-name>
description: <precise trigger description — see each section below>
user-invocable: false
---
```

`user-invocable: false` hides them from the `/` menu (they're background knowledge, not actions). The descriptions are loaded into Claude's context so it knows what's available, but full skill content only loads when invoked.

**Important**: There is no mechanism to force all four skills to load simultaneously. Claude may load one, some, or all depending on the task. Each skill must be self-contained enough to be useful on its own, while cross-referencing the others via markdown links for when deeper guidance is needed.

The design-review command lives in `commands/` (not `skills/`) because it is user-invoked only and follows the plugin ecosystem convention.

---

## 5. Skill: Design Soul

**File**: `skills/design-soul/SKILL.md`
**Type**: Agent-invoked (Claude loads when relevant)

### Frontmatter

```yaml
---
name: design-soul
description: Chatverce design philosophy and principles. Use when designing UI components, creating screens, building user interfaces, making layout decisions, choosing visual approaches, or writing user-facing features for the Chatverce product. Provides the core design ethos inspired by Jony Ive.
user-invocable: false
---
```

### Content

#### The Chatverce Design Ethos

Five core principles derived from Jony Ive, adapted for an AI product targeting non-technical users:

**1. Inevitable Simplicity**
"The solution should seem so obvious you forget it was designed." Every screen should feel like the only way it could have been. If a user has to think about where to click, the design has failed.

Ive: "So much of what we try to do is get to a point where the solution seems inevitable: you know, you think 'of course it's that way, why would it be any other way?'"

**2. Care Is Visible**
Users sense the difference between thoughtful and thoughtless. Every pixel, every transition, every word should testify that someone cared. This is not about polish for polish's sake — it's about respect for the person using the product.

Ive: "What we make testifies who we are. People can sense care and can sense carelessness."

**3. Deference to the User's Goal**
The UI recedes. The user's assistant, their conversation, their data is the hero — never the chrome. Interfaces should feel like direct manipulation of information, not operating a machine.

Ive (iOS 7): "An interface that is unobtrusive and deferential, one where the design recedes and in doing so actually elevates your content."

**4. Complexity Absorbed, Not Transferred**
Chatverce does the hard work so the user doesn't have to. AI agent building is complex — the interface must never be. We solve complicated problems without letting people know how complicated the problem was.

Ive: "We try to solve very complicated problems without letting people know how complicated the problem was."

**5. Quiet Confidence**
Premium doesn't shout. No gradients-for-the-sake-of-gradients, no excessive animation, no "look how smart this is." The product speaks through its craft. True luxury does not need to announce itself.

Ive: "There is a profound and enduring beauty in simplicity; in clarity, in efficiency."

#### Anti-Patterns — What to Never Do

- Never use technical jargon in UI copy ("Deploy agent" -> "Go live")
- Never show raw error messages, stack traces, or error codes to users
- Never add a feature without removing complexity elsewhere
- Never use decoration that doesn't serve comprehension
- Never sacrifice usability for aesthetics ("A beautiful product that doesn't work very well is ugly")
- Never add a button, field, or option that serves less than 80% of users — hide it in advanced settings
- Never use placeholder text as the only label
- Never use color alone to communicate meaning (accessibility)

#### The Autopilot Promise

Every design decision should reinforce the tagline "Put your business on autopilot." If a feature doesn't make the user feel like things are running themselves, question why it exists. The entire experience should communicate: set it up once, it handles the rest.

#### The Ive Test — Apply to Every Screen

Before shipping any screen, component, or flow:
1. Can anything be removed without losing function?
2. Does the UI defer to the user's content and goal?
3. Would a non-technical business owner understand it in under 3 seconds?
4. Does it feel inevitable — like the only way it could have been?
5. Does it reinforce the "autopilot" promise?

If any answer is no, iterate.

### Supporting File: `ive-philosophy.md`

Referenced from SKILL.md via: `For the complete philosophy with quotes and case studies, see [ive-philosophy.md](ive-philosophy.md)`

Deep reference material (loaded on demand) containing:

- Jony Ive's complete design philosophy with key quotes organized by principle
- Dieter Rams' 10 Principles of Good Design mapped to Chatverce decisions
- Case studies translating Apple design decisions to SaaS UI:
  - iPod "one button" -> Agent builder "one primary CTA per screen"
  - iOS 7 deference -> Dashboard that foregrounds user data over chrome
  - iPhone eliminating the on-off button -> Removing unnecessary confirmation steps
  - Apple Watch personalization -> Letting users customize their assistant's personality
- Ive's evolved philosophy (2024-2026): minimalism must have soul, not just reduction
- The "fragility of ideas" — why early design concepts need protection from premature criticism

---

## 6. Skill: Brand Identity

**File**: `skills/brand-identity/SKILL.md`
**Type**: Agent-invoked (Claude loads when relevant)

### Frontmatter

```yaml
---
name: brand-identity
description: Chatverce brand identity tokens — colors, typography, spacing, shadows, border radius, and iconography. Use when writing CSS, Tailwind classes, styling components, choosing colors, setting fonts, or implementing any visual design for Chatverce.
user-invocable: false
---
```

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

### Supporting File: `tokens.md`

Referenced from SKILL.md via: `For implementation-ready code, see [tokens.md](tokens.md)`

Implementation-ready design tokens in four formats:

**1. CSS Custom Properties** (for `web/app/globals.css`):
Token naming convention: `--cv-{category}-{name}` (e.g., `--cv-color-primary`, `--cv-color-accent`, `--cv-radius-md`)

**2. Tailwind Config Extension** (for `web/tailwind.config.js`):
Extends under `colors.cv` namespace (e.g., `cv-primary`, `cv-accent`, `cv-snow`) to avoid conflicts with default Tailwind palette. Usage: `bg-cv-primary`, `text-cv-accent`, `border-cv-frost`.

**3. TypeScript Constants** (for `mobile/constants/design-system.ts`):
Direct replacement for the existing design-system.ts file, maintaining the same export structure.

**4. MUI Theme Override** (for web MUI v7 integration):
The Chatverce web app uses both shadcn/Tailwind AND MUI v7. The tokens file includes a `createTheme()` override that maps Chatverce tokens to MUI's palette, typography, and shape. New components should use shadcn + Tailwind tokens. Existing MUI components should be migrated gradually — the theme override ensures they stay visually consistent during migration.

**Component library strategy**: New components use shadcn + Tailwind. Existing MUI components continue working via the MUI theme override. No rush to migrate — consistency is maintained by both systems reading from the same token values.

---

## 7. Skill: Component Patterns

**File**: `skills/component-patterns/SKILL.md`
**Type**: Agent-invoked (Claude loads when relevant)

### Frontmatter

```yaml
---
name: component-patterns
description: Chatverce UI component patterns — buttons, cards, forms, navigation, modals, tables, empty states, the agent builder canvas, and motion/animation rules. Use when building React components, creating pages, implementing UI interactions, or working on the agent builder interface for Chatverce.
user-invocable: false
---
```

### Component Checklist

Before building any component, apply the Ive Test (defined in full in the design-soul skill). Quick reference:
1. Can anything be removed? 2. Does it defer to content? 3. Instantly understandable? 4. Feels inevitable? 5. Reinforces "autopilot"?

### Buttons

- **Primary**: Midnight Navy background, white text. One primary action per screen.
- **Secondary**: Teal outline, teal text. Supporting actions.
- **Ghost**: Text only with subtle hover. Tertiary/cancel actions.
- No gradients. No shadows on buttons. Color alone communicates hierarchy.
- Minimum touch target: `44px` (Apple HIG standard)
- Loading state: subtle pulse animation, never a spinner replacing the label
- Destructive actions: `Coral` background, require confirmation for irreversible operations

### Cards

- White surface, single `sm` shadow, `8px` radius
- Content-first: the card's information is the design, not the card itself
- No colored borders, no header stripes. Differentiate with a small color dot or icon.
- Generous internal padding: `20px` minimum
- On hover: elevate shadow to `md` with `150ms ease-out`

### Forms & Inputs

- Single-column layouts only. Never side-by-side inputs for non-technical users.
- Labels always above inputs, never floating/inside
- `8px` radius, `Frost` border, `14px` text
- Focus state: `Teal` border with `AI Glow` ring
- Error messages appear below the input in `Coral`, never as toasts for form errors
- Progressive disclosure: show only what's needed now, reveal complexity as the user advances
- Group related fields visually with subtle section dividers

### Navigation

- Sidebar: clean, icon + label, single level. No nested menus.
- Active state: `Teal` text + subtle `Teal 10%` background pill
- Breadcrumbs for depth, never deep dropdown trees
- Mobile: bottom tab bar, max 5 items
- Current location should always be obvious without reading — visual state alone

### Empty States

- Never a blank screen. Every empty state is an invitation.
- Simple line-art illustration + clear headline + single CTA
- Example: "No agents yet" -> headline: "Create your first assistant" + prominent button
- Tone: encouraging, never apologetic ("No data found" is wrong)

### Tables & Lists

- Clean, minimal borders — use alternating subtle backgrounds or spacing instead of grid lines
- Row hover: subtle `Cloud` background
- Sortable columns indicated by small arrow, not heavy UI
- Pagination: simple "Previous / Next" for non-technical users, not page numbers
- Always show a meaningful empty state when no data

### Modals & Dialogs

- White surface, `lg` shadow, `12px` radius
- Overlay: `#1A1D23` at 50% opacity
- One clear action per modal. If it needs more, it should be a page.
- Close: X button in top-right, click overlay to dismiss, Escape key
- Enter animation: fade + scale from 95% to 100% over `300ms ease-out`

### The Agent Builder (Core Product Surface)

- Visual, node-based canvas with drag-and-drop
- Each node: white card, small colored left-border indicating type:
  - Trigger ("When this happens"): Teal
  - Action ("Do this"): Navy
  - Condition ("Check if"): Amber
  - Response: Meadow
- Connections: smooth bezier curves in `Slate`, animated flow dots in `Teal` when active
- Properties panel: slides from right, same card aesthetic
- Canvas background: subtle dot grid on `Snow` background
- Zoom controls: minimal, bottom-right corner

### Terminology Mapping (Technical -> User-Facing)

| Technical | Chatverce says | Why |
|-----------|---------------|-----|
| Deploy | Go live | Universal understanding |
| Agent | Assistant | Friendly, not espionage |
| Node | Step | Sequential, simple |
| Trigger | "When this happens" | Natural language |
| Action | "Do this" | Natural language |
| Conditional/Branch | "Check if" | Natural language |
| Webhook | Connection | Simple |
| API key | Secret key | Familiar concept |
| Workflow | Flow | Shorter |
| NLP/Intent | "Understands" | What, not how |
| Fallback | "If unsure, do this" | Descriptive |
| Variable | Field | Common |
| Endpoint | — | Never show to user |
| Latency | Speed / response time | Human language |
| Uptime | Availability | Or just "always on" |

### Motion & Animation

Ive's rule: motion serves comprehension, never decoration.

- Micro-interactions: `150ms ease-out`
- Panel/page transitions: `300ms ease-out`
- Enter: fade + slight upward translate (`8px`). Exit: fade only.
- AI-working indicator: gentle teal pulse (the only allowed "decorative" motion)
- Never: bouncing, spinning, confetti, particle effects, parallax scrolling
- Reduced motion: respect `prefers-reduced-motion` — disable all non-essential animation

### Supporting File: `examples.md`

Referenced from SKILL.md via: `For implementation code snippets, see [examples.md](examples.md)`

Concrete code snippets for each pattern using shadcn + Tailwind with Chatverce tokens:
- Button variants (`bg-cv-primary`, `border-cv-accent`, ghost)
- Card component with proper token usage
- Form input with focus states (`ring-cv-accent/10`)
- Navigation sidebar active state
- Empty state component
- Modal with correct animation
- Agent builder node card

---

## 8. Skill: Voice & Tone

**File**: `skills/voice-and-tone/SKILL.md`
**Type**: Agent-invoked (Claude loads when relevant)

### Frontmatter

```yaml
---
name: voice-and-tone
description: Chatverce voice, tone, and terminology guidelines. Use when writing user-facing text, button labels, error messages, empty states, onboarding copy, tooltips, notifications, or any UI strings for Chatverce. Also use when reviewing existing copy.
user-invocable: false
---
```

### The Voice Principle

Chatverce speaks like a calm, competent friend who happens to be brilliant at AI — but never makes you feel stupid. Think: the best Apple Store employee. They don't explain the chip architecture. They say "This one's faster."

### Voice Attributes

| Attribute | What it means | Do | Don't |
|-----------|---------------|-----|-------|
| Clear | No jargon, no ambiguity | "Your assistant is live" | "Agent deployed to production endpoint" |
| Confident | Declarative, not hedging | "This will reply automatically" | "This should help with automating replies" |
| Warm | Human, encouraging | "Nice — your first assistant is ready" | "Agent creation successful" |
| Brief | Fewer words, then cut again | "Replies in 2 seconds" | "Average response latency is approximately 2 seconds" |

### Writing Rules

1. **Lead with the outcome, not the mechanism.** "Customers get instant replies" not "The AI processes incoming messages and generates contextual responses"
2. **Use "you" and "your."** "Your assistant" not "The agent." It's theirs.
3. **Present tense.** "Your assistant replies to customers" not "Your assistant will reply to customers"
4. **No exclamation marks in UI.** Exception: first-time milestones ("Your first assistant is live!")
5. **Error messages are solutions.** "Check your WhatsApp connection in Settings" not "Error: Channel disconnected (ERR_WA_401)"
6. **Numbers over adjectives.** "Handled 1,247 conversations" not "Handled many conversations"
7. **Never blame the user.** "That didn't work — try again" not "Invalid input"

### Tone by Context

| Context | Tone | Example |
|---------|------|---------|
| Onboarding | Encouraging, guiding | "Let's set up your first assistant — takes about 3 minutes" |
| Success | Warm, understated | "All set. Your assistant is handling conversations now." |
| Error | Calm, solution-focused | "Something went wrong connecting to WhatsApp. Reconnect in Settings." |
| Empty state | Inviting, forward-looking | "No conversations yet. They'll show up here once your assistant is live." |
| Destructive action | Clear, no drama | "This will permanently delete the assistant and its conversation history." |
| Loading/waiting | Transparent | "Setting things up..." not "Please wait while we process your request" |
| Dashboard stats | Factual, proud | "Your assistants handled 3,421 conversations this week" |

### Button & Action Labels

| Pattern | Do | Don't |
|---------|-----|-------|
| Primary CTA | "Create assistant" | "Submit" / "Create new AI agent" |
| Save | "Save changes" | "Update" / "Apply configuration" |
| Cancel | "Cancel" | "Discard" / "Abort" |
| Delete | "Delete assistant" | "Remove" / "Destroy" |
| Connect | "Connect WhatsApp" | "Configure WhatsApp channel integration" |
| Go live | "Go live" | "Deploy to production" |
| Navigation | "Assistants" | "Agent management" |

---

## 9. Command: Design Review

**File**: `commands/design-review.md` (in `commands/` directory, not `skills/`, per plugin convention for user-invoked-only items)
**Type**: User-invoked only

### Frontmatter

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

### Command Body (Markdown Instructions)

The command body is the task prompt sent to the `design-reviewer` agent. It uses `$ARGUMENTS` to receive the target:

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

### Trigger

```
/chatverce-design:design-review src/components/inbox
/chatverce-design:design-review ThreadCard
/chatverce-design:design-review app/(private)/dashboard
```

---

## 10. Agent: Design Reviewer

**File**: `agents/design-reviewer.md`

### Frontmatter

```yaml
---
name: design-reviewer
description: Reviews UI code against Chatverce design system — brand identity, Ive-inspired design principles, component patterns, voice & tone, and accessibility. Use when reviewing frontend code, PRs with UI changes, or auditing existing screens.
tools: Read, Grep, Glob, Bash(find *)
model: sonnet
---
```

### Agent Body

The markdown body below the frontmatter contains the agent's system prompt. It inlines the essential design rules so the agent has full context without depending on skill auto-loading:

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

### How It's Used

The design-reviewer agent is invoked by the `design-review` command via `agent: design-reviewer` in its frontmatter. When a user runs `/chatverce-design:design-review`, the command's markdown body becomes the task prompt, and the agent provides the system context and tool configuration.

---

## 11. Implementation Notes

### Installation

Installation happens in two steps (see Section 2 for marketplace manifest):

```bash
# 1. Add the marketplace (one-time setup)
claude plugin marketplace add --url https://github.com/org-siharilabs/plugins

# 2. Install the plugin
claude plugin install chatverce-design@org-siharilabs-plugins --scope project
```

Using `--scope project` writes to `.claude/settings.json` so the plugin is shared with the whole team via version control.

For local development during plugin creation:

```bash
claude --plugin-dir ./plugins/chatverce-design
```

### Token Integration

The `tokens.md` file provides design tokens in four formats (see Section 6 for details):
1. CSS custom properties (`--cv-color-primary`, etc.) for `web/app/globals.css`
2. Tailwind config extension (`cv-primary`, etc.) for `web/tailwind.config.js`
3. TypeScript constants for `mobile/constants/design-system.ts`
4. MUI theme override for existing MUI v7 components

### Migration Path

The existing design system files that will eventually be updated:
- `web/app/globals.css` — CSS variables (replace existing `--color-primary` etc.)
- `web/tailwind.config.js` — Tailwind theme (add `cv-` namespace)
- `web/lib/themeConfig.ts` — MUI theme (map to Chatverce tokens)
- `mobile/constants/design-system.ts` — Mobile tokens (replace entirely)
- `mobile/constants/theme.ts` — Mobile theme

The plugin itself doesn't modify these files — it provides the source of truth that developers and Claude reference when making changes.

### Plugin Conflicts

The official `frontend-design` plugin (if installed) advises against Inter as a font choice. When working on Chatverce specifically, the `chatverce-design` plugin takes precedence — Inter is the deliberate brand choice. If both plugins are active, Claude should follow `chatverce-design` for Chatverce projects and `frontend-design` for other projects. This is handled naturally by skill descriptions mentioning "Chatverce" explicitly.

### What's NOT in Scope

- Actually migrating the existing codebase to the new brand (separate project)
- Dark mode implementation (token system accommodates it, but not v1)
- Landing page redesign (separate project, but will use this design system)
- Logo redesign (keeping the existing "C" mark)
- Migrating MUI components to shadcn (gradual, not part of this plugin)

---

## 12. Validation & Testing

Before publishing v1.0.0:

1. **Structure validation**: `claude plugin validate ./chatverce-design` must pass
2. **Skill loading**: Start Claude with `--plugin-dir`, ask "What skills are available?" — all four skills should appear in Claude's knowledge
3. **Skill triggering**: Ask Claude to "build a card component for Chatverce" — it should automatically invoke `brand-identity` and/or `component-patterns`
4. **Command invocation**: `/chatverce-design:design-review web/components/inbox` should produce a structured report
5. **Agent delegation**: The design-review command should correctly delegate to the `design-reviewer` agent
6. **Token accuracy**: Verify all hex values, font names, and spacing values in `tokens.md` match this spec exactly
7. **Cross-reference**: Ensure no contradictions between the four skills (e.g., color names, terminology)

---

## 13. Version Strategy

The plugin follows semantic versioning:

- **1.0.0**: Initial release with all five components (4 skills + 1 command + 1 agent)
- **1.x.0**: Add new skills (e.g., `dark-mode`, `responsive-patterns`, `illustration-style`)
- **1.0.x**: Fix token values, improve descriptions, clarify rules
- **2.0.0**: Breaking changes (e.g., rename token variables, restructure skills)

Update `version` in `plugin.json` on every change — Claude Code uses this to determine whether to update cached plugins.

---

## 14. Success Criteria

The plugin is successful when:

1. Claude automatically produces UI code that follows the brand identity without being told
2. `/chatverce-design:design-review` catches deviations with specific, actionable feedback
3. New screens built with the plugin look and feel consistent with each other
4. A non-technical business owner can use any screen built under this system without confusion
5. The visual identity says "AI platform" not "WhatsApp tool"
