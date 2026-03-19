
# Alert

**Gallery reference:** https://component.gallery/components/alert/
**Aliases:** Banner, Inline notification, System message, Callout, Notice

## When to Use

Use Alert for persistent conditions that the user should be aware of while using the current screen: a billing account nearing its limit, an agent configuration error that is blocking publishing, a form-level validation summary, a system maintenance notice. Alert stays visible because the condition it describes is ongoing — unlike Toast, which confirms a one-time event and disappears.

## When NOT to Use

Do not use Alert for transient feedback on completed actions — that is Toast's role. Do not use Alert for individual field-level validation — use helper text below the input. Do not use Alert inside a Modal unnecessarily — if the modal is about the issue, the dialog itself communicates it. Do not use Alert to celebrate success — a toast or inline confirmation is sufficient and less heavy.

## Anatomy

- **Container:** Left-colored border (4px, variant color), `cv-bg-surface` or variant-tinted subtle background, `cv-radius-md`
- **Icon:** 20px, colored by variant (info=teal, warning=amber, error=coral, success=green)
- **Title (optional):** Body weight 500, `cv-text-primary`
- **Body text:** Body, `cv-text-primary` or `cv-text-secondary`
- **Action link (optional):** `cv-text-link`, inline or below body
- **Dismiss button (optional):** X icon, `cv-text-tertiary`, top-right

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Info | Teal icon, `cv-bg-accent-subtle` background | Neutral system information, tips, AI status |
| Warning | Amber icon, amber-tinted background | Non-critical issue needing attention |
| Error | Coral icon, `cv-bg-destructive-subtle` background | Configuration error, failed operation, blocking issue |
| Success | Green icon, green-tinted background | Persistent success state (agent is live, payment confirmed) |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | Full alert visible | Clear, persistent awareness |
| Hover action (web) | Action link underlines | Readable, clear interaction affordance |
| Dismissed | Slides up / fades out | User acknowledged and moved on |
| Collapsed (optional) | Title only, expand arrow | Saves space while preserving awareness |

## Chatverce Styling

- Background: variant-specific subtle tint (info: `cv-bg-accent-subtle`, error: `cv-bg-destructive-subtle`, warning: amber-10%, success: green-10%)
- Left border: 4px solid, variant color (`cv-status-info` / `cv-status-warning` / `cv-status-error` / `cv-status-success`)
- Border radius: `cv-radius-md` (right corners only if using left border, or all corners if no side border)
- Text: `cv-text-primary` for title and body
- Icon size: 20px, variant color
- Padding: 12px vertical, 16px horizontal

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Alert>` + `<AlertTitle>` + `<AlertDescription>`. Override the variant styles with cv-token classes. The alert renders inline in the page flow — position it at the top of the relevant content section (above the form, at the top of the settings panel).

### iOS (Expo + NativeWind)
Render as a styled `View` with a left border and subtle background. Use `View` with `borderLeftWidth={4}` and the variant color as `borderLeftColor`. Place it at the top of the screen or section it relates to.

### Android (Expo + NativeWind)
Same approach as iOS. Android does not have a native equivalent for in-page banners — use the same custom View pattern. Ensure the background tint is visible in both light and dark mode.

## Accessibility

- Web: Use `role="alert"` for error alerts (browser announces immediately). Use `role="status"` for info and success (polite announcement). Dismissible alerts must have a close button with `aria-label="Dismiss alert"`.
- Mobile: `accessibilityLiveRegion="assertive"` for errors, `"polite"` for info/success. If the alert is dismissible, the close button must have `accessibilityRole="button"` and `accessibilityLabel`.

## Code Examples

### Web

```tsx
import { Alert, AlertDescription, AlertTitle } from "@/components/ui/alert";
import { AlertCircle, Info, CheckCircle, AlertTriangle } from "lucide-react";
import { X } from "lucide-react";

