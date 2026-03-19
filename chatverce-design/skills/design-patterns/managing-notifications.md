
# Managing Notifications

## What the User Is Doing

The user is deciding what they want to be interrupted by, and when. They may be configuring notification preferences proactively in Settings, or they are responding to an existing notification — tapping it, dismissing it, or acting on it. The notification system must surface genuine signal without generating noise that trains the user to ignore everything.

## Emotional Intention

**"I see what matters."** Users should feel that every notification is purposeful. No notification should leave them thinking "why did I get this?" Batch unimportant notifications; surface critical ones immediately. Give users precise control so they can tune the system to their workflow rather than silencing everything.

## How It Works

**Notification types and their channels:**

| Type | Trigger | In-App | Push (Mobile) | Email |
|------|---------|--------|---------------|-------|
| New conversation | Customer message arrives | Toast + badge | Yes | No |
| Assistant error | AI processing failure | Toast + inline Alert | Yes | Yes (digest) |
| Channel disconnect | Channel session expires | Persistent Alert | Yes | Yes (immediate) |
| Weekly stats | Sunday 8am | — | No | Yes |
| Team invitation | Admin invites member | — | No | Yes (immediate) |
| Go live success | Assistant published | Toast | No | No |

**Batching rules:**
- Multiple events of the same type within 60 seconds are batched into a single notification
- Format: *"3 new conversations"* not three separate *"New conversation from Priya"* toasts
- Batching applies to push notifications and in-app toasts — email digests are always batched by design

**Notification preferences:**
- Per-channel (users with WhatsApp and Telegram can configure each independently)
- Per-type (new conversations, errors, weekly stats can be toggled independently)
- Global quiet hours: set a time window during which no push notifications are sent (e.g., 10pm–8am)
- Preferences are in the Notifications settings group; see `settings` pattern

**Push notification handling (mobile):**
- Foreground (app is open): In-app toast only — do not also fire the OS push banner
- Background (app is closed): OS push notification → tapping opens the relevant conversation or screen
- Notification data includes a route (`/conversations/{id}`) for deep linking

## Components Used

- **Toast** — in-app real-time notification for new events
- **Badge** — unread count on conversation list items, channel icons, and the app icon (mobile)
- **Toggle** — notification preference on/off in Settings
- **List** — notification preference groups per-channel and per-type
- **Alert** — persistent in-app notification for critical states (channel disconnect)

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

In-app notifications only — web browsers require explicit permission for OS notifications, which Chatverce does not request (avoid permission fatigue). All real-time events surface as toasts.

Unread conversation count appears as a badge on the sidebar conversations link and as the browser tab title prefix: *(3) Chatverce*.

Notification preferences in Settings use a table layout: rows for each notification type, columns for each delivery channel (In-App, Push, Email) with Toggle per cell.

### iOS (Expo + NativeWind)

Use `expo-notifications` for push notification registration, scheduling, and handling.

Request push permission during onboarding (Step 2 or after "Go Live") — frame it as: *"Get notified when a new conversation arrives."* Do not request permission on cold launch without context.

Group notifications by conversation thread. The iOS notification center should show: *"Chatverce — 3 new conversations"* not 3 separate banners.

Badge count on the app icon reflects total unread conversations. Update via `expo-notifications` `setBadgeCountAsync`.

### Android (Expo + NativeWind)

Same `expo-notifications` implementation. Android does not require permission for notifications pre-Android 13; request for Android 13+ (API 33+).

Use notification channels (Android concept) to group Chatverce notifications: one channel for "Conversations", one for "Errors & Alerts". Users can configure these independently in Android system settings.

Notification grouping: use the `summaryText` field to display a group summary when 4+ notifications arrive.

## Do / Don't

| Do | Don't |
|----|-------|
| Batch multiple events of the same type within 60 seconds | Show 3 separate toasts for 3 new conversations in 30 seconds |
| Request push permission with context ("Get notified when…") | Request push permission on cold app launch with no framing |
| Suppress OS push banners when the app is in the foreground | Fire both an in-app toast and an OS banner simultaneously |
| Use persistent Alerts for blocking critical states (channel disconnect) | Use a dismissible toast for a channel disconnect — it will be missed |
| Offer per-channel and per-type notification preferences | Offer only a global "notifications on/off" toggle |
| Update app icon badge count to reflect unread conversations | Show a badge count for notifications the user has already seen |

## Chatverce-Specific

**New conversation:** The most frequent notification. In-app toast: *"New conversation from Priya (WhatsApp)"* with a channel indicator badge. Tapping opens the conversation. Batched version: *"3 new conversations"* with a count badge.

**Assistant error:** Push + email. An assistant failure is business-critical — the user's automation is broken. Use a persistent in-app Alert on the dashboard, not just a toast. Email subject: *"Your assistant [Name] encountered an error."*

**Weekly stats:** Email only, Sunday morning. Subject: *"Your Chatverce week: 47 conversations handled, 92% resolution rate."* No push notification — this is a digest, not urgent.

**Channel disconnect:** Persistent inline Alert on the dashboard, push notification, and email. All three channels because this is blocking: the assistant cannot work until reconnected. Alert includes a "Reconnect now" button.

**Quiet hours for business tools:** The target user is a business owner or support manager. Quiet hours should default to 10pm–8am local time, pre-configured — not opt-in. Users can widen this window, but it should not default to "always notify."
