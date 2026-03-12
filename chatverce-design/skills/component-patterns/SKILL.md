---
name: component-patterns
description: Chatverce UI component patterns — buttons, cards, forms, navigation, modals, tables, empty states, the agent builder canvas, and motion/animation rules. Use when building React components, creating pages, implementing UI interactions, or working on the agent builder interface for Chatverce.
user-invocable: false
---

### Component Checklist

Before building any component, apply the Ive Test (defined in full in the design-soul skill). Quick reference:
1. Can anything be removed? 2. Does it defer to content? 3. Instantly understandable? 4. Feels inevitable? 5. Reinforces "autopilot"?

### Buttons

- **Primary**: Midnight Navy background, white text. One primary action per screen.
- **Secondary**: Teal outline, teal text. Supporting actions.
- **Ghost**: Text only with subtle hover. Tertiary/cancel actions.
- **Destructive**: Coral background, white text. Require confirmation for irreversible operations.
- No gradients. No shadows on buttons. Color alone communicates hierarchy.
- Minimum touch target: `44px` (Apple HIG standard)
- Loading state: subtle pulse animation, never a spinner replacing the label

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
- Example: "No agents yet" → headline: "Create your first assistant" + prominent button
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

### Terminology Mapping (Technical → User-Facing)

| Technical | Chatverce Says | Why |
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

Cross-reference: `For implementation code snippets, see [examples.md](examples.md)`
