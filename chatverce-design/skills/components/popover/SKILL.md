---
name: popover
description: Click-triggered floating panel for inline contextual content with optional interactivity
user_invocable: false
---

# Popover

**Gallery reference:** https://component.gallery/components/popover/
**Aliases:** Floating panel, Tooltip-like, Inline card, Info card, Context card

## When to Use

Use Popover for contextual content that the user explicitly requests by clicking: a rich node detail in the agent builder, a user profile card on hover/click, filter options in a table header, color picker, or a date range picker inline. Popover is distinct from Tooltip (hover only, no interaction) and Dropdown Menu (actions only, no rich content).

## When NOT to Use

Do not use Popover for a single line of text hint — that is Tooltip. Do not use Popover for a list of actions — that is Dropdown Menu. Do not use Popover for full-screen takeover or significant workflow steps — use Modal or Drawer. Do not use Popover on mobile for anything that requires significant reading or interaction — convert to a bottom sheet.

## Anatomy

- **Trigger:** Any element — button, link, icon, or text
- **Panel:** `cv-bg-surface-elevated`, `cv-shadow-md`, `cv-radius-md`, `cv-border-default` border, 8px padding
- **Arrow (optional):** Small triangle pointing to trigger
- **Header (optional):** Title, `cv-text-primary`, Body 500
- **Content:** Freeform — text, links, form, color picker, etc.
- **Close button (optional):** X icon when panel should persist until explicitly closed

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Info card | Rich text + optional link | User profile card, glossary definition, metric explanation |
| Form popover | Inline form fields | Quick edit, filter controls, inline date range |
| Color picker | Swatch grid + custom hex | Color selection in agent builder |
| Node detail | Agent node info in canvas | Hover/click detail on a node |
| Mobile bottom sheet | Same content, bottom sheet delivery | Any popover on narrow viewports |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Trigger default | No indicator | Neutral |
| Trigger hover/focus | Slight highlight depending on trigger type | Discoverable |
| Open | Panel visible, positioned relative to trigger | Contextual, focused |
| Content hover | Interactive content responds normally | Natural |
| Escape key | Closes panel | Keyboard-native |
| Click outside | Closes panel | Natural dismissal |
| Mobile (small screen) | Renders as bottom sheet | Native, no overflow |

## Chatverce Styling

- Panel bg: `cv-bg-surface-elevated`
- Panel shadow: `cv-shadow-md`
- Panel border: `cv-border-default`
- Panel border radius: `cv-radius-md`
- Padding: 12px (compact), 16px (default)
- Max width: 280px (default), 360px (wide), 480px (form popover)
- z-index: 40 (below modal at 50)
- Positioning: auto-adjust to stay within viewport

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Popover>` (built on Radix UI `@radix-ui/react-popover`). Radix handles positioning (with collision detection), focus management, and Escape key. Set `align` and `side` props to control preferred positioning.

### iOS (Expo + NativeWind)
On screens wider than 768pt (iPad), Popover can render as a floating panel. On iPhone (narrow), always convert to a BottomSheet or Modal. Use a responsive breakpoint check with `useWindowDimensions()` to switch rendering strategy.

### Android (Expo + NativeWind)
Same as iOS — narrow screens use BottomSheet. Android has no native popover equivalent that can be styled.

## Accessibility

- Web: `role="dialog"` on the panel when it contains interactive content. `aria-labelledby` if it has a title. `aria-modal` depends on whether focus is trapped (use `true` for form-heavy popovers). Escape key closes. Focus moves into panel on open, returns to trigger on close.
- Mobile: When rendered as a BottomSheet, treat accessibility the same as Drawer. `accessibilityViewIsModal={true}` on the modal container.

## Code Examples

### Web

```tsx
import { Popover, PopoverContent, PopoverTrigger } from "@/components/ui/popover";
import { Button } from "@/components/ui/button";
import { Info } from "lucide-react";

