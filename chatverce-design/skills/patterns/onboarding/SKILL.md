---
name: onboarding
description: Progressive first-run experience that gets users to value in under 3 minutes
user_invocable: false
---

# Onboarding

## What the User Is Doing

The user has signed up for Chatverce. They have some intent — automate support, deploy a sales assistant, manage WhatsApp conversations — but have not yet done anything meaningful in the product. They are evaluating whether the product is worth their time and effort. The first 3 minutes are the product's job interview.

## Emotional Intention

**"This will only take a minute."** The user should feel that setup is achievable, not daunting. Every step should feel like forward progress, not work. The product meets the user where they are and removes every obstacle between them and their first working assistant. Onboarding is not a form — it is a guided handoff to the product's core value.

Reference: [design-ethos] — "Chatverce's Autopilot Promise: the product does the work, not the user."

## How It Works

Onboarding follows a 3-step wizard. Steps are sequential but each step is self-contained — a user who drops off can resume.

**Step 1: Connect a Channel**
- User selects WhatsApp, Telegram, or another available channel
- OAuth or QR-code pairing flow (channel-specific)
- Success state: channel name and avatar shown, green status indicator
- If user skips: empty state with CTA to connect later

**Step 2: Create Your First Assistant**
- Name, persona prompt, language setting
- Default values pre-filled ("Your first assistant", "English") — user can accept or change
- Preview: live test input showing how the assistant will respond
- Complexity is hidden behind "Advanced settings" accordion

**Step 3: Go Live**
- Review summary: channel connected, assistant configured
- Single "Go Live" button — the Autopilot Promise culminates here
- Post-action: success toast + redirect to dashboard with live badge on assistant card

**Progressive disclosure rules:**
- Show only what is required for the current step
- Advanced options are always in an accordion, collapsed by default
- Never ask for information that is not used immediately or in the next step

**Empty states as onboarding:** After first login, if no onboarding has been started, the dashboard shows an empty state that doubles as the onboarding entry point. The empty state is not a dead end — it is the first step.

**Tooltips on first encounter:** On a user's first visit to the settings page, assistant editor, and channel manager, show a single tooltip pointing to the most important action. Dismiss on any click. Never show more than one tooltip at a time.

## Components Used

- **Progress indicator** — 3-step linear progress bar at the top of the wizard
- **Modal** — contains the wizard on web; full-screen on mobile
- **Form** — channel credentials, assistant configuration
- **Empty state** — dashboard zero state acts as onboarding trigger
- **Hero** — step-level illustration or icon to orient the user to what they are configuring
- **Tooltip** — first-encounter hints on key interactive elements
- **Button (primary)** — "Continue", "Go Live" — always one primary action per step

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

Render the wizard as a full-page overlay (not a modal dialog) with a persistent sidebar showing the 3 steps. The sidebar gives the user spatial awareness — they can see where they are and what remains. The active step label is bold; completed steps show a checkmark icon using `cv-status-success` color.

Keyboard navigation is required: `Tab` through form fields, `Enter` to advance, `Escape` exits to dashboard (with a confirmation if unsaved).

### iOS (Expo + NativeWind)

Full-screen pages, one per step. Swipe right-to-left to advance (with a visible "Continue" button as the primary affordance). Swipe left-to-right to go back. Step dots at the bottom provide position awareness. Use `cv-duration-slow` (500ms) slide transition between pages.

The "Go Live" step uses `Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success)` when the assistant goes live.

### Android (Expo + NativeWind)

Same full-screen page pattern as iOS. Use the Android back gesture to navigate to the previous step (do not exit onboarding). Step dots at the bottom. No swipe-to-advance — rely on the visible "Continue" button to align with Android navigation conventions.

## Do / Don't

| Do | Don't |
|----|-------|
| Pre-fill default values wherever reasonable | Ask for information you won't use in the flow |
| Show exactly 3 steps — no more | Add "optional" steps that feel required |
| Let users skip and return to complete setup later | Block dashboard access until onboarding is complete |
| Use the empty state as the natural entry point | Build a separate marketing-style welcome screen |
| Celebrate completion ("Your assistant is live!") | Silently redirect without acknowledgment |
| One primary action per step | Show two "Continue" buttons or ambiguous next actions |

## Chatverce-Specific

**The opening line matters.** Use: *"Let's set up your first assistant — takes about 3 minutes."* Not "Welcome to Chatverce." Not "Get started." The 3-minute promise is a commitment. Honor it by keeping the wizard under 3 minutes for an average user.

**Autopilot Promise:** The onboarding flow is where the Autopilot Promise is made tangible. By the end of step 3, the user's assistant is live and handling real conversations. The product has done the setup work; the user only made decisions.

**Channel connection error:** If the OAuth or QR pairing fails, show an inline Alert (not a toast) with the specific error and a retry button. Do not advance the progress indicator on failure. The user must successfully connect a channel before moving to step 2 — this is not skippable.

**Returning users:** If a user has completed onboarding before, never re-show the wizard. Add new channels or assistants through the regular Settings flow, not a re-run of onboarding.
