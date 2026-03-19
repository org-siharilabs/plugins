---
name: drawer
description: Sliding panel for content-rich side interactions without leaving the current screen
user_invocable: false
---

# Drawer

**Gallery reference:** https://component.gallery/components/drawer/
**Aliases:** Side panel, Slide-over, Sheet, Bottom sheet, Side drawer, Slide panel

## When to Use

Use Drawer for content-rich panels that accompany the current view rather than replacing it: agent property panels in the builder, conversation details, filter panels, settings sub-sections. Drawer is the right pattern when the user needs to act on something while keeping context of the background screen visible.

## When NOT to Use

Do not use Drawer for single-decision confirmations — that is Modal's role. Do not use Drawer for primary navigation — that is Navigation's role. Do not use Drawer when the content is so extensive it deserves its own page. Do not use Drawer for brief hints or contextual pop-ups — use Tooltip or Popover.

## Anatomy

- **Overlay (scrim):** Partial or full, `rgba(0,0,0,0.3)`, closes drawer on click
- **Panel container:** `cv-bg-surface`, `cv-shadow-lg`, full viewport height (web side), bottom sheet (mobile)
- **Header:** Title (H2), optional close button (X top-right), optional action button
- **Content area:** Scrollable body — forms, lists, detail sections
- **Footer (optional):** Sticky action buttons within the panel

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Right panel (web) | Slides in from right, 400px wide | Properties panel, agent settings, filters |
| Left panel (web) | Slides in from left, 280px wide | Navigation override, menu on mobile breakpoint |
| Bottom sheet (mobile) | Slides up from bottom, snap points | All drawer interactions on mobile |
| Full-height (web) | 100vh, 480px wide | Complex forms, detail views |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default (open) | Panel visible, scrim behind | Focused side context |
| Opening | Slide in 250ms ease-out | Smooth, deliberate arrival |
| Closing | Slide out 200ms ease-in | Quick, unobtrusive departure |
| Swipe dismiss (mobile) | Swipe down to close | Natural, gesture-native |
| Scrim click | Closes drawer | Intuitive dismissal |
| Escape key (web) | Closes drawer | Keyboard-native |

## Chatverce Styling

Same card aesthetic as the Card component — visual consistency between surfaces:

- Background: `cv-bg-surface`
- Shadow: `cv-shadow-lg` (on the panel edge)
- Border radius: `cv-radius-lg` on the exposed corners only (top-left + bottom-left for right panel; top corners for bottom sheet)
- Padding: 24px sides, 20px top (below header), 16px bottom
- Header divider: `cv-border-default` 1px line below header
- Footer: sticky bottom, `cv-border-default` top divider, `cv-bg-surface` background

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Sheet>` component (built on Radix UI Dialog). Set `side="right"` for properties panels. The Sheet handles focus trap, overlay, and escape key. Width is controlled by the `className` on `<SheetContent>`.

### iOS (Expo + NativeWind)
Use `@gorhom/bottom-sheet` with `BottomSheetModal` and `BottomSheetScrollView`. Define snap points based on content height (e.g., `["50%", "90%"]`). Enable `enablePanDownToClose` for swipe dismiss. Use `backdropComponent` for the scrim.

### Android (Expo + NativeWind)
Same as iOS — `@gorhom/bottom-sheet`. Android handles the elevation and shadow differently; set `backgroundStyle` explicitly. Ensure `android_keyboardInputMode="adjustResize"` if the drawer contains form inputs.

## Accessibility

- Web: `role="dialog"`, `aria-modal="true"` (when it traps focus), `aria-labelledby` pointing to the panel title. If the drawer does not trap focus (side panel that coexists with the main content), use `role="complementary"` instead.
- Mobile: `accessibilityViewIsModal={true}` for full-screen drawers. For panels that coexist, use `accessibilityLabel` on the panel container. The drag handle should have `accessibilityLabel="Drag to resize panel"`.

## Code Examples

### Web

```tsx
import {
  Sheet, SheetContent, SheetHeader, SheetTitle, SheetFooter,
} from "@/components/ui/sheet";
import { Button } from "@/components/ui/button";

