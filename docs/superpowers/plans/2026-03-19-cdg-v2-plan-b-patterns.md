# CDG v2.0 — Plan B: Patterns Layer

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Write all 14 behavioral pattern skills that describe HOW users accomplish tasks in Chatverce — the layer between foundations and components.

**Architecture:** Each pattern follows the pattern skill template from the spec. Patterns reference foundation tokens and list which catalog components participate. They are activated by context (when an agent is implementing a feature that involves the pattern).

**Tech Stack:** Markdown (SKILL.md files)

**Spec:** `docs/specs/2026-03-19-chatverce-design-plugin-v2-design.md` — Part 3: Patterns (lines 610–730)

**Depends on:** Plan A (foundations must exist for cross-references)

**Working directory:** `/Users/sandeep/Desktop/dev/org-siharilabs/plugins`

---

## Pattern Skill Template

Every pattern SKILL.md follows this structure:

```markdown
---
name: {pattern-name}
description: {one-liner describing the behavioral pattern}
user_invocable: false
---

# {Pattern Name}

## What the User Is Doing
{Behavioral description — the task, not the UI}

## Emotional Intention
{What the user should feel during this pattern — from philosophy/design-ethos}

## How It Works
{Step-by-step flow with decision points, branching logic}

## Components Used
{Which catalog components participate, with cross-reference paths}

## Platform Considerations
### Web (Next.js + Tailwind + shadcn/ui)
### iOS (Expo + NativeWind)
### Android (Expo + NativeWind)

## Do / Don't
| Do | Don't |
|----|-------|

## Chatverce-Specific
{How this pattern applies to Chatverce's product domain — assistants, conversations, channels}
```

---

## File Map

All files created in `chatverce-design/skills/patterns/`:

```
patterns/
  loading/SKILL.md                    # Task 1
  onboarding/SKILL.md                # Task 2
  feedback/SKILL.md                   # Task 3
  searching/SKILL.md                  # Task 4
  settings/SKILL.md                   # Task 5
  modality/SKILL.md                   # Task 6
  entering-data/SKILL.md             # Task 7
  managing-accounts/SKILL.md         # Task 8
  managing-notifications/SKILL.md    # Task 9
  undo-and-recovery/SKILL.md         # Task 10
  ai-interactions/SKILL.md           # Task 11 (expanded — Chatverce-critical)
  real-time-communication/SKILL.md   # Task 12
  channel-switching/SKILL.md         # Task 13
  going-live/SKILL.md                # Task 14
```

---

## Tasks

### Task 1: Loading Pattern

**Files:** Create `chatverce-design/skills/patterns/loading/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **What the user is doing:** Waiting for content, data, or AI processing to complete.
- **Emotional intention:** Transparency — "I know what's happening and it won't take long."
- **How it works:**
  - < 300ms: No loading indicator (perceived as instant)
  - 300ms–2s: Skeleton loader replacing content areas
  - 2s–10s: Skeleton + descriptive text ("Loading conversations...")
  - 10s+: Progress bar with steps or percentage
  - AI processing: Teal pulse + contextual label ("Thinking...", "Setting things up...")
- **Components:** Skeleton, Spinner, Progress bar, AI working indicator
- **Do:** Use skeleton shapes that match the content layout. Show optimistic UI where possible (e.g., message appears immediately, sending indicator in background).
- **Don't:** Use generic spinners for content that has a known layout. Never block the entire screen for a single loading item.
- **Platform:** Web: CSS skeleton animations. Mobile: `react-native-reanimated` for smooth skeleton shimmer. Both: `motion-safe:` / `useReducedMotion()`.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/loading/SKILL.md
git commit -m "feat(cdg): add loading pattern — skeleton, optimistic UI, AI processing states"
```

---

### Task 2: Onboarding Pattern

**Files:** Create `chatverce-design/skills/patterns/onboarding/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **Emotional intention:** Guided ease — "This will only take a minute."
- **How it works:**
  - First-run: 3-step wizard (Connect channel → Create assistant → Go live)
  - Progressive disclosure: Show complexity only when user reaches it
  - Empty states as onboarding: Every empty screen is an invitation to start
  - Tooltips/spotlights: Point out key features on first encounter, then never again
- **Components:** Progress indicator, Modal, Form, Empty state, Hero, Tooltip
- **Chatverce-specific:** "Let's set up your first assistant — takes about 3 minutes." The Autopilot Promise starts here.
- **Platform:** Web: Full wizard with sidebar progress. Mobile: Full-screen pages with swipe navigation.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/onboarding/SKILL.md
git commit -m "feat(cdg): add onboarding pattern — first-run wizard, progressive disclosure"
```

