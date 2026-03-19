---
name: toast
description: Transient notification that confirms an action outcome without interrupting workflow
user_invocable: false
---

# Toast

**Gallery reference:** https://component.gallery/components/toast/
**Aliases:** Snackbar, Notification, Flash message, Alert toast, Banner notification

## When to Use

Use Toast for transient feedback about a completed action: "Agent saved", "Invitation sent", "Copy failed". Toast confirms that something happened without demanding the user's focus. It is appropriate when the message is not critical — missing it would not leave the user confused about the system state.

## When NOT to Use

Do not use Toast for persistent conditions that require action — use Alert. Do not use Toast for destructive action confirmation — use a Modal. Do not stack more than 2 toasts. Do not use Toast for loading states — use Skeleton or Spinner inline. Do not use Toast for error conditions the user cannot recover from — those belong in an Alert or dedicated error state.

## Anatomy

- **Container:** `cv-bg-surface-elevated`, `cv-shadow-md`, `cv-radius-md`, min-width 280px, max-width 400px
- **Icon (optional):** 20px, colored by variant (success=green, error=coral, warning=amber, info=teal)
- **Title:** Body weight 500, `cv-text-primary`
- **Description (optional):** Caption, `cv-text-secondary`, max 2 lines
- **Action button (optional):** Ghost/link style, `cv-text-link` or `cv-text-accent`
- **Close button:** X icon, `cv-text-tertiary`
- **Progress bar (optional):** Thin bottom line showing remaining display time

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Success | Green icon, `cv-status-success` | Action completed successfully |
| Error | Coral/red icon, `cv-status-error` | Action failed — user can retry |
| Warning | Amber icon, `cv-status-warning` | Action completed with caveats |
| Info | Teal icon, `cv-status-info` | Neutral information, AI activity updates |
| With Action | Any variant + actionable button | Undo, retry, view details |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Entering | Slide in from edge, 250ms ease-out | Smooth, non-startling arrival |
| Visible | Static, auto-dismiss timer running | Informative, present |
| Hover (web) | Timer pauses on hover | Respectful — gives time to read |
| Action hover | Underline on action text | Clearly interactive |
| Exiting | Slide out + fade, 150ms ease-in | Unobtrusive departure |
| Dismissed manually | Immediate slide out | Responsive to user control |

## Chatverce Styling

- Background: `cv-bg-surface-elevated`
- Shadow: `cv-shadow-md`
- Border radius: `cv-radius-md`
- Text: `cv-text-primary` (title), `cv-text-secondary` (description)
- Icon colors: `cv-status-success` / `cv-status-error` / `cv-status-warning` / `cv-status-info`
- Auto-dismiss: 4000ms (default), 8000ms (with-action variant — user needs time to read + act)
- Max stacked: 2 toasts visible simultaneously. Queue additional ones.
- z-index: 100 (above modals)

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use `sonner` (the toast library that shadcn/ui recommends) or shadcn/ui's built-in `<Toaster>` + `toast()` utility. Position: top-right (desktop), top-center (mobile breakpoint). Add the `<Toaster>` once in the root layout. Call `toast.success()`, `toast.error()`, etc. from anywhere in the app.

### iOS (Expo + NativeWind)
Position at the top, below the status bar safe area. Use `react-native-toast-message` or a custom animated View with `Animated.spring`. Respect `useSafeAreaInsets()` from `react-native-safe-area-context` for top offset.

### Android (Expo + NativeWind)
Position at the bottom, above the navigation bar safe area — this matches Android Material design convention for Snackbars. Use `useSafeAreaInsets()` for bottom offset.

## Accessibility

- Web: The toast container must have `aria-live="polite"` (info/success/warning) or `aria-live="assertive"` (error). Use `role="status"` for polite toasts and `role="alert"` for error toasts. The close button must have `aria-label="Dismiss notification"`.
- Mobile: `accessibilityLiveRegion="polite"` on the toast container. VoiceOver/TalkBack will announce the toast text automatically. Ensure the action button has `accessibilityRole="button"` and `accessibilityLabel`.

## Code Examples

### Web

```tsx
// In root layout:
import { Toaster } from "sonner";
<Toaster
  position="top-right"
  toastOptions={{
    classNames: {
      toast: "bg-[--cv-bg-surface-elevated] shadow-[--cv-shadow-md] rounded-[--cv-radius-md] border-0",
      title: "text-[--cv-text-primary] font-medium text-sm",
      description: "text-[--cv-text-secondary] text-xs",
      actionButton: "text-[--cv-text-link] text-sm font-medium",
      closeButton: "text-[--cv-text-tertiary]",
    },
  }}
/>

// Triggering toasts throughout the app:
import { toast } from "sonner";

// Success
toast.success("Agent saved", {
  description: "Your changes are live.",
  duration: 4000,
});

// Error with action
toast.error("Failed to send invitation", {
  description: "Check the email address and try again.",
  duration: 8000,
  action: {
    label: "Retry",
    onClick: () => resendInvitation(),
  },
});

// Info (AI activity)
toast.info("Agent is processing", {
  description: "This usually takes under 30 seconds.",
  duration: 4000,
});
```

### Mobile

```tsx
import Toast from "react-native-toast-message";
import { useSafeAreaInsets } from "react-native-safe-area-context";

// In root layout — iOS: top, Android: bottom
function ToastConfig() {
  const insets = useSafeAreaInsets();
  return (
    <Toast
      config={{
        success: ({ text1, text2 }) => (
          <View
            className="mx-4 bg-[--cv-bg-surface-elevated] rounded-[--cv-radius-md] p-4 flex-row items-start gap-3"
            style={{ shadowColor: "#000", shadowOpacity: 0.1, shadowRadius: 8, elevation: 4 }}
            accessibilityLiveRegion="polite"
          >
            <CheckCircle size={20} color="var(--cv-status-success)" />
            <View className="flex-1">
              <Text className="text-[--cv-text-primary] font-medium text-sm">{text1}</Text>
              {text2 && <Text className="text-[--cv-text-secondary] text-xs mt-0.5">{text2}</Text>}
            </View>
          </View>
        ),
      }}
      topOffset={insets.top + 8}
      bottomOffset={insets.bottom + 8}
    />
  );
}

// Triggering:
Toast.show({ type: "success", text1: "Agent saved", text2: "Your changes are live." });
Toast.show({ type: "error", text1: "Failed to save", text2: "Check your connection." });
```

## Anti-Patterns

- Never show more than 2 toasts simultaneously — queue additional ones.
- Never use Toast for critical errors that leave the system in an unclear state — use an Alert or inline error.
- Never use Toast for success on a destructive action ("Agent deleted") without an Undo action — give users a way back.
- Never auto-dismiss a toast with an action button in fewer than 8 seconds — the user needs time to decide.
- Never use `aria-live="assertive"` for non-error toasts — it interrupts screen readers mid-sentence.