// Info card popover — metric explanation
function MetricInfoPopover({ metric }) {
  return (
    <Popover>
      <PopoverTrigger asChild>
        <button
          className="text-[--cv-text-tertiary] hover:text-[--cv-text-secondary] transition-colors"
          aria-label={`More info about ${metric.name}`}
        >
          <Info size={14} />
        </button>
      </PopoverTrigger>
      <PopoverContent
        className="bg-[--cv-bg-surface-elevated] shadow-[--cv-shadow-md] rounded-[--cv-radius-md] border border-[--cv-border-default] p-4 max-w-[280px]"
        side="top"
        align="start"
      >
        <p className="text-[--cv-text-primary] text-sm font-medium mb-1">{metric.name}</p>
        <p className="text-[--cv-text-secondary] text-xs leading-relaxed">{metric.description}</p>
        {metric.learnMoreUrl && (
          <a href={metric.learnMoreUrl} className="text-[--cv-text-link] text-xs mt-2 block underline">
            Learn more
          </a>
        )}
      </PopoverContent>
    </Popover>
  );
}

// Form popover — inline filter
function FilterPopover({ filters, onApply }) {
  return (
    <Popover>
      <PopoverTrigger asChild>
        <Button variant="outline" className="border-[--cv-border-default] text-[--cv-text-primary] gap-2">
          <Filter size={14} /> Filters
        </Button>
      </PopoverTrigger>
      <PopoverContent
        className="bg-[--cv-bg-surface-elevated] shadow-[--cv-shadow-md] rounded-[--cv-radius-md] border border-[--cv-border-default] p-4 w-[320px]"
        align="start"
      >
        <p className="text-[--cv-text-primary] text-sm font-medium mb-3">Filter conversations</p>
        <FilterForm filters={filters} onApply={onApply} />
      </PopoverContent>
    </Popover>
  );
}
```

### Mobile

```tsx
import { useWindowDimensions } from "react-native";
import BottomSheet, { BottomSheetView } from "@gorhom/bottom-sheet";
import { Pressable, View, Text } from "react-native";
import { Info } from "lucide-react-native";
import { useRef, useState } from "react";

function MetricInfoPopoverMobile({ metric }) {
  const [open, setOpen] = useState(false);
  const sheetRef = useRef(null);
  const { width } = useWindowDimensions();
  const isNarrow = width < 768;

  // On narrow screens, always use bottom sheet
  if (isNarrow) {
    return (
      <>
        <Pressable
          onPress={() => setOpen(true)}
          hitSlop={8}
          accessibilityRole="button"
          accessibilityLabel={`More info about ${metric.name}`}
        >
          <Info size={14} color="var(--cv-text-tertiary)" />
        </Pressable>

        <BottomSheet
          ref={sheetRef}
          index={open ? 0 : -1}
          snapPoints={["30%"]}
          enablePanDownToClose
          onClose={() => setOpen(false)}
          backgroundStyle={{ backgroundColor: "var(--cv-bg-surface-elevated)" }}
        >
          <BottomSheetView className="px-6 pb-8 pt-2">
            <Text className="text-[--cv-text-primary] font-medium text-base mb-2">{metric.name}</Text>
            <Text className="text-[--cv-text-secondary] text-sm leading-relaxed">{metric.description}</Text>
          </BottomSheetView>
        </BottomSheet>
      </>
    );
  }

  // On wide screens (iPad), could render inline — same pattern with absolute positioning
  return null; // handle wide screen separately
}
```

## Anti-Patterns

- Never use Popover for a single plain text hint — Tooltip is lighter and more appropriate.
- Never make a Popover that requires scrolling to see all content on mobile — convert to a BottomSheet or Modal.
- Never use Popover for primary actions — if the content drives a major decision, it belongs in a Modal.
- Never omit collision detection — a Popover that opens off-screen is unusable. Radix handles this automatically; custom implementations must too.
- Never leave a Popover open when the user clicks outside it — always close on outside click.
