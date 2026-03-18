---
name: button
description: Primary interactive trigger for all user-initiated actions in Chatverce
user_invocable: false
---

# Button

**Gallery reference:** https://component.gallery/components/button/
**Aliases:** CTA, Action button, Submit button, Trigger, Call to action

## When to Use

Use Button whenever the user needs to trigger an action: submitting a form, confirming a dialog, saving settings, creating a new agent, sending a message. Button is the single most important interactive element in the system — its visual weight communicates the hierarchy of actions available on any screen.

## When NOT to Use

Do not use Button for navigation between pages or screens — use Link or the Navigation component. Do not use multiple Primary buttons on one screen; if you find yourself reaching for two primaries, one of them is wrong. Do not use Button for toggle states that persist — use Toggle instead.

## Anatomy

- **Container:** Background fill (variant-dependent), cv-radius-md (8px), min-height 44px
- **Label:** Body weight 500 (Medium), cv-text-on-accent (primary) or cv-text-primary (secondary/ghost)
- **Leading icon (optional):** 16px, same color as label
- **Loading indicator:** Spinner replacing label during async operations
- **Focus ring:** 2px cv-border-accent offset 2px (web), platform focus ring (mobile)

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Primary | Teal fill (cv-bg-accent), white label | One per screen. The single most important action. |
| Secondary | Transparent bg, cv-border-default border, cv-text-primary label | Supporting actions alongside a Primary. |
| Ghost | No bg, no border, cv-text-primary label | Tertiary actions, cancel, "back" in flows. |
| Destructive | cv-bg-destructive fill, cv-text-on-destructive label | Delete, remove, irreversible actions only. |
| Icon-only | Square, icon centered, no label | Toolbar actions where space is constrained. Always include aria-label. |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | Resting variant fill | Ready, stable, inviting |
| Hover (web) | cv-bg-accent-hover (primary), cv-bg-surface-elevated (secondary) | Responsive, alive |
| Pressed/Active | 90% brightness of hover state | Tactile confirmation |
| Focus | 2px cv-border-accent focus ring | Accessible, keyboard-navigable |
| Disabled | cv-text-tertiary, cv-bg-surface-sunken, no pointer events | Unavailable, not interactive |
| Loading | Spinner replaces label, pointer-events: none | Processing — user action received |

## Chatverce Styling

- Background: `cv-bg-accent` (primary), transparent (secondary/ghost), `cv-bg-destructive` (destructive)
- Text: `cv-text-on-accent` (primary/destructive), `cv-text-primary` (secondary/ghost)
- Border: none (primary/ghost), `cv-border-default` (secondary)
- Border radius: `cv-radius-md`
- Shadow: none at rest, `cv-shadow-sm` on hover for secondary
- Min height: 44px (mobile), 40px (web at comfortable density)
- Horizontal padding: 16px (default), 12px (compact/icon-adjacent)
- Transition: 150ms ease for background and shadow

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Button>` as the base. Override variants with `cv-*` token classes. Ensure `type="button"` on non-submit buttons inside forms to prevent accidental form submission.

### iOS (Expo + NativeWind)
Use `Pressable` with NativeWind classes. Trigger `Haptics.impactAsync(ImpactFeedbackStyle.Medium)` from `expo-haptics` on press. Ensure `accessibilityRole="button"` and `accessibilityLabel`.

### Android (Expo + NativeWind)
Same as iOS. Use `Haptics.impactAsync(ImpactFeedbackStyle.Light)` — Android haptic feedback should be lighter than iOS. Ensure the touch target is at least 48dp (Android material guideline).

## Accessibility

- Web: native `<button>` element, `aria-label` for icon-only, `aria-disabled` (not `disabled` attribute) when the button is unavailable but needs to be discoverable
- Mobile: `accessibilityRole="button"`, `accessibilityLabel`, `accessibilityState={{ disabled: bool, busy: bool }}` for loading

## Code Examples

### Web

```tsx
import { Button } from "@/components/ui/button";
import { Loader2 } from "lucide-react";

// Primary
<Button className="bg-[--cv-bg-accent] text-[--cv-text-on-accent] hover:bg-[--cv-bg-accent-hover] rounded-[--cv-radius-md] min-h-[40px]">
  Create Agent
</Button>

// Secondary
<Button variant="outline" className="border-[--cv-border-default] text-[--cv-text-primary] hover:bg-[--cv-bg-surface-elevated] rounded-[--cv-radius-md]">
  Cancel
</Button>

// Destructive
<Button className="bg-[--cv-bg-destructive] text-[--cv-text-on-destructive] rounded-[--cv-radius-md]">
  Delete Agent
</Button>

// Loading
<Button disabled className="bg-[--cv-bg-accent] text-[--cv-text-on-accent]">
  <Loader2 className="mr-2 h-4 w-4 animate-spin" />
  Saving…
</Button>
```

### Mobile

```tsx
import { Pressable, Text, ActivityIndicator } from "react-native";
import * as Haptics from "expo-haptics";

function CvButton({ label, onPress, variant = "primary", loading = false, disabled = false }) {
  const handlePress = async () => {
    await Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium);
    onPress?.();
  };

  const containerClass = {
    primary: "bg-[--cv-bg-accent] rounded-[--cv-radius-md] min-h-[44px] px-4 items-center justify-center",
    secondary: "border border-[--cv-border-default] rounded-[--cv-radius-md] min-h-[44px] px-4 items-center justify-center",
    ghost: "min-h-[44px] px-4 items-center justify-center",
    destructive: "bg-[--cv-bg-destructive] rounded-[--cv-radius-md] min-h-[44px] px-4 items-center justify-center",
  }[variant];

  const textClass = {
    primary: "text-[--cv-text-on-accent] font-medium text-base",
    secondary: "text-[--cv-text-primary] font-medium text-base",
    ghost: "text-[--cv-text-primary] font-medium text-base",
    destructive: "text-[--cv-text-on-destructive] font-medium text-base",
  }[variant];

  return (
    <Pressable
      className={containerClass}
      onPress={handlePress}
      disabled={disabled || loading}
      accessibilityRole="button"
      accessibilityLabel={label}
      accessibilityState={{ disabled: disabled || loading, busy: loading }}
    >
      {loading
        ? <ActivityIndicator color="var(--cv-text-on-accent)" />
        : <Text className={textClass}>{label}</Text>
      }
    </Pressable>
  );
}
```

## Anti-Patterns

- Never place two Primary buttons side by side. One screen = one primary action.
- Never use color alone to distinguish button purpose — ensure variant (outline, fill, ghost) carries the meaning.
- Never disable a button without explaining why the action is unavailable (use a tooltip or helper text near the form).
- Never use Button for inline navigation — that is Link's job.
- Never skip the loading state on async actions. A button that appears stuck destroys trust.
