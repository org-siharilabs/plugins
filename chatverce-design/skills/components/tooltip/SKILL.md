---
name: tooltip
description: Brief contextual label that appears on hover or focus to clarify the purpose of an icon or control
user_invocable: false
---

# Tooltip

**Gallery reference:** https://component.gallery/components/tooltip/
**Aliases:** Hint, Helper text, Info popup, Title, Hover label

## When to Use

Use Tooltip to clarify the purpose of icon-only buttons, truncated text labels, or controls whose visual representation alone is ambiguous. Tooltip is appropriate when the hint is 1–10 words and requires no interaction — it is purely informational. Icon buttons without a visible label must always have a Tooltip equivalent (or an `aria-label`).

## When NOT to Use

Do not use Tooltip for content that requires interaction (links, buttons inside the tip) — use Popover instead. Do not use Tooltip for important instructions — users who depend on keyboard or touch will miss hover-only content. Do not use Tooltip on mobile as a primary pattern — on touch screens, hover does not exist; use long-press sparingly and prefer Popover for actionable hints. Do not use Tooltip for error messages — use form validation text instead.

## Anatomy

- **Trigger:** The element the tooltip describes (icon button, truncated label, input with help icon)
- **Container:** Dark background (`cv-bg-surface-elevated` inverted / near-black), `cv-radius-sm`, 8px padding horizontal, 4px vertical
- **Text:** `cv-text-on-accent` equivalent (white/near-white), Caption size (12–13px)
- **Arrow:** Optional small triangle pointing toward the trigger
- **Delay:** 500ms before showing (prevents tooltip storms on fast mouse movements)

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Default | Dark bg, white text, arrow | Standard hint on icon or truncated content |
| Light | cv-bg-surface bg, cv-text-primary text, border | Use on dark backgrounds where dark tooltip would disappear |
| Rich | Supports a title line + body text | Complex controls needing a short description (2 lines max) |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | Tooltip hidden | Non-intrusive, content is primary |
| Hover (web) | 500ms delay, then fade in 100ms | Helpful, not jarring |
| Focus (web) | Shows immediately on keyboard focus | Keyboard-accessible, instant |
| Active/Pressed | Tooltip hides | Action is taken, hint no longer needed |
| Touch (mobile) | Long-press shows for 2s then hides | Accessible on touch, but consider Popover |
| Disabled trigger | Tooltip still shows on hover/focus | Explains why something is disabled |

## Chatverce Styling

- Background: `#1a1f2e` (near-black, dark-mode-like regardless of theme) or `cv-bg-surface-elevated` in dark mode
- Text: white (`#FFFFFF`) or `cv-text-on-accent` equivalent
- Border radius: `cv-radius-sm` (4px)
- Padding: `4px 8px`
- Font size: 12px (Caption), weight 400
- Max width: 200px — wrap to second line beyond this
- Shadow: `cv-shadow-sm` for light variant
- Delay: 500ms show, 100ms hide

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Tooltip>` (built on Radix UI). Set `delayDuration={500}`. Radix Tooltip handles positioning, overflow clipping, and ARIA automatically. The trigger element gets `aria-describedby` pointing to the tooltip content.

### iOS (Expo + NativeWind)
Native iOS has no hover. For icon-only buttons, rely on `accessibilityLabel` and `accessibilityHint` — VoiceOver will announce these. For sighted users who want discovery, implement long-press with a 600ms delay using `onLongPress` on `Pressable`, showing a brief overlay. Seriously consider replacing tooltips with Popover for any hint requiring more than 5 words.

### Android (Expo + NativeWind)
Same as iOS. Android TalkBack uses `accessibilityLabel`. Long-press tooltip is acceptable but use sparingly. Prefer Popover for any interactive or multi-word hint.

## Accessibility

- Web: `role="tooltip"` on the tooltip content element. The trigger must have `aria-describedby` pointing to the tooltip's `id`. Never rely on Tooltip alone for critical information — the control must be understandable without it.
- Mobile: Do not use Tooltip as the sole accessibility mechanism. `accessibilityLabel` on the trigger is mandatory. `accessibilityHint` can carry the same content as a tooltip for screen reader users.

## Code Examples

### Web

```tsx
import {
  Tooltip, TooltipContent, TooltipProvider, TooltipTrigger,
} from "@/components/ui/tooltip";
import { Settings } from "lucide-react";

// Icon button with tooltip
<TooltipProvider delayDuration={500}>
  <Tooltip>
    <TooltipTrigger asChild>
      <button
        className="p-2 rounded-[--cv-radius-sm] hover:bg-[--cv-bg-surface-elevated] text-[--cv-text-secondary]"
        aria-label="Agent settings"
      >
        <Settings size={20} />
      </button>
    </TooltipTrigger>
    <TooltipContent
      className="bg-[#1a1f2e] text-white text-xs rounded-[--cv-radius-sm] px-2 py-1 max-w-[200px]"
      sideOffset={4}
    >
      Agent settings
    </TooltipContent>
  </Tooltip>
</TooltipProvider>

// Tooltip on disabled button (still shows even when disabled)
<TooltipProvider delayDuration={500}>
  <Tooltip>
    <TooltipTrigger asChild>
      <span tabIndex={0}>  {/* span wrapper allows focus on disabled button */}
        <button
          disabled
          className="bg-[--cv-bg-accent] text-[--cv-text-on-accent] opacity-50 cursor-not-allowed px-4 py-2 rounded-[--cv-radius-md]"
          aria-disabled="true"
        >
          Publish
        </button>
      </span>
    </TooltipTrigger>
    <TooltipContent className="bg-[#1a1f2e] text-white text-xs rounded-[--cv-radius-sm] px-2 py-1">
      Add at least one trigger node to publish
    </TooltipContent>
  </Tooltip>
</TooltipProvider>
```

### Mobile

```tsx
import { Pressable, View, Text } from "react-native";
import { useState } from "react";
import { Settings } from "lucide-react-native";

// Mobile: long-press reveals brief tooltip overlay
function IconButtonWithHint({ label, hint, onPress }) {
  const [showHint, setShowHint] = useState(false);

  return (
    <View className="relative">
      <Pressable
        className="p-3 rounded-[--cv-radius-sm]"
        onPress={onPress}
        onLongPress={() => {
          setShowHint(true);
          setTimeout(() => setShowHint(false), 2000);
        }}
        accessibilityRole="button"
        accessibilityLabel={label}
        accessibilityHint={hint}
      >
        <Settings size={20} color="var(--cv-text-secondary)" />
      </Pressable>

      {showHint && (
        <View
          className="absolute -top-10 left-1/2 -translate-x-1/2 bg-[#1a1f2e] rounded px-2 py-1 z-50"
          accessibilityElementsHidden={true}
        >
          <Text className="text-white text-xs whitespace-nowrap">{hint || label}</Text>
        </View>
      )}
    </View>
  );
}
```

## Anti-Patterns

- Never put interactive content (buttons, links) inside a Tooltip — use Popover instead.
- Never use Tooltip as the only source of critical information — always ensure the base control is understandable without the hint.
- Never show a Tooltip instantly (0ms delay) on web — it creates visual noise on fast mouse movements.
- Never use Tooltip as a substitute for a proper label on form inputs — use Label and helper text.
- Never rely on Tooltip for mobile UX — hover does not exist on touch. Always pair with `accessibilityLabel`.
