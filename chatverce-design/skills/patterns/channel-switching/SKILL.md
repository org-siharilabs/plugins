---
name: channel-switching
description: Unified inbox pattern that brings all channels together without collapsing their distinct identities
user_invocable: false
---

# Channel Switching

## What the User Is Doing

The user is managing conversations across multiple connected channels — WhatsApp, Telegram, and others — from a single inbox. They need to understand which channel each conversation is on, filter to a specific channel when needed, and switch channels without losing their place. The product's job is to make multi-channel feel like a superpower, not a source of confusion.

## Emotional Intention

**"All my channels, one place."** The user should feel that Chatverce collapses the cognitive overhead of managing multiple messaging platforms into a single, coherent view. Switching between channels should feel as natural as switching between tabs — the user stays in control of their attention.

## How It Works

**Unified inbox (default view):**
- All conversations from all connected channels appear in a single list
- Conversations are sorted by most recent activity (same as a standard inbox)
- Each conversation row includes a **channel indicator badge** — a small colored dot or icon identifying the channel source (WhatsApp, Telegram)
- The channel indicator is the *only* place channel color is used — nowhere else in the interface uses channel-specific colors

**Filtering by channel:**
- A filter control (tabs or segmented control) above the conversation list allows the user to view all channels or a single channel
- "All" is the default and most important view
- Per-channel filters show only conversations from that channel, preserving all other list behaviors (sort, search, unread badge)
- Filter state persists within the session; resets to "All" on fresh load

**Context preservation:**
- Switching the channel filter does not close an open conversation; the conversation panel remains visible
- Scroll position in the conversation list is preserved per filter — switching back to "All" returns to the last scroll position in that view
- Active search query is cleared on channel filter change (different result sets)

**Cross-channel threading:**
- If a customer contacts via both WhatsApp and Telegram, they may appear as two separate contacts (depending on if they are linked) or as a unified contact with two conversation threads
- Where contacts are linked, show both channel badges on the contact avatar
- Do not merge messages from different channels into a single thread — channel context matters for sending replies on the correct platform

## Components Used

- **Channel indicator** — small badge (colored dot + platform icon) on conversation rows and contact avatars
- **Badge** — unread message count per conversation, per channel
- **Tabs** — web filter control for channel selection
- **Segmented control** — mobile filter control for channel selection (or filter chips)
- **List** — conversation list, unified or filtered view

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

Channel filter uses a **tab bar** above the conversation list. Tabs: "All", then one tab per connected channel (e.g., "WhatsApp", "Telegram"). Tabs use the standard tab component with `cv-border-accent` underline on the active tab.

A **sidebar filter** is an alternative for users with 3+ channels: a vertical list of channel names in the left sidebar, each with the channel indicator icon and an unread badge count. The active channel is highlighted with `cv-bg-accent-subtle`.

Keyboard: `←` / `→` navigates between channel tabs when the tab bar is focused.

### iOS (Expo + NativeWind)

**Segmented control** at the top of the conversations list screen. iOS segmented controls support up to 5 segments reliably. For users with more than 4 channels, show "All", the 3 most-used channels, and a "More" segment that opens a channel selector sheet.

**Filter chips** are an alternative for users who prefer horizontal scrollable chips over a segmented control — implement based on the number of channels. For 2–3 channels: segmented control. For 4+ channels: scrollable filter chips.

### Android (Expo + NativeWind)

Same filter chip approach as iOS for 4+ channels. For 2–3 channels, a tab bar at the top of the screen (Material-adjacent) with the channel names.

Android back gesture returns the user to the "All" filter from a per-channel filter, before exiting the app. This mirrors Material navigation: back navigates up the hierarchy, not out of the app.

## Do / Don't

| Do | Don't |
|----|-------|
| Show a channel indicator badge on every conversation row | Use channel colors as background fills or tints on conversation rows |
| Use channel indicator color coding *only* on the channel badge itself | Apply WhatsApp green or Telegram blue anywhere in the main UI outside the indicator |
| Preserve conversation list scroll position per filter | Reset scroll to the top on every channel filter change |
| Default to the "All" view — multi-channel is the product's core value | Default to a single channel and require the user to manually switch to the unified view |
| Show clear channel source on every search result | Return search results without indicating which channel each result is from |
| Allow switching the channel filter without closing an open conversation | Dismiss the open conversation when the user changes the channel filter |

## Chatverce-Specific

**Channel color coding — strict rule:** Channel colors (WhatsApp green `#25D366`, Telegram blue `#2AABEE`) appear *only* on the channel indicator badge. They are never used as background fills, row tints, avatar borders, or any decorative element outside the indicator. Violating this rule corrupts the Chatverce color identity — the product begins to look like a reskin of WhatsApp.

**Channel indicator spec:**
- Size: 16×16px circle
- WhatsApp: `#25D366` fill, white WhatsApp icon inside
- Telegram: `#2AABEE` fill, white paper-plane icon inside
- Position: bottom-right of the contact avatar (overlapping the avatar corner)

**Unread count per channel:** The channel filter tabs/chips display an unread badge count next to the channel name. "All" shows the total unread across all channels. Per-channel tabs show that channel's unread only.

**Replying on the correct channel:** When composing a reply from the unified inbox, the reply is sent on the same channel the incoming message was received on. This is surfaced in the composer: a small channel indicator above the text input reads *"Reply on WhatsApp"*. This prevents the common error of intending to reply on WhatsApp but accidentally sending on Telegram.