---

### Task 3: Feedback Pattern

**Files:** Create `chatverce-design/skills/patterns/feedback/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **Emotional intention:** Confidence — "I know the result of my action."
- **Decision tree:**
  - Success (transient, non-critical): Toast — auto-dismiss 4s
  - Success (important milestone): Toast with celebration copy — "Nice — your first assistant is ready"
  - Error (recoverable): Toast with action — "Couldn't save. [Try again]"
  - Error (blocking): Inline Alert — stays until resolved
  - Warning (needs attention): Alert (warning variant) — persistent
  - Info (system update): Alert (info variant) or Banner — dismissible
  - Destructive confirmation: Modal — requires explicit action
- **Components:** Toast, Alert, Badge, Modal
- **Do:** Match urgency to placement (toast = low urgency, inline alert = medium, modal = high).
- **Don't:** Use toasts for errors that require action. Never stack more than 2 toasts.
- **Platform:** Web: Toast top-right or bottom-center. iOS: Toast below Dynamic Island. Android: Toast bottom above nav. System alerts via `Alert.alert()` on mobile.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/feedback/SKILL.md
git commit -m "feat(cdg): add feedback pattern — toast/alert decision tree, urgency hierarchy"
```

---

### Task 4: Searching Pattern

**Files:** Create `chatverce-design/skills/patterns/searching/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **Emotional intention:** Efficient flow — "I can find anything fast."
- **How it works:** Search input → debounced query (300ms) → results list → empty results with suggestion
- **Components:** Search input, List, Empty state, Badge, Skeleton
- **Chatverce-specific:** Search conversations, search assistants, search contacts. Cross-channel search (WhatsApp + Telegram results together).
- **Platform:** Web: Cmd+K global search. Mobile: Pull-down search bar (iOS), search icon in header (Android).

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/searching/SKILL.md
git commit -m "feat(cdg): add searching pattern — global search, cross-channel, empty results"
```

---

### Task 5: Settings Pattern

**Files:** Create `chatverce-design/skills/patterns/settings/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **Emotional intention:** Trust — "My data, my rules."
- **How it works:** Grouped settings with clear labels. Toggles for on/off. Destructive settings (delete account, disconnect channel) require confirmation modal.
- **Components:** Toggle, Select, Form, Modal (confirm), Navigation, Accordion
- **Do:** Group related settings. Show current value. Save automatically on toggle change.
- **Don't:** Bury important settings. Use jargon in setting labels.
- **Platform:** Web: Sidebar nav + content area. Mobile: Stacked list with drill-down navigation.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/settings/SKILL.md
git commit -m "feat(cdg): add settings pattern — grouped preferences, destructive confirmations"
```

---

### Task 6: Modality Pattern

**Files:** Create `chatverce-design/skills/patterns/modality/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **Emotional intention:** Focus — "This needs my attention right now."
- **Interruption hierarchy (least to most disruptive):**
  1. Inline (expandable sections, progressive disclosure) — no interruption
  2. Popover (contextual info, quick actions) — minimal interruption
  3. Drawer/Bottom sheet (detail views, forms) — moderate interruption
  4. Modal/Dialog (confirmations, critical flows) — full interruption
- **When to use each:** Decision tree based on: Is context needed? Is it dismissible? How much content?
- **Components:** Modal, Drawer, Popover, Alert
- **Critical rule:** One modal at a time. Never stack modals. If you need a modal inside a modal, redesign the flow.
- **Platform:** Web: Modal = centered overlay. Drawer = side panel. Mobile: Modal = bottom sheet. Drawer = bottom sheet (same component, different context).

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/modality/SKILL.md
git commit -m "feat(cdg): add modality pattern — interruption hierarchy, modal vs drawer vs popover"
```

---

### Task 7: Entering Data Pattern

