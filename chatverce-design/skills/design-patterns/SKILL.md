---
name: design-patterns
description: "14 behavioral patterns for Chatverce UX: loading, onboarding, feedback, search, settings, modality, forms, accounts, notifications, undo, AI interactions, real-time communication, channel switching, and going live. Use when implementing user flows."
user_invocable: false
---

# Chatverce Design Patterns

14 behavioral patterns that define HOW users accomplish tasks in Chatverce. These bridge foundations (the visual primitives) and components (the building blocks) — they describe the orchestration.

## Pattern Reference Files

Load specific patterns based on the user flow you're implementing:

### Core UX Patterns
- [loading.md](loading.md) — Skeleton screens, optimistic UI, AI processing states, time-sensitive feedback
- [onboarding.md](onboarding.md) — 3-step wizard, progressive disclosure, first-run experience
- [feedback.md](feedback.md) — Toast/Alert decision tree by urgency, success/error/warning states
- [searching.md](searching.md) — Cross-channel search, Cmd+K command palette, filters
- [settings.md](settings.md) — Grouped preferences, destructive confirmations, privacy controls
- [modality.md](modality.md) — Interruption hierarchy: inline → popover → drawer → modal (no modals on mobile)

### Data & Account Patterns
- [entering-data.md](entering-data.md) — Form flows, validation timing, KeyboardAvoidingView, multi-step forms
- [managing-accounts.md](managing-accounts.md) — Auth flows, teams, roles, profile management
- [managing-notifications.md](managing-notifications.md) — Batching, quiet hours, push notification patterns
- [undo-and-recovery.md](undo-and-recovery.md) — Undo toast (8s window), data loss prevention, confirmation patterns

### Chatverce-Specific Patterns
- [ai-interactions.md](ai-interactions.md) — Processing states (Thinking→Working→Complete→Failed), uncertainty, correction, transparency
- [real-time-communication.md](real-time-communication.md) — Typing indicators, read receipts, presence dots, message states
- [channel-switching.md](channel-switching.md) — Unified inbox, cross-channel threading, channel indicators
- [going-live.md](going-live.md) — Pre-flight checklist → Preview → Go live → Dashboard flow

## Key Pattern Rules

1. **Modality hierarchy:** inline → popover → drawer → modal. Never use modals on mobile — use bottom sheets/drawers instead.
2. **Loading:** Show skeleton within 100ms. Use optimistic UI for actions under 2s. Show progress for actions over 4s.
3. **Feedback:** Toasts for success (auto-dismiss 5s). Alerts for errors that need action. Never toast errors.
4. **Forms:** Labels above inputs (never floating). One primary CTA per form. Validate on blur, not on change.
5. **Undo:** All destructive actions get an 8-second undo toast before execution.
6. **AI states:** Always show what the AI is doing. Never leave users guessing. Use the Thinking→Working→Complete→Failed state machine.
