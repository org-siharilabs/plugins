
# Modal

**Gallery reference:** https://component.gallery/components/modal/
**Aliases:** Dialog, Overlay, Popup, Alert dialog, Confirmation dialog

## When to Use

Use Modal when the user must make a focused decision before continuing: confirming a destructive action, completing a short focused form (invite team member, rename agent), or viewing a detail that requires full attention. Modal communicates "this requires your complete focus before you can continue."

## When NOT to Use

Do not use Modal for multi-step flows with more than 3 steps — use a dedicated page or Drawer instead. Do not use Modal for passive notifications — use Toast or Alert. Do not use Modal for content-rich side panels — use Drawer. Do not nest modals. Do not use Modal for simple tooltips or contextual hints — use Tooltip or Popover.

## Anatomy

- **Overlay (scrim):** Full-viewport, `rgba(0,0,0,0.5)`, closes modal on click
- **Container:** `cv-bg-surface-elevated`, `cv-shadow-lg`, `cv-radius-lg`, max-width 480px (default), 640px (wide)
- **Header:** Title (H2), optional close button (X icon, top-right)
- **Body:** Content area — description, form fields, detail view
- **Footer:** Action buttons — one primary action, one ghost/secondary cancel

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Confirmation | Title + description + confirm/cancel | Destructive confirmation, irreversible action |
| Form | Title + form fields + submit/cancel | Invite user, rename, quick create |
| Info | Title + rich content + single close action | Detail view, help content |
| Alert | Icon + title + description + confirm | System-level warning requiring acknowledgment |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default (open) | Centered overlay, scrim behind | Focused, contained, deliberate |
| Opening | Fade-in + scale 95%→100%, 200ms ease-out | Smooth, intentional arrival |
| Closing | Fade-out + scale 100%→95%, 150ms ease-in | Quick, unobtrusive departure |
| Focus trap | Focus cycles within modal | Safe, cannot accidentally leave |
| Escape key | Closes modal | Quick, keyboard-native escape |
| Scrim click | Closes modal (unless form has unsaved data) | Intuitive dismissal |

## Chatverce Styling

- Background: `cv-bg-surface-elevated`
- Shadow: `cv-shadow-lg`
- Border radius: `cv-radius-lg`
- Overlay: `rgba(0,0,0,0.5)` — `bg-black/50` in Tailwind
- Max width: 480px default, 640px for wide variant
- Padding: 24px (header/footer), 20px (body)
- z-index: 50 (modal), 40 (overlay scrim)

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Dialog>` (built on Radix UI). It handles focus trap, escape key, and ARIA automatically. Do not re-implement these behaviors. Ensure the trigger button has a clear label. Use `<DialogTitle>` and `<DialogDescription>` — Radix uses these for `aria-labelledby` and `aria-describedby`.

### iOS (Expo + NativeWind)
Use `.sheet(isPresented:)` style presentation via Expo Router's `<Stack.Screen presentation="modal">` or `expo-modal`. Prefer `pageSheet` presentation style which gives the native iOS card-from-bottom feel. The status bar and gesture dismissal are handled automatically.

### Android (Expo + NativeWind)
On Android, a bottom sheet pattern (via `@gorhom/bottom-sheet`) is more native than a centered overlay. Wrap modal content in a BottomSheetModal. For confirmation dialogs, Android's native `Alert.alert()` can be used for simple two-option decisions.

## Accessibility

- Web: `role="dialog"`, `aria-modal="true"`, `aria-labelledby` pointing to the dialog title, `aria-describedby` pointing to the description. Focus must be trapped inside the modal while open. On close, focus must return to the element that triggered the modal.
- Mobile: `accessibilityViewIsModal={true}` (iOS), `accessibilityLabel` on the modal container. Ensure close button has `accessibilityLabel="Close dialog"`.

## Code Examples

### Web

```tsx
import {
  Dialog, DialogContent, DialogHeader, DialogTitle,
  DialogDescription, DialogFooter,
} from "@/components/ui/dialog";
import { Button } from "@/components/ui/button";

function DeleteAgentModal({ open, onClose, onConfirm, agentName }) {
  return (
    <Dialog open={open} onOpenChange={onClose}>
      <DialogContent className="bg-[--cv-bg-surface-elevated] shadow-[--cv-shadow-lg] rounded-[--cv-radius-lg] max-w-md">
        <DialogHeader>
          <DialogTitle className="text-[--cv-text-primary] text-xl font-semibold">
            Delete {agentName}?
          </DialogTitle>
          <DialogDescription className="text-[--cv-text-secondary] text-sm mt-1">
            This action cannot be undone. All conversations and settings will be permanently removed.
          </DialogDescription>
        </DialogHeader>
        <DialogFooter className="mt-6 flex gap-3">
          <Button variant="ghost" onClick={onClose} className="text-[--cv-text-primary]">
            Cancel
          </Button>
          <Button onClick={onConfirm} className="bg-[--cv-bg-destructive] text-[--cv-text-on-destructive]">
            Delete Agent
          </Button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}
```

### Mobile

```tsx
import BottomSheet, { BottomSheetView } from "@gorhom/bottom-sheet";
import { View, Text, Pressable } from "react-native";
import { useRef } from "react";

function DeleteAgentSheet({ visible, onClose, onConfirm, agentName }) {
  const sheetRef = useRef(null);

  return (
    <BottomSheet
      ref={sheetRef}
      index={visible ? 0 : -1}
      snapPoints={["35%"]}
      enablePanDownToClose
      onClose={onClose}
      backgroundStyle={{ backgroundColor: "var(--cv-bg-surface-elevated)" }}
      handleIndicatorStyle={{ backgroundColor: "var(--cv-border-default)" }}
    >
      <BottomSheetView className="px-6 pb-8">
        <Text className="text-[--cv-text-primary] text-xl font-semibold mb-2">
          Delete {agentName}?
        </Text>
        <Text className="text-[--cv-text-secondary] text-sm mb-6">
          This action cannot be undone. All conversations and settings will be permanently removed.
        </Text>
        <Pressable
          className="bg-[--cv-bg-destructive] rounded-[--cv-radius-md] min-h-[44px] items-center justify-center mb-3"
          onPress={onConfirm}
          accessibilityRole="button"
          accessibilityLabel="Confirm delete agent"
        >
          <Text className="text-[--cv-text-on-destructive] font-medium">Delete Agent</Text>
        </Pressable>
        <Pressable
          className="min-h-[44px] items-center justify-center"
          onPress={onClose}
          accessibilityRole="button"
          accessibilityLabel="Cancel"
        >
          <Text className="text-[--cv-text-primary] font-medium">Cancel</Text>
        </Pressable>
      </BottomSheetView>
    </BottomSheet>
  );
}
```

## Anti-Patterns

- Never place two actions that are both equally prominent. One primary (colored), one ghost (cancel).
- Never nest a modal inside another modal. If you need a second layer, reconsider the flow.
- Never open a modal without a way to close it (close button + escape key + scrim click).
- Never use Modal for content that takes more than 2–3 minutes to read or fill in.
- Never remove the scrim/overlay — it communicates modal scope and prevents misclicks on background content.
