---
name: ai-working-indicator
description: Teal pulsing animation with a contextual label indicating that an AI process is actively running
user_invocable: false
---

# AI Working Indicator

**Gallery reference:** https://component.gallery/components/loader/
**Aliases:** AI thinking indicator, Processing indicator, Agent working state

## When to Use

Use AI Working Indicator whenever a Chatverce AI agent is actively processing — thinking through a response, setting up a new connection, or executing a background task. It is the primary feedback mechanism between user action and AI response. Always display a contextual label so users understand what specifically is happening, not just that something is happening.

## When NOT to Use

Do not use AI Working Indicator for standard HTTP loading states (fetching a list, saving a form) — use Spinner or Skeleton instead. Do not use it when the AI is idle. Do not show it continuously without ever resolving — it must always transition to a success, error, or result state.

## Anatomy

- **Pulse dot:** Small circle, 8px, `cv-bg-accent` (teal). Animate with a `ping` pulse effect.
- **Label:** Contextual text to the right of the dot. Examples: "Thinking…", "Setting things up…", "Connecting to WhatsApp…".
- **Container:** `role="status"` + `aria-live="polite"`. Updates text when the phase changes.

## Variants

| Variant | Description |
|---------|-------------|
| Inline | Dot + label in a row. Used inside conversation bubbles or panels alongside running agents. |
| Full panel | Centered dot + label in a dedicated loading surface (e.g., a connecting screen). |

## Chatverce Styling

- Dot color: `cv-bg-accent` — teal only. No other brand colors.
- Dot size: 8px (`w-2 h-2`)
- Pulse animation: `animate-ping` (Tailwind) at default speed (1s). **Only allowed decorative motion in Chatverce.**
- Label: `cv-text-sm`, `cv-text-secondary`
- Spacing: `cv-space-2` between dot and label
- Respect `prefers-reduced-motion`: disable `animate-ping`, show static dot

**Contextual labels by phase:**

| Phase | Label |
|-------|-------|
| Agent generating response | "Thinking…" |
| Onboarding / first setup | "Setting things up…" |
| Channel connection | "Connecting to WhatsApp…" / "Connecting to Telegram…" |
| Data processing | "Processing your request…" |
| Generic / unknown | "Working…" |

## Platform Considerations

Web: `animate-ping` Tailwind class on the dot. Wrap the entire indicator in `role="status" aria-live="polite"`. Mobile: Replace `animate-ping` with `Animated.loop` using a scale + opacity pulse. Trigger `Haptics.notificationAsync(NotificationFeedbackType.Success)` from `expo-haptics` when the indicator resolves (AI finishes).

## Accessibility

- Container must have `role="status"` and `aria-live="polite"` so screen readers announce the active state.
- When the phase label changes (e.g., from "Connecting…" to "Setting up…"), the updated text inside the `aria-live` region is announced automatically.
- Respect `prefers-reduced-motion`: disable the pulse animation and show a static dot. The text label still communicates the state.
- Do not use `aria-live="assertive"` — this is background activity, not an urgent alert.

## Code Example

```tsx
function AiWorkingIndicator({ label = "Thinking…" }) {
  return (
    <div
      role="status"
      aria-live="polite"
      className="flex items-center gap-2"
    >
      <span className="relative flex h-2 w-2">
        <span className="animate-ping absolute inline-flex h-full w-full rounded-full bg-[--cv-bg-accent] opacity-75 motion-reduce:animate-none" />
        <span className="relative inline-flex rounded-full h-2 w-2 bg-[--cv-bg-accent]" />
      </span>
      <span className="text-sm text-[--cv-text-secondary]">{label}</span>
    </div>
  );
}

// Usage
<AiWorkingIndicator label="Connecting to WhatsApp…" />
<AiWorkingIndicator label="Thinking…" />
<AiWorkingIndicator label="Setting things up…" />
```

## Anti-Patterns

- Never show a generic "Loading…" label without AI context — use Spinner for non-AI loads.
- Never animate with anything other than the teal pulse — this is the only decorative motion token in the design system.
- Never leave the indicator running indefinitely — always resolve to a success or error state with a timeout.
- Never use `aria-live="assertive"` — AI background processes should not interrupt the user.
