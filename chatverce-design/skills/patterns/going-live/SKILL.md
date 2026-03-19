---
name: going-live
description: The publication flow that transforms a configured assistant into a live, customer-facing product
user_invocable: false
---

# Going Live

## What the User Is Doing

The user has configured an assistant, connected a channel, and is about to publish it to real customers. This is the culminating moment of their work in Chatverce. They are crossing a threshold: from setup to operation. The product should honor that moment — treating publication as a milestone, not a routine button click.

## Emotional Intention

**"You built this."** Quiet pride, not fanfare. The user should feel the satisfaction of completing something real — an assistant that will now handle conversations on their behalf. The interface marks the moment without being frivolous about it. *"Your assistant is live"* is a complete sentence; it does not need confetti.

Reference: [design-ethos] — "The Autopilot Promise culminates here."

## How It Works

### Assistant Status States

| Status | Badge Color | Meaning |
|--------|------------|---------|
| Draft | `cv-text-tertiary` (muted) | In progress, not published |
| Testing | `cv-status-info` (teal) | Live to selected test contacts only |
| Live | `cv-status-success` (green) | Active, handling real conversations |
| Paused | `cv-status-warning` (amber) | Published but temporarily inactive |
| Archived | `cv-text-tertiary` (muted) | Retired, visible in archived list only |

### Pre-flight → Publish → Post-live Flow

**Step 1: Pre-flight check**
Before the user can publish, the system validates that all required configuration is complete:
- Channel connected and healthy ✓
- Assistant has a name ✓
- Assistant has a system prompt ✓
- Language set ✓
- (Optional) Test conversation completed ✓

If any required item is missing, it is shown as a checklist item with a red × and a direct action link (*"Add a system prompt →"*). The "Go Live" button is disabled until all required items are checked. Optional items show as greyed-out suggestions.

**Step 2: Preview / Test**
A *"Test your assistant"* section lets the user type a test message and see how the assistant responds. This is not a separate page — it is a panel within the pre-flight step. The user can send multiple messages and observe responses before committing.

**Step 3: Go Live confirmation**
A modal presents:
- Summary: assistant name, connected channel, primary language
- The Autopilot Promise: *"[Assistant Name] will handle all incoming [WhatsApp] conversations automatically. You can take over any conversation at any time."*
- Primary action: "Go Live" button (`cv-bg-accent`, not `cv-bg-destructive` — this is a positive action)
- Secondary action: "Cancel" (returns to editor)

**Step 4: Transition state**
After clicking "Go Live":
- Button transitions to loading state (spinner inside button, disabled)
- Status label below button: *"Going live…"*
- Duration: typically 2–5 seconds (backend provisioning)

**Step 5: Success**
- Button resolves; no more spinner
- Toast: *"[Assistant Name] is live!"* (8-second window with Undo action — see `undo-and-recovery` pattern)
- Redirect to dashboard; assistant card now shows green "Live" status badge
- Dashboard header banner on first go-live: *"Your first assistant is handling conversations. Welcome to Autopilot."* (dismissible, one-time only)

## Components Used

- **Progress indicator** — pre-flight checklist shows completion state per item
- **Button (primary)** — "Go Live" — disabled until pre-flight passes
- **Modal** — Go Live confirmation
- **Toast** — *"[Assistant Name] is live!"* with Undo (8-second window)
- **Alert** — pre-flight blocking errors, post-live channel health alerts
- **Badge** — assistant status (Draft, Testing, Live, Paused, Archived) on assistant cards

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

The pre-flight and test steps are displayed in a **multi-step sidebar layout** — left sidebar shows the pre-flight checklist and step progress; right content area shows the test conversation panel. This layout mirrors the onboarding wizard to reinforce familiarity.

The Go Live confirmation modal is centered. Width: 480px. The "Go Live" primary button uses `cv-bg-accent` (teal) — reinforcing that this is the positive culminating action.

The `Escape` key dismisses the confirmation modal and returns to the editor. Do not dismiss on backdrop click — the user should not accidentally cancel a deliberate Go Live by clicking outside.

### iOS (Expo + NativeWind)

Full-screen step-by-step presentation — each step (pre-flight, test, confirmation) is a separate screen with a back button to the previous step. The progress indicator at the top shows 3 steps.

**Haptic on success:** `Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success)` when the assistant transitions to Live status. This is the single most important haptic in the product — it marks the Autopilot Promise completion.

The confirmation screen's "Go Live" button is a full-width primary button at the bottom of the screen, above the safe area. Prominent, reachable.

### Android (Expo + NativeWind)

Same full-screen steps as iOS. Android back gesture navigates to the previous step. The "Go Live" button is full-width at the bottom. Same haptic success pattern via `expo-haptics`.

The first-time "Welcome to Autopilot" banner after go-live is a persistent card at the top of the Android dashboard, dismissible with a swipe.

## Do / Don't

| Do | Don't |
|----|-------|
| Validate all required configuration before enabling "Go Live" | Allow publishing an assistant with a missing system prompt or no channel |
| Use the exact copy "Go Live" — not "Deploy", "Publish", or "Activate" | Use technical publishing terminology (deploy, ship, release) |
| Use `cv-bg-accent` (teal) for the "Go Live" button — this is positive | Use `cv-bg-destructive` (red) for a positive publishing action |
| Show a toast with an Undo option (8-second window) after going live | Go live silently with no confirmation or feedback |
| Mark the first go-live as a milestone with a one-time banner | Show the milestone banner on every go-live after the first |
| Include haptic feedback on mobile for the go-live success | Fire haptics during the loading state — only on confirmed success |

## Chatverce-Specific

**"Go Live" not "Deploy":** Language matters. *Deploy* is infrastructure language. *Go Live* is human language — it communicates consequence (real customers, real conversations) without technical framing. This word is locked across the entire product; never substitute it.

**Undo Go Live:** After publishing, the user has an 8-second window to undo (toast with Undo button). After 8 seconds, the assistant is live and the path to deactivation is the "Pause" action — not undo. The Pause action is prominent on the assistant card for Live assistants.

**Autopilot Promise completion:** On first go-live, the dashboard header shows: *"Your first assistant is handling conversations. Welcome to Autopilot."* This is shown exactly once per account — the first time any assistant goes live. It is the product's acknowledgment that the Autopilot Promise has been delivered.

**Status badge on assistant cards:** Every assistant card on the dashboard displays its status badge. Status badges use the color tokens from the states table and are the primary at-a-glance health indicator. A Live assistant with a channel error should show both a green "Live" badge and an amber alert icon — both states are true and both matter.

**Paused vs. Archived:** Paused means temporary — the user intends to resume. Archived means retired — the assistant is done. These are separate actions with different confirmations. Archiving requires a confirmation modal; pausing does not.