// Error alert (blocking condition)
<Alert
  role="alert"
  className="border-l-4 border-l-[--cv-status-error] bg-[--cv-bg-destructive-subtle] rounded-[--cv-radius-md] flex items-start gap-3"
>
  <AlertCircle className="h-5 w-5 text-[--cv-status-error] mt-0.5 shrink-0" />
  <div className="flex-1">
    <AlertTitle className="text-[--cv-text-primary] font-medium text-sm mb-1">
      Agent cannot be published
    </AlertTitle>
    <AlertDescription className="text-[--cv-text-secondary] text-sm">
      Your agent is missing a trigger node.{" "}
      <a href="/builder" className="text-[--cv-text-link] underline">
        Open builder to fix this.
      </a>
    </AlertDescription>
  </div>
</Alert>

// Info alert (dismissible)
function InfoAlert({ onDismiss }) {
  return (
    <Alert
      role="status"
      className="border-l-4 border-l-[--cv-status-info] bg-[--cv-bg-accent-subtle] rounded-[--cv-radius-md] flex items-start gap-3 relative"
    >
      <Info className="h-5 w-5 text-[--cv-status-info] mt-0.5 shrink-0" />
      <div className="flex-1">
        <AlertDescription className="text-[--cv-text-primary] text-sm">
          Your trial ends in 5 days.{" "}
          <a href="/billing" className="text-[--cv-text-link] underline">Upgrade now</a>{" "}
          to keep your agents running.
        </AlertDescription>
      </div>
      <button
        onClick={onDismiss}
        className="p-1 text-[--cv-text-tertiary] hover:text-[--cv-text-primary]"
        aria-label="Dismiss alert"
      >
        <X size={16} />
      </button>
    </Alert>
  );
}
```

### Mobile

```tsx
import { View, Text, Pressable } from "react-native";
import { AlertCircle, Info } from "lucide-react-native";

// Error alert
<View
  className="flex-row items-start gap-3 rounded-[--cv-radius-md] p-4 mb-4"
  style={{ borderLeftWidth: 4, borderLeftColor: "var(--cv-status-error)", backgroundColor: "var(--cv-bg-destructive-subtle)" }}
  accessibilityLiveRegion="assertive"
  accessibilityRole="alert"
>
  <AlertCircle size={20} color="var(--cv-status-error)" />
  <View className="flex-1">
    <Text className="text-[--cv-text-primary] font-medium text-sm mb-1">
      Agent cannot be published
    </Text>
    <Text className="text-[--cv-text-secondary] text-sm">
      Add a trigger node to continue.
    </Text>
  </View>
</View>

// Info alert (dismissible)
function InfoAlertMobile({ message, onDismiss }) {
  return (
    <View
      className="flex-row items-start gap-3 rounded-[--cv-radius-md] p-4 mb-4"
      style={{ borderLeftWidth: 4, borderLeftColor: "var(--cv-status-info)", backgroundColor: "var(--cv-bg-accent-subtle)" }}
      accessibilityLiveRegion="polite"
    >
      <Info size={20} color="var(--cv-status-info)" />
      <Text className="flex-1 text-[--cv-text-primary] text-sm">{message}</Text>
      {onDismiss && (
        <Pressable onPress={onDismiss} accessibilityRole="button" accessibilityLabel="Dismiss alert">
          <Text className="text-[--cv-text-tertiary] text-lg leading-none">×</Text>
        </Pressable>
      )}
    </View>
  );
}
```

## Anti-Patterns

- Never use Alert for individual field-level errors — put validation messages below the input field.
- Never use Alert to replace a proper empty state — if there is no data, use the Empty State component.
- Never stack more than 2 alerts on one screen — if there are multiple issues, summarize them in one alert.
- Never make a non-dismissible alert for low-priority information — respect the user's ability to acknowledge and move on.
- Never use the Error variant for warnings — amber is for warning, coral is for error. The distinction matters for user perception of severity.