function AgentPropertiesDrawer({ open, onClose, agent }) {
  return (
    <Sheet open={open} onOpenChange={onClose}>
      <SheetContent
        side="right"
        className="w-[400px] bg-[--cv-bg-surface] shadow-[--cv-shadow-lg] flex flex-col"
      >
        <SheetHeader className="border-b border-[--cv-border-default] pb-4">
          <SheetTitle className="text-[--cv-text-primary] text-xl font-semibold">
            Agent Properties
          </SheetTitle>
        </SheetHeader>

        <div className="flex-1 overflow-y-auto py-5 px-1 space-y-4">
          {/* Form fields or detail content */}
          <AgentSettingsForm agent={agent} />
        </div>

        <SheetFooter className="border-t border-[--cv-border-default] pt-4 flex gap-3">
          <Button variant="ghost" onClick={onClose} className="text-[--cv-text-primary]">
            Cancel
          </Button>
          <Button className="bg-[--cv-bg-accent] text-[--cv-text-on-accent]">
            Save Changes
          </Button>
        </SheetFooter>
      </SheetContent>
    </Sheet>
  );
}
```

### Mobile

```tsx
import BottomSheet, { BottomSheetScrollView, BottomSheetBackdrop } from "@gorhom/bottom-sheet";
import { View, Text, Pressable } from "react-native";
import { useRef, useCallback } from "react";

function AgentPropertiesSheet({ visible, onClose, agent }) {
  const sheetRef = useRef(null);
  const snapPoints = ["60%", "92%"];

  const renderBackdrop = useCallback(
    (props) => <BottomSheetBackdrop {...props} disappearsOnIndex={-1} appearsOnIndex={0} opacity={0.3} />,
    []
  );

  return (
    <BottomSheet
      ref={sheetRef}
      index={visible ? 0 : -1}
      snapPoints={snapPoints}
      enablePanDownToClose
      onClose={onClose}
      backdropComponent={renderBackdrop}
      backgroundStyle={{ backgroundColor: "var(--cv-bg-surface)" }}
      handleIndicatorStyle={{ backgroundColor: "var(--cv-border-default)", width: 40 }}
    >
      <View className="px-6 py-2 border-b border-[--cv-border-default]">
        <Text className="text-[--cv-text-primary] text-xl font-semibold">Agent Properties</Text>
      </View>

      <BottomSheetScrollView className="flex-1 px-6 py-4">
        <AgentSettingsForm agent={agent} />
      </BottomSheetScrollView>

      <View className="px-6 pb-8 pt-4 border-t border-[--cv-border-default] flex-row gap-3">
        <Pressable
          className="flex-1 min-h-[44px] border border-[--cv-border-default] rounded-[--cv-radius-md] items-center justify-center"
          onPress={onClose}
          accessibilityRole="button"
          accessibilityLabel="Cancel"
        >
          <Text className="text-[--cv-text-primary] font-medium">Cancel</Text>
        </Pressable>
        <Pressable
          className="flex-1 min-h-[44px] bg-[--cv-bg-accent] rounded-[--cv-radius-md] items-center justify-center"
          accessibilityRole="button"
          accessibilityLabel="Save changes"
        >
          <Text className="text-[--cv-text-on-accent] font-medium">Save</Text>
        </Pressable>
      </View>
    </BottomSheet>
  );
}
```

## Anti-Patterns

- Never open a Drawer from inside a Modal — user is already in a focused context.
- Never use Drawer for primary navigation on web — sidebar navigation is its own component.
- Never make Drawer content non-scrollable when it may overflow — always use a scrollable content area.
- Never omit the close mechanism (close button + scrim click + escape/swipe) — trapped drawers destroy trust.
- Never put a Drawer inside another Drawer — stack contexts are confusing.