**Files:** Create `chatverce-design/skills/patterns/entering-data/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **Emotional intention:** Effortless — "This form respects my time."
- **How it works:**
  - Single-column layout only
  - Labels always above, never floating/inside
  - Inline validation on blur (not on every keystroke)
  - Error messages below the field in red, with solution
  - Progressive disclosure — show fields as needed
  - Multi-step forms: progress indicator, save progress, back button
  - Smart defaults — pre-fill what you can
- **Components:** Form, Text input, Textarea, Select, Checkbox, Radio button, File upload, Progress indicator, Label, Fieldset
- **Chatverce-specific:** Assistant creation form, channel connection form, business profile setup.
- **Platform:** Web: Standard form layout. Mobile: ScrollView with KeyboardAvoidingView. Inputs use 16px minimum font (prevents iOS auto-zoom).

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/entering-data/SKILL.md
git commit -m "feat(cdg): add entering-data pattern — form flows, validation, progressive disclosure"
```

---

### Task 8: Managing Accounts Pattern

**Files:** Create `chatverce-design/skills/patterns/managing-accounts/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **Emotional intention:** Secure control — "I own my account."
- **Flows:** Sign up, Sign in, Password reset, Profile editing, Team management (invite, roles, remove), Account deletion (cooling-off period).
- **Components:** Form, Avatar, Badge, Table, Modal
- **Chatverce-specific:** Team roles for assistant management. Channel ownership. API key management ("Secret key" in user-facing copy).
- **Platform:** Web: Full-page auth flows. Mobile: Native auth screens, biometric login support.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/managing-accounts/SKILL.md
git commit -m "feat(cdg): add managing-accounts pattern — auth, profile, team management"
```

---

### Task 9: Managing Notifications Pattern

**Files:** Create `chatverce-design/skills/patterns/managing-notifications/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **Emotional intention:** Informed, not overwhelmed — "I see what matters."
- **Types:** In-app (toast, badge, alert), Push (mobile), Email digest.
- **Batching:** Group related notifications. "3 new conversations" not 3 separate notifications.
- **Preferences:** Per-channel, per-type controls. Global quiet hours.
- **Components:** Toast, Badge, Toggle, List, Alert
- **Chatverce-specific:** New conversation notification, assistant error alert, weekly stats digest, channel disconnection warning.
- **Platform:** Web: In-app only (badge, toast). Mobile: Push via `expo-notifications`. Notification grouping on both iOS and Android.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/managing-notifications/SKILL.md
git commit -m "feat(cdg): add managing-notifications pattern — batching, preferences, push"
```

---

### Task 10: Undo & Recovery Pattern

**Files:** Create `chatverce-design/skills/patterns/undo-and-recovery/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **Emotional intention:** Safety net — "I can fix mistakes."
- **Undo:** Toast with "Undo" action button for reversible actions (archive, move, status change). 8-second window.
- **Recovery:** For non-undoable actions, confirm before acting. Show what will happen.
- **Data loss prevention:** Warn before navigating away from unsaved changes. Auto-save drafts.
- **Components:** Toast (with undo action), Alert, Modal
- **Chatverce-specific:** Undo assistant archive. Recover deleted conversation (30-day window). Undo "Go live" (take assistant offline).
- **Platform:** Web: Toast with undo. Mobile: Toast with undo + swipe to undo on lists.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/undo-and-recovery/SKILL.md
git commit -m "feat(cdg): add undo-and-recovery pattern — undo toast, data loss prevention"
```

---

### Task 11: AI Interactions Pattern (Expanded)

**Files:** Create `chatverce-design/skills/patterns/ai-interactions/SKILL.md`

**This is the most critical pattern for Chatverce — write at full depth.**

- [ ] **Step 1: Write SKILL.md**

**Required sections (from spec lines 700–730):**

1. **What the User Is Doing:** Interacting with AI-powered features — creating assistants, watching AI process conversations, reviewing AI suggestions, correcting AI outputs.

2. **Emotional Intention:** Trust and transparency — "I understand what the AI is doing and I'm in control."

3. **Processing States** — Full table from spec:
   - Thinking (0-3s): Teal pulse, "Thinking..."
   - Working (3-30s): Teal pulse + progress text
   - Long-running (30s+): Progress bar with steps
   - Complete: Checkmark fade-in, "All set."
   - Failed: Coral icon + solution text

4. **Uncertainty/Confidence:**
   - Never show confidence scores to users
   - Use language: "This looks like..." (confident) vs "I'm not sure..." (uncertain)
   - AI suggestions visually distinct from confirmed data

5. **Correction:**
   - Always let users correct AI outputs (edit, regenerate, reject)
   - Show what the AI did, make it easy to undo
   - "Smart suggestions" pattern: AI proposes, user confirms

