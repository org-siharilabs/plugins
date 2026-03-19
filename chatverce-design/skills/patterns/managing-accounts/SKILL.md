---
name: managing-accounts
description: Account lifecycle flows covering authentication, profile management, team access, and safe account deletion
user_invocable: false
---

# Managing Accounts

## What the User Is Doing

The user is operating on their account or their team's access — signing in, updating their profile, inviting colleagues, assigning roles, or making the irreversible decision to delete their account. These flows are infrequent but high-stakes: a sign-in problem blocks all product use; a mis-configured team role can expose sensitive data.

## Emotional Intention

**"I own my account."** Users should feel that their account is secure, comprehensible, and entirely under their control. Authentication should be fast and trustworthy. Team management should make permissions obvious. Account deletion should be possible but considered — the product respects the user's decision without making it a hostage situation.

## How It Works

**Sign up / Sign in:**
- Email + password authentication as baseline
- OAuth (Google) as a fast-track option
- Password requirements shown inline before submission (not after), so the user knows the rules before typing
- On sign-in failure, show a specific inline error ("Incorrect password" vs "No account found") — do not merge them into a generic "Invalid credentials" message
- "Forgot password?" link always visible on the sign-in form, never buried

**Password reset:**
1. User enters email address
2. System sends a reset link (show confirmation: *"Check your inbox — we sent a reset link to [email]"*)
3. Reset link opens a new-password form; validate password strength inline
4. On success, redirect to sign-in with a toast: *"Password updated. Sign in with your new password."*

**Profile editing:**
- Name, avatar, email, timezone, language
- Avatar upload uses drag-and-drop on web; tap-to-replace on mobile
- Email change requires re-authentication (enter current password) before the change is confirmed
- Changes save on "Save changes" button click; no auto-save for profile fields

**Team management:**
- Team members are listed in a table: avatar, name, email, role, joined date, actions
- Invite new member: email input + role select → sends invitation email → row appears as "Pending" with a badge
- Role change: Select component per row, auto-saves on selection change, success toast confirms
- Remove member: triggers a modal confirmation — *"Remove [Name] from your team? They will lose access immediately."* — with a "Remove" destructive button

**Account deletion (cooling-off):**
1. User initiates deletion from the Danger zone in Settings
2. Modal asks: *"Are you sure? This deletes all your assistants, channels, and conversations permanently."*
3. Requires the user to type *"DELETE"* to confirm (not just a checkbox — deliberate friction for irreversible actions)
4. On confirm, the account enters a 7-day cooling-off period (data preserved, login disabled)
5. User receives an email with a reactivation link valid for 7 days
6. After 7 days, data is permanently purged

## Components Used

- **Form** — sign-up, sign-in, profile edit, password reset, invitation
- **Avatar** — profile photo with upload affordance
- **Badge** — team member role (Owner, Admin, Member), invitation status (Pending)
- **Table** — team members list with role and actions per row
- **Modal** — team member removal confirmation, account deletion confirmation

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

Authentication pages are **full-page layouts** — not modals over the product. Clean, centered card (max 400px) with the Chatverce wordmark above. No product navigation visible during auth — this is a focused flow.

Password inputs include a show/hide toggle (eye icon). Screen readers announce the toggle state: "Show password" / "Hide password."

### iOS (Expo + NativeWind)

Authentication on mobile uses a **native feel** — full-screen form with the keyboard appearing without scroll disruption (KeyboardAvoidingView). Support **Sign in with Apple** as required by App Store guidelines if OAuth is available.

**Biometric authentication:** For returning users, offer Face ID / Touch ID for sign-in after the initial password auth. Use `expo-local-authentication` to check availability and request biometric prompt. Display: *"Use Face ID to sign in faster"* as an opt-in on first session.

### Android (Expo + NativeWind)

Full-screen auth forms matching the iOS approach. Support **Google Sign-In** as a fast-track OAuth option — standard on Android. Biometric support via `expo-local-authentication` with Fingerprint.

## Do / Don't

| Do | Don't |
|----|-------|
| Show specific sign-in error messages ("Incorrect password" vs "No account found") | Merge all auth failures into a generic "Invalid credentials" message |
| Require re-authentication before allowing email address changes | Allow critical account changes without verifying identity |
| Show password strength requirements before the user starts typing | Only show requirements after a failed submission |
| Use a table layout for team members — scannable with actions per row | Use a flat card-per-member layout that doesn't scale past 5 members |
| Require typing "DELETE" for account deletion | Allow account deletion with a single checkbox |
| Implement a 7-day cooling-off period for account deletion | Delete data immediately on confirmation |

## Chatverce-Specific

**Team roles and assistants:** In Chatverce, team roles extend to assistant management. Admin and Owner roles can create, edit, and publish assistants. Member roles can view assistants and respond to conversations but cannot modify assistant configuration or go live. Enforce this in the UI: hide the "Go Live" button for Member roles and show a tooltip: *"Contact your admin to publish this assistant."*

**Channel ownership:** Each channel is owned by the account (not a specific team member). All Admins and Owners can reconnect, reconfigure, or disconnect channels. Members see channels as read-only.

**"Secret key" display for API keys:** API keys are displayed in Account Settings under Integrations. Keys are masked by default (`sk-••••••••••••••••`). A "Reveal" button shows the full key for 15 seconds, then re-masks automatically. Clipboard copy is always available without revealing. Never log or display API keys in URLs or toast messages.

**Invitation expiry:** Team invitations expire after 7 days. Expired invitations show in the team table as "Expired" badge with a "Resend" action. Accepted invitations transition to the standard member row automatically.
