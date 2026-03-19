---
name: real-time-communication
description: Live conversation indicators — typing, presence, message states, and read receipts — that make the product feel alive
user_invocable: false
---

# Real-Time Communication

## What the User Is Doing

The user is managing live conversations — watching a customer type, tracking whether a message was delivered and read, monitoring the presence of their AI assistant, or responding to an incoming message in real time. This is the highest-attention state in the product: the user is on, and they need the interface to keep up.

## Emotional Intention

**"I see what's happening right now."** The product should feel like a live window into conversations, not a list of static records. Real-time indicators are the mechanism that creates this feeling. Used correctly, they make the product feel responsive and alive. Used incorrectly (too many indicators, stale presence, unresponsive typing dots), they create confusion and erode the sense of control.

## How It Works

### Typing Indicators

- Animated three-dot indicator (*…*) in the conversation thread when the other party is typing
- Animation: three dots with staggered opacity pulse (dot 1 → dot 2 → dot 3 → repeat), `cv-duration-normal` loop
- **10-second timeout:** if no new typing event is received in 10 seconds, the indicator disappears automatically (the other party stopped typing without sending)
- Show typing indicator only when the user is viewing the conversation — do not show it in the conversation list as a persistent badge

### Read Receipts

Read receipts are **optional** and controlled by the end customer's privacy settings on their platform (WhatsApp, Telegram). Do not show read receipts if the platform does not provide them.

| State | Icon | Color |
|-------|------|-------|
| Sending | Single clock/pending icon | `cv-text-tertiary` |
| Sent (server received) | Single checkmark | `cv-text-secondary` |
| Delivered (customer device received) | Double checkmark | `cv-text-secondary` |
| Read (customer opened) | Double checkmark filled | `cv-status-success` green |
| Failed | X or alert icon | `cv-status-error` + Retry action |

### Presence Indicators

Presence is shown on avatars as a small colored dot (8px) at the bottom-right of the avatar.

| State | Color | Token |
|-------|-------|-------|
| Online | Green | `cv-status-success` |
| Away / Idle | Amber | `cv-status-warning` |
| Offline | None (no dot) | — |

**Rules:**
- AI assistants are always shown as "online" when they are active and handling conversations — their presence reflects their operational state, not a human availability concept
- Team member presence reflects their last active time: online if active within 5 minutes, away if 5–30 minutes, offline if over 30 minutes
- Do not show presence on customer avatars — customers are external and their presence is not tracked in Chatverce

### Message States (Full lifecycle)

`Sending` → `Sent` → `Delivered` → `Read` → `Failed (with retry)`

Failed messages show an inline retry button directly on the failed message. The retry button triggers an immediate resend attempt. If the second attempt also fails, show an inline Alert within the thread.

## Components Used

- **Avatar** (with presence dot) — team member and assistant presence
- **Badge** — unread count on conversation list items
- **Spinner / animated dots** — typing indicator animation
- **List** — conversation list with live state updates (unread badge, typing indicator preview)
- **Conversation thread** — message-level state icons (sent, delivered, read, failed)

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

Real-time updates delivered via **WebSocket** (or Server-Sent Events for one-way updates). Use a persistent connection maintained at the app level (not per-component). Reconnect automatically on disconnect with exponential backoff.

Typing indicator in the conversation thread: positioned as a full-width row at the bottom of the message list (below all messages), matching the layout of a standard incoming message. The animated dots sit where the message content would be.

New messages scroll the thread to the bottom automatically if the user was already at the bottom. If the user has scrolled up (reading history), do not auto-scroll — show a floating *"New message ↓"* chip instead.

### iOS (Expo + NativeWind)

**Foreground:** WebSocket for live updates. All real-time state updates (typing, read receipts, presence) are delivered through the WebSocket connection.

**Background:** Push notifications via `expo-notifications` when a new message arrives and the app is not foregrounded. The push notification opens the relevant conversation via deep link.

The *"New message ↓"* chip on auto-scroll override: a floating pill button pinned to the bottom of the `ScrollView`, above the input. Tapping it scrolls to the bottom and dismisses the chip.

### Android (Expo + NativeWind)

Same WebSocket foreground + push background pattern. Android can be more aggressive about killing background processes — use a foreground service notification (via `expo-notifications` background task) for critical message delivery when the app is backgrounded.

Typing indicator and message states: identical to iOS implementation.

## Do / Don't

| Do | Don't |
|----|-------|
| Time out the typing indicator at 10 seconds with no new typing event | Leave the typing indicator active indefinitely after typing stops |
| Show read receipts only when the platform provides them | Mock or estimate read receipt data when it is unavailable |
| Auto-scroll to new messages only when the user is already at the bottom | Force-scroll the user up from their history reading position |
| Show the "New message ↓" chip when the user is scrolled up and a new message arrives | Let new messages silently arrive without any notification when the user is scrolled up |
| Reconnect WebSocket automatically with exponential backoff | Leave the connection dead and show stale data without alerting the user |
| Mark AI assistants as "online" when they are active | Leave AI assistant presence as "offline" when they are actively handling conversations |

## Chatverce-Specific

**Multi-channel presence:** A user managing WhatsApp and Telegram simultaneously sees both channels' conversations in the unified inbox. Presence indicators on conversation list rows include the channel badge (see `channel-switching` pattern) — the user can see at a glance which channel a conversation is on, who is typing, and what the message state is.

**AI assistant always "online":** An active Chatverce assistant has a green presence dot. If the assistant is paused or in Draft status, the dot is removed (offline state). If the assistant has an error, the dot changes to amber (away). This gives users a quick health check: green = working, amber = needs attention, no dot = not active.

**Typing indicator in AI-handled conversations:** When the AI assistant is composing a response, show the typing indicator (animated dots) in the assistant's position in the conversation thread. This sets the customer's expectation: something is happening. The typing indicator disappears when the first token of the AI response streams in.

**Message failure in multi-channel:** If a message fails to deliver on WhatsApp (e.g., the customer has not opted in), show the failure state with a channel-specific error: *"Message not delivered on WhatsApp. The customer may not have accepted your business account."* Not a generic "Failed."
