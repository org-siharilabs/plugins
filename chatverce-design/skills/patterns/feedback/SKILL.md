---
name: feedback
description: System feedback patterns that match the urgency and recoverability of every action outcome
user_invocable: false
---

# Feedback

## What the User Is Doing

The user took an action — saved a setting, deleted a conversation, submitted a form, triggered an AI run — and needs to know what happened. Without feedback, they are left guessing: Did it work? Did I break something? Should I try again? Feedback closes the action loop.

## Emotional Intention

**"I know the result of my action."** Users should feel oriented after every interaction. Positive outcomes should feel affirming without being distracting. Errors should feel solvable, not alarming. The product tells users what happened and what to do next — it never leaves them stranded.

## How It Works

Feedback type is determined by a decision tree based on outcome type, severity, and whether action is required.

**Decision tree:**

| Outcome | Severity | Action Required | Pattern |
|---------|---------|----------------|---------|
| Success — routine | Low | No | Toast (4s auto-dismiss) |
| Success — milestone | High | No | Toast with celebration label (e.g., "Assistant is live!") |
| Error — recoverable | Medium | Yes (retry/fix) | Toast + action button |
| Error — blocking | High | Yes (must resolve) | Inline Alert (persistent) |
| Warning — attention needed | Medium | Optional | Alert (persistent, dismissible) |
| Info — context | Low | No | Alert (dismissible) |
| Destructive action — confirm | High | Yes (confirm/cancel) | Modal confirmation |

**Toast rules:**
- Auto-dismiss at 4 seconds for informational and success toasts
- Toasts with action buttons (Undo, Retry) do not auto-dismiss — the user must act or manually close
- Maximum 2 toasts stacked at any time; a third queues and replaces the oldest
- Toasts are never used for errors that require the user to do something — use an inline Alert

**Alert rules:**
- Inline Alerts sit within the content area, contextually close to the affected element
- Persistent alerts (warnings, blocking errors) require explicit dismissal
- Never use an Alert for a success — that is Toast territory unless the success is very detailed

**Modal confirmation rules:**
- Required before any non-undoable destructive action: deleting an assistant, removing a channel, account deletion
- Contains: description of what will be destroyed, a destructive confirm button (`cv-bg-destructive`), and a cancel button
- Never use a modal for success or informational feedback

## Components Used

- **Toast** — transient feedback, success, recoverable errors, undo actions
- **Alert** — persistent warnings, blocking errors, informational banners
- **Badge** — status indicator on cards (Live, Error, Draft) — passive feedback without interruption
- **Modal** — destructive action confirmation only

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

Toasts appear in the **top-right corner**. They enter with `cv-ease-out` (300ms) and exit with `cv-ease-in` (150ms). Stack vertically with 8px spacing between toasts.

Inline Alerts use `cv-bg-destructive-subtle` (error), `cv-status-warning` (warning), or `cv-status-info` (info) as the left border accent — 4px left border reinforces severity without overwhelming the layout.

### iOS (Expo + NativeWind)

Toasts appear at the **top of the screen, below the Dynamic Island / status bar**. Use safe area insets. Enter from above, exit upward. 300ms `cv-ease-out` enter, 150ms `cv-ease-in` exit.

Success toasts pair with `Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success)`. Error toasts pair with `Haptics.notificationAsync(Haptics.NotificationFeedbackType.Error)`. Haptics are additive — never replace the visual with haptic alone.

### Android (Expo + NativeWind)

Toasts appear at the **bottom of the screen, above the navigation bar**. Android users are conditioned to bottom feedback (system Snackbar pattern). Enter from below, exit downward.

Same haptic patterns as iOS via `expo-haptics`. Check availability — no-ops on devices without haptic support.

## Do / Don't

| Do | Don't |
|----|-------|
| Match feedback urgency to placement (toast = transient, alert = persistent) | Show a toast for an error that requires the user to take action |
| Use destructive button color (`cv-bg-destructive`) in confirmation modals | Stack more than 2 toasts simultaneously |
| Auto-dismiss informational toasts at 4 seconds | Auto-dismiss toasts that contain undo buttons |
| Keep inline Alerts contextually close to the affected element | Use a modal to confirm a routine, reversible action |
| Pair haptics with visual feedback on mobile | Use haptics as the only feedback signal |
| Provide a next step in error messages ("Check your connection and retry") | Show "An error occurred." with no action path |

## Chatverce-Specific

**Assistant status changes:** When an assistant goes from Draft → Live, use a celebration toast: *"Your assistant is live!"* with the assistant name. This is a milestone, not a routine action — treat it as such.

**Channel errors:** If a channel disconnects (e.g., WhatsApp session expired), use a persistent inline Alert on the dashboard — not a toast. The user needs to reconnect before the assistant can work. This is a blocking error.

**Bulk actions (archiving multiple conversations):** Show a single toast summarizing the operation: *"3 conversations archived."* with an Undo button. Do not show 3 separate toasts.

**AI run failure:** If an AI assistant fails to respond, show an inline Alert within the conversation thread — not a global toast. The error is contextual to that conversation. Include a "Retry" action button.

**Destructive: Delete assistant.** Modal must state: *"Deleting [Assistant Name] will remove all its configuration. This cannot be undone."* The confirm button label is "Delete assistant", not "OK" or "Confirm".