6. **Transparency:**
   - Communicate where AI is involved
   - Never trick users into thinking AI content is human
   - Clear visual indicator for AI-generated responses
   - Explain capabilities and limitations in onboarding

7. **Components:** AI working indicator, Spinner, Toast, Alert, Progress bar, Badge

8. **Platform Considerations:**
   - Web: Inline AI indicators, streaming text for chat responses
   - Mobile: Haptic on AI completion (success notification). AI processing respects battery — show "Processing on server" if needed.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/ai-interactions/SKILL.md
git commit -m "feat(cdg): add ai-interactions pattern — processing states, uncertainty, correction, transparency"
```

---

### Task 12: Real-time Communication Pattern

**Files:** Create `chatverce-design/skills/patterns/real-time-communication/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **Emotional intention:** Connected presence — "I see what's happening right now."
- **Typing indicators:** Animated dots when user/assistant is typing. Timeout after 10s of no new input.
- **Read receipts:** Subtle check marks (sent → delivered → read). Optional per user.
- **Presence:** Online/offline/away status on avatars. Green dot for online.
- **Message states:** Sending → Sent → Delivered → Read → Failed (with retry).
- **Components:** Avatar, Badge, Spinner, List, Conversation thread
- **Chatverce-specific:** Multi-channel presence (user online on WhatsApp, offline on Telegram). AI assistant always "online."
- **Platform:** Web: WebSocket-driven updates. Mobile: Push notifications for background, WebSocket for foreground.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/real-time-communication/SKILL.md
git commit -m "feat(cdg): add real-time-communication pattern — typing, presence, message states"
```

---

### Task 13: Channel Switching Pattern

**Files:** Create `chatverce-design/skills/patterns/channel-switching/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **Emotional intention:** Unified view — "All my channels, one place."
- **How it works:** Conversations from all channels (WhatsApp, Telegram, Webchat, Phone, Email) appear in unified inbox. Channel indicator badge on each conversation. Filter by channel. Context preserved when switching.
- **Components:** Channel indicator, Badge, Tabs, Segmented control, List
- **Chatverce-specific:** Channel color coding (WhatsApp green, Telegram blue, etc. — ONLY on channel indicators). Cross-channel conversation threading. Channel connection status.
- **Do:** Show channel source clearly. Allow filtering by channel. Preserve scroll position when switching.
- **Don't:** Use channel colors for anything except indicators. Make switching require navigation.
- **Platform:** Web: Channel filter tabs or sidebar. Mobile: Segmented control or filter chips at top of conversation list.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/channel-switching/SKILL.md
git commit -m "feat(cdg): add channel-switching pattern — unified inbox, cross-channel threading"
```

---

### Task 14: Going Live Pattern

**Files:** Create `chatverce-design/skills/patterns/going-live/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

**Key content:**
- **Emotional intention:** Quiet pride — "You built this."
- **How it works:**
  1. Pre-flight check: Verify assistant configuration is complete (channel connected, responses configured)
  2. Preview: "Test your assistant" — simulated conversation
  3. Go live confirmation: Clear statement of what will happen ("Your assistant will start responding to WhatsApp messages")
  4. Status transition: "Going live..." → "Your assistant is live" (with celebration moment — warm copy, checkmark animation)
  5. Post-live: Dashboard shows real-time activity
- **Status states:** Draft → Testing → Live → Paused → Archived
- **Components:** Progress indicator, Button, Modal, Toast, Alert, Badge
- **Chatverce-specific:** The Autopilot Promise culminates here. "Go live" not "Deploy." Status badges on assistant cards.
- **Platform:** Web: Multi-step flow with sidebar progress. Mobile: Full-screen steps with haptic success on go-live.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/patterns/going-live/SKILL.md
git commit -m "feat(cdg): add going-live pattern — pre-flight, preview, status transitions"
```

---

## Plan B Complete Checklist

After all 14 tasks, verify:

- [ ] All 14 pattern directories exist under `skills/patterns/`
- [ ] Each SKILL.md has proper YAML frontmatter
- [ ] Each pattern references components from the catalog
- [ ] Each pattern has platform considerations for Web, iOS, and Android
- [ ] No hardcoded hex values (reference foundation tokens)
- [ ] Terminology matches writing foundation (e.g., "Go live" not "Deploy")

```bash
find chatverce-design/skills/patterns -name "SKILL.md" | wc -l
```

Expected: 14 files
