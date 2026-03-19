---
name: modality
description: Interruption hierarchy governing when to use inline, popover, drawer, or modal — and how to stack them correctly
user_invocable: false
---

# Modality

## What the User Is Doing

The user is in the middle of a task, and the product needs to either present additional information, ask for input, or confirm a decision. The question is: how much should this interrupt the user? Modality determines the answer by matching the visual weight of the interruption to the significance of the decision.

## Emotional Intention

**"This needs my attention."** When a modal, drawer, or popover appears, it should feel proportional — neither alarming when routine nor dismissible when critical. The right interruption weight signals the right urgency. A modal means: stop and decide. A popover means: here is more information. Inline means: see this, but keep working.

## How It Works

**Interruption hierarchy — choose the lowest level that serves the need:**

| Level | Component | Use When |
|-------|-----------|----------|
| 1. Inline | Alert, inline error | Context is self-contained; user should not be interrupted |
| 2. Popover | Popover, Tooltip | Brief supplementary information; user keeps context of the page |
| 3. Drawer / Bottom sheet | Drawer | Significant secondary content; user may need to reference primary content |
| 4. Modal | Modal | Action requires full attention; current context must be suspended |

**Decision tree for choosing interruption level:**

1. **Does the user need to see background context while acting?** → Use a Drawer (side panel on web, bottom sheet on mobile)
2. **Is this a brief tooltip or help text?** → Use a Popover
3. **Is the action destructive and irreversible?** → Use a Modal
4. **Is the information contextually tied to a form field or element?** → Use Inline (Alert, helper text, error message)
5. **Does the action require the user's complete attention and blocks all other activity?** → Use a Modal

**Modal rules — strict:**
- One modal at a time. Never stack modals. If a modal action requires confirmation (e.g., a confirmation within a form modal), the outer modal closes and the confirmation modal replaces it.
- Every modal has exactly one primary action and one dismiss action ("Cancel" or "×")
- Modals are dismissible with `Escape` on web
- Modals with destructive primary actions use `cv-bg-destructive` for the confirm button
- Modals with neutral primary actions use `cv-bg-accent`

**Popover rules:**
- Popovers are not used for actions — only information and navigation
- Dismiss on any click outside or `Escape`
- Maximum 320px wide; keep content scannable

**Drawer rules:**
- Drawers preserve scroll position of the underlying page
- On web: slides in from the right side; does not cover the full screen
- On mobile: slides up from the bottom (bottom sheet pattern)
- Drawers may contain forms, longer content, or secondary navigation

## Components Used

- **Modal** — full-attention decisions and confirmations
- **Drawer** — secondary content panels, settings panels, detail views
- **Popover** — contextual information, supplementary help
- **Alert** — inline contextual warnings and errors (Level 1)

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

Modal: centered overlay with a backdrop. Width 480–600px depending on content. Enters with `cv-ease-out` fade + 8px translate-up (300ms); exits with `cv-ease-in` fade (150ms). Focus is trapped inside the modal; `Tab` cycles through interactive elements. Return focus to the triggering element on close.

Drawer: slides in from the right. Width 400px (default), up to 600px for complex content. Same enter/exit animation pattern as modal, but translating from the right.

Popover: anchored to the triggering element. Uses `@radix-ui/react-popover` via shadcn/ui. No animation required; instant show/hide is acceptable at this interruption level.

### iOS (Expo + NativeWind)

Modal maps to a **bottom sheet** (not a centered card). The sheet rises from the bottom, with a drag handle at the top. Heights: compact (40% screen), standard (65%), full (95%). Use `@gorhom/bottom-sheet`.

Drawer also maps to a bottom sheet on iOS — the distinction is in content volume and whether it's triggered by a navigation action (drawer) or a decision (modal).

Popovers are rare on iOS. Prefer action sheets (bottom) for multi-choice decisions and inline labels for supplementary information.

### Android (Expo + NativeWind)

Same bottom sheet pattern as iOS. Android back gesture dismisses modals and drawers. The drag-to-dismiss behavior should feel native: the sheet follows the user's finger and snaps closed if released past the threshold (50% of sheet height).

## Do / Don't

| Do | Don't |
|----|-------|
| Choose the lowest interruption level that serves the need | Default to modals for every non-trivial interaction |
| One modal at a time — never stack | Open a modal from inside a modal |
| Trap focus inside modals; return focus on close | Let focus escape a modal to the underlying page |
| Use `cv-bg-destructive` on modal confirm buttons for irreversible actions | Use the same button style for destructive and neutral modal actions |
| Dismiss popovers on outside click or Escape | Require explicit close for popovers (they are supplementary, not blocking) |
| Give every modal a clear dismiss path (Cancel, ×, Escape) | Create modals with no escape path |

## Chatverce-Specific

**Assistant configuration panel:** When editing an assistant from the dashboard, use a Drawer (not a modal) — the user may need to reference the assistant card behind it. The drawer slides in from the right on web, bottom sheet on mobile.

**Go Live confirmation:** Uses a Modal. This is the Autopilot Promise moment — it warrants full attention. Modal contains a pre-flight summary and the "Go Live" primary action button.

**Delete assistant:** Modal with destructive confirm. Not a drawer, not inline — this needs the user's deliberate focus.

**Conversation detail on mobile:** Use a full-screen navigation push, not a modal or drawer. Deep content deserves its own screen on mobile.

**Tooltip-level help:** On first encounter with a feature, use a Popover anchored to the element. Not a modal walkthrough. The user can dismiss and continue their task.
