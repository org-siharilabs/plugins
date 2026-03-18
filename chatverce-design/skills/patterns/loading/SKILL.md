---
name: loading
description: Time-sensitive feedback patterns that keep users informed without interrupting flow
user_invocable: false
---

# Loading

## What the User Is Doing

The user is waiting. They triggered an action — navigating to a page, submitting a form, fetching data — and the system has not responded yet. They need to know the system received their input and is working. Without feedback, users assume the product is broken.

## Emotional Intention

**"I know what's happening."** The user should never feel abandoned or confused. Every wait state communicates: the system is alive, your request was received, and here is roughly how long this will take. Uncertainty breeds distrust; honest loading states build it.

Reference: [design-ethos] — "Trustworthy systems tell you what they're doing."

## How It Works

Loading states are tiered by duration. The goal is to match the visual weight of the indicator to the user's perceived wait.

| Duration | Pattern | Rationale |
|----------|---------|-----------|
| 0–300ms | No indicator | Below perception threshold. Adding an indicator causes visual flicker. |
| 300ms–2s | Skeleton screen | Users scan content shape; skeleton anchors their eye before data arrives. |
| 2s–10s | Skeleton + contextual text | "Loading conversations…" — users need language when patience is required. |
| 10s+ | Progress bar (if deterministic) or spinner + text | Progress signals movement. Text prevents abandonment. |
| AI processing | Teal pulse on AI indicator | Communicates AI activity specifically — distinct from generic loading. |

**Decision flow:**

1. Is the layout known? Use a skeleton matching the content shape.
2. Is the layout unknown (e.g., raw API response)? Use a centered spinner with text.
3. Is AI involved? Replace or supplement the standard indicator with the teal pulse.
4. Is the operation over 10 seconds? Show a progress bar if progress is measurable; otherwise use spinner + elapsed time.
5. Can the result be optimistically rendered? Do it — remove the loading state entirely.

**Optimistic UI:** Whenever an action is highly likely to succeed (archive conversation, toggle setting), update the UI immediately and revert only on error. Do not make users wait for server confirmation on routine actions.

## Components Used

- **Skeleton** — shaped placeholder for known content areas (conversation list rows, card grids, profile sections)
- **Spinner** — used only when the content shape is unknown or as a compact inline indicator
- **Progress bar** — deterministic or bounded long-running operations
- **AI working indicator** — teal pulse, exclusively for AI processing states
- **Toast** — used to confirm completion of a background operation when the user has navigated away

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

Implement skeleton via CSS `animate-pulse` with `motion-safe:` prefix. Skeleton shapes must match the real content's dimensions as closely as possible — use the same padding, border-radius, and grid layout. Do not use a generic gray rectangle over a complex layout.

```html
<!-- Match actual content layout -->
<div class="motion-safe:animate-pulse space-y-3">
  <div class="h-4 bg-cv-bg-surface-sunken rounded w-3/4"></div>
  <div class="h-4 bg-cv-bg-surface-sunken rounded w-1/2"></div>
</div>
```

For AI loading, apply the teal pulse using `cv-bg-accent-subtle` with an opacity keyframe. Stop the animation the moment output begins rendering.

### iOS (Expo + NativeWind)

Use `react-native-reanimated` for shimmer animations. Check `useReducedMotion()` — when true, replace shimmer with a static muted `cv-bg-surface-sunken` fill.

```typescript
import { useReducedMotion } from 'react-native-reanimated';
const reducedMotion = useReducedMotion();
// Shimmer only when motion is acceptable
```

Skeleton shapes must respect the physical row height of the iOS list components being loaded.

### Android (Expo + NativeWind)

Same Reanimated shimmer pattern as iOS. Follow Material-adjacent conventions for placement: shimmer rows typically sit within the same card/list container as the final content. Respect `AccessibilityInfo.isReduceMotionEnabled()` as a fallback check.

## Do / Don't

| Do | Don't |
|----|-------|
| Use skeleton shapes that match the real content layout | Use a generic spinner for a page with a known layout structure |
| Apply optimistic UI for high-confidence actions (toggle, archive) | Block the entire screen with a loading overlay for background operations |
| Show contextual text after 2s ("Loading conversations…") | Loop the AI pulse indefinitely — stop it when output begins |
| Wrap all animations in `motion-safe:` (web) or check `useReducedMotion()` (mobile) | Introduce loading states under 300ms — they cause more confusion than they prevent |
| Match skeleton border-radius and spacing to the real component | Use a spinner when a progress bar is possible (deterministic operations) |

## Chatverce-Specific

**Conversation list:** Skeleton rows should have a circular avatar placeholder, a wide text line, and a narrow timestamp line — matching the real conversation row layout exactly.

**AI assistant processing:** The AI working indicator (teal pulse on the assistant avatar or response area) is the canonical signal that an AI is thinking. It is not a generic spinner. Use `cv-bg-accent-subtle` + opacity loop. Stop immediately when the first token of output streams in.

**Channel sync:** When a WhatsApp or Telegram channel is syncing historical messages, show a progress bar (if the API provides a count) or a skeleton list. Display "Syncing 240 messages from WhatsApp…" not a bare spinner.

**Dashboard metrics:** Skeleton cards matching metric card dimensions. Reveal with a staggered fade (each card 50ms after the previous) to feel responsive rather than a sudden full-page pop.
