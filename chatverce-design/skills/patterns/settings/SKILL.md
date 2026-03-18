---
name: settings
description: Organized, trustworthy settings surfaces that give users control without cognitive overload
user_invocable: false
---

# Settings

## What the User Is Doing

The user is configuring the product to match their preferences, business rules, or security requirements. They may be adjusting a single toggle or performing a significant action like deleting their account. The settings surface must support both — quick changes with no friction and consequential changes with appropriate confirmation.

## Emotional Intention

**"My data, my rules."** Users should feel genuinely in control. Settings are not a dumping ground for features that did not fit elsewhere — they are a first-class surface where the user's authority over the product is respected. Every setting has a clear label, a clear effect, and a clear path to undo or reverse it.

## How It Works

**Grouping:** Settings are organized into named groups. Related settings live together; unrelated settings do not share a section because of convenience. Each group has a heading and optionally a short description explaining what these settings affect.

**Standard groups for Chatverce:**
- Account (profile, password, email)
- Team (members, roles, invitations)
- Assistants (per-assistant settings accessible from here by reference, not inline)
- Channels (connected channels, reconnect, disconnect)
- Notifications (preferences per type and channel)
- Billing (plan, usage, invoices)
- Danger zone (account deletion, data export)

**Toggles (on/off settings):**
- Toggle state change is saved **immediately and automatically** — no save button required
- A brief success toast confirms the change: *"Notifications turned off."* (auto-dismiss 4s)
- If the save fails, revert the toggle and show an error toast with retry

**Select and form settings:**
- Show a "Save changes" button when the user has modified a field
- The button is disabled until a change is detected (no dirty = no save)
- Auto-save is not used for text field settings — explicit save intent prevents accidental changes

**Destructive settings (Danger zone):**
- Always grouped at the bottom, visually separated with a section heading "Danger zone"
- Each destructive action (delete account, delete all conversations) requires a modal confirmation
- Confirmation modals use `cv-bg-destructive` for the confirm button
- Labels are specific: "Delete account permanently", not "Delete"

**Accordion for advanced settings:**
- Secondary/advanced options live in a collapsed accordion by default
- Label the accordion: "Advanced settings" — not just "More"

## Components Used

- **Toggle** — boolean on/off settings, auto-save immediately
- **Select** — single-choice settings from a bounded list (language, timezone)
- **Form** — text field settings, requires explicit save
- **Modal** — confirmation for destructive settings actions
- **Navigation** — sidebar or list to move between settings groups
- **Accordion** — secondary/advanced settings, collapsed by default

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

Two-column layout: **left sidebar** with settings group navigation, **right content area** with the active group's settings. The sidebar uses `cv-bg-surface` and the active item is highlighted with `cv-bg-accent-subtle` and `cv-text-primary` bold.

The content area is a scrollable column, max-width 640px, centered in the right panel. Section headings use `cv-text-secondary` uppercase small label style.

Keyboard: `Tab` through settings fields; toggles respond to `Space`.

### Mobile (Expo + NativeWind)

Settings use a **stacked list with drill-down navigation**. The top level is a list of group names. Tapping a group navigates to a new full-screen page with that group's settings. Use the native back gesture or back button to return.

Toggles on mobile are `Switch` components from React Native, styled with NativeWind to use `cv-bg-accent` for the active state. They visually match the platform convention while using Chatverce color tokens.

Destructive settings appear at the bottom of the relevant group page — account deletion in "Account", channel removal in "Channels".

## Do / Don't

| Do | Don't |
|----|-------|
| Auto-save toggle changes immediately | Make users hunt for a save button after toggling a switch |
| Show a brief success toast on toggle save | Show no feedback when a toggle is changed |
| Group related settings with clear section headings | Create one giant flat list of all settings |
| Put destructive settings in a clearly labeled "Danger zone" at the bottom | Mix destructive settings with routine settings |
| Use a modal confirmation for irreversible destructive actions | Allow account deletion with a single click and no confirmation |
| Show a save button for text field settings only when a change is detected | Show a disabled save button when nothing has changed |

## Chatverce-Specific

**Assistant settings vs. global settings:** Settings in the Settings surface are account-wide or team-wide. Per-assistant configuration (prompt, language, handoff rules) lives in the assistant editor, not here. Settings should reference assistants by name but not inline their full configuration.

**Channel reconnect:** When a channel is disconnected (expired session), the Channels settings group shows the channel with an error badge and a "Reconnect" button. This is settings as error recovery.

**Team roles for Chatverce:**
- Owner — full access, billing, can delete account
- Admin — full access except billing and account deletion
- Member — can view and respond to conversations, cannot modify assistants or channels

Role assignment uses a Select component per team member row in the Team settings group. Role change saves on selection (auto-save), with a success toast.

**API keys:** Shown in Account settings under "Integrations." Keys are masked by default; reveal requires clicking a "Show" button. Copying uses a clipboard action with a brief success toast: *"Key copied."*
