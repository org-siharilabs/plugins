
# Undo and Recovery

## What the User Is Doing

The user did something they want to reverse — archived the wrong conversation, deleted a message template, paused an assistant they meant to keep running. Or the user is about to do something irreversible and needs a moment to reconsider. The product's job is to catch the user before they fall, and to help them recover when they do.

## Emotional Intention

**"I can fix mistakes."** Users should be able to act quickly, confident that consequential errors are recoverable. Fear of making mistakes slows users down and erodes trust. A product that handles mistakes gracefully builds the kind of trust that makes users efficient. The product is a safety net, not a minefield.

## How It Works

**Undo (reversible actions):**

1. User performs a reversible action (archive, mute, unpublish, bulk select + archive)
2. Toast appears immediately with an **Undo** button — do not auto-dismiss this toast
3. The user has an **8-second window** to click Undo
4. If Undo is clicked within 8 seconds: action is reversed, toast dismisses, brief confirmation toast appears (*"Undone."* — 2s auto-dismiss)
5. If 8 seconds pass with no action: toast auto-dismisses, action is committed

**Recovery (data that persists after deletion):**

| Item | Recovery period | Recovery path |
|------|----------------|---------------|
| Deleted conversation | 30 days | Settings → Data → Deleted conversations |
| Archived assistant | Indefinite | Assistants → Archived tab → Restore |
| Deleted message template | 30 days | Settings → Data → Deleted templates |

Items in recovery are accessible but not active. Recovery restores the item to its last state before deletion.

**Preventing data loss:**

- If the user navigates away from a form with unsaved changes, show a confirmation alert: *"You have unsaved changes. Leave without saving?"* with "Discard changes" and "Keep editing" options
- Auto-save drafts for long-form content (assistant prompt, message templates) every 30 seconds; display a *"Draft saved"* indicator (not a toast — use subtle status text)
- The draft indicator resets to *"Unsaved changes"* when the user modifies the content

**Confirm before non-undoable actions:**

Some actions are genuinely irreversible — account deletion, permanent data purge, channel disconnection (all conversations from that channel are paused). For these:
1. Use a Modal confirmation (see `modality` pattern)
2. Use specific, honest language: *"This cannot be undone."* Never soften irreversibility
3. For the most extreme cases (account deletion), require deliberate input — type "DELETE"

## Components Used

- **Toast (with Undo action button)** — 8-second undo window; does not auto-dismiss while the button is present
- **Alert** — unsaved changes warning on navigation, blocking error when data loss is imminent
- **Modal** — confirmation for truly irreversible actions

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

Undo toast appears in the top-right position (consistent with other toasts). The Undo button is styled as a ghost/outline button inside the toast — distinct from the close × button. The toast includes a subtle 8-second progress bar at the bottom so the user can see the window closing.

Unsaved changes alert: use the browser's `beforeunload` event as a fallback, plus an in-product confirmation dialog for in-app navigation (Next.js router `routeChangeStart` event).

### iOS (Expo + NativeWind)

Undo toast appears at the top below the status bar (consistent with other toasts on iOS). Same 8-second window.

**Swipe-to-undo on lists:** For actions triggered by swipe gestures (swipe to archive, swipe to delete), use the iOS-native shake-to-undo or a visible "Undo" toast as the primary recovery mechanism. Do not rely on the device accelerometer shake for undo in a business app — it is unreliable and obscure.

Haptics: `Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium)` on the Undo button tap to confirm the reversal.

### Android (Expo + NativeWind)

Undo toast appears at the bottom above the navigation bar (Android Snackbar convention). Same 8-second window. The Undo button within the toast uses `cv-text-on-accent` color (teal text on the dark toast background) — following Android Material Snackbar conventions.

## Do / Don't

| Do | Don't |
|----|-------|
| Show an Undo button in the toast for every reversible action | Show a reversible action toast that auto-dismisses without an Undo option |
| Give users an 8-second window with a visible countdown | Set the undo window so short that it is practically unreachable |
| Auto-save drafts every 30 seconds for long-form content | Let users lose 10 minutes of work on a prompt if they navigate away |
| Warn before navigating away from unsaved form changes | Silently discard unsaved changes on back navigation |
| Use honest language for irreversible actions ("This cannot be undone") | Soften irreversibility ("Are you sure you want to continue?") |
| Keep deleted items recoverable for 30 days | Purge data immediately on user deletion |

## Chatverce-Specific

**Undo archive conversation:** The most common undo scenario. Archive is triggered by a swipe or a button. Undo toast: *"Conversation archived. Undo"* — 8 seconds. The conversation visually leaves the list immediately (optimistic UI); if Undo is clicked, it returns to its original position with an entry animation.

**Recover deleted conversation (30 days):** Conversations moved to the deleted state are recoverable for 30 days. Access via Settings → Data → Deleted conversations. Recovery restores the conversation to the Inbox. After 30 days, a scheduled purge removes the data permanently. A warning email is sent at the 25-day mark: *"A conversation from Priya will be permanently deleted in 5 days."*

**Undo Go Live:** If an assistant is published (Go Live), a brief undo window exists. Toast: *"[Assistant Name] is live. Undo"* — 8 seconds. If Undo is clicked, the assistant returns to Draft status and a toast confirms: *"[Assistant Name] unpublished."* After 8 seconds, no undo — use the Pause action instead.

**Unsaved assistant prompt:** The assistant prompt editor auto-saves drafts every 30 seconds. The draft indicator in the editor header reads *"Draft saved 30s ago"* or *"Unsaved changes."* The user can discard the draft explicitly (confirm modal: *"Discard draft and revert to saved version?"*).
