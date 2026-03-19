
# Dropdown Menu

**Gallery reference:** https://component.gallery/components/dropdown-menu/
**Aliases:** Context menu, Action menu, Menu, Options menu, More actions

## When to Use

Use Dropdown Menu for a set of contextual actions on a specific item: agent row actions (Edit, Duplicate, Archive, Delete), message actions (Copy, Reply, Delete), or toolbar actions grouped under a "..." button. Dropdown Menu is the correct pattern when multiple actions are available but showing all of them as buttons would be too cluttered.

## When NOT to Use

Do not use Dropdown Menu for selecting a value to fill a form field — that is Select or Combobox. Do not use Dropdown Menu for primary navigation — that is Navigation. Do not use Dropdown Menu for simple on/off states — that is Toggle. Do not nest dropdown menus more than two levels deep.

## Anatomy

- **Trigger:** Button (often icon-only with MoreHorizontal/…), `cv-radius-md`
- **Panel:** `cv-bg-surface-elevated`, `cv-shadow-md`, `cv-radius-md`, min-width 160px, `cv-border-default` border
- **Menu items:** `cv-text-primary`, 36px row height, 12px horizontal padding, leading icon optional (16px)
- **Item hover:** `cv-bg-surface` tint
- **Separator:** `cv-border-default` 1px line between item groups
- **Destructive item:** `cv-text-error`, optional destructive icon
- **Keyboard shortcut (optional):** Right-aligned, `cv-text-tertiary`, Caption

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Default | Trigger + panel of actions | Row actions, toolbar overflow |
| With icons | Each item has a leading icon | When visual scanning helps |
| With shortcuts | Items show keyboard shortcuts | Power user tools, desktop app |
| Grouped | Items separated by visual groups | When actions have distinct categories |
| With submenu | Item opens a nested menu | Grouped sub-actions (max one level) |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Trigger default | Icon button resting | Discoverable |
| Trigger hover | cv-bg-surface-elevated | Responsive |
| Menu open | Panel visible, first item focused | Ready to choose |
| Item hover | cv-bg-surface tint | Hoverable |
| Item focused (keyboard) | cv-bg-surface tint + focus ring | Navigable |
| Item active/selected | cv-bg-surface-elevated brief | Action triggered |
| Escape key | Menu closes, focus returns to trigger | Keyboard-native exit |
| Menu closing | Fade/scale out 100ms | Quick, unobtrusive |

## Chatverce Styling

- Panel bg: `cv-bg-surface-elevated`
- Panel shadow: `cv-shadow-md`
- Panel border: `cv-border-default`
- Panel border radius: `cv-radius-md`
- Item text: `cv-text-primary` (default), `cv-text-error` (destructive)
- Item hover bg: `cv-bg-surface`
- Separator: `cv-border-default` 1px
- Panel min-width: 160px, max-width: 240px
- z-index: 50

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<DropdownMenu>` (built on Radix UI). Radix handles `role="menu"`, keyboard navigation (arrow keys, Enter, Escape, type-ahead), portal rendering, and positioning. Always use `<DropdownMenuSeparator>` for visual grouping and `<DropdownMenuItem destructive>` or custom className for destructive items.

### iOS (Expo + NativeWind)
For item-level actions, use the iOS native `UIContextMenuInteraction` via Expo's `ContextMenuView` (from `@zeroseven/react-native-context-menu` or `react-native-context-menu-view`). Alternatively, use a BottomSheet-based action list for consistent cross-platform behavior.

### Android (Expo + NativeWind)
Android has a native popup menu but it cannot be styled. Use the same BottomSheet-based action list as the iOS fallback for consistent Chatverce styling across both platforms.

## Accessibility

- Web: `role="menu"` on the panel, `role="menuitem"` on each item, `aria-haspopup="menu"` on the trigger, `aria-expanded` toggle. Keyboard: arrow keys navigate items, Enter/Space activates, Escape closes. Type-ahead: typing a letter jumps to the first matching item.
- Mobile: BottomSheet list items have `accessibilityRole="menuitem"`. The trigger has `accessibilityRole="button"` and `accessibilityLabel="More actions"` (or context-specific label).

## Code Examples

### Web

```tsx
import {
  DropdownMenu, DropdownMenuContent, DropdownMenuItem,
  DropdownMenuSeparator, DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import { MoreHorizontal, Edit2, Copy, Archive, Trash2 } from "lucide-react";
import { Button } from "@/components/ui/button";

function AgentActionsMenu({ agent, onEdit, onDuplicate, onArchive, onDelete }) {
  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button
          variant="ghost"
          size="icon"
          className="h-8 w-8 text-[--cv-text-secondary] hover:text-[--cv-text-primary] hover:bg-[--cv-bg-surface-elevated]"
          aria-label={`More actions for ${agent.name}`}
        >
          <MoreHorizontal size={16} />
        </Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent
        align="end"
        className="bg-[--cv-bg-surface-elevated] shadow-[--cv-shadow-md] rounded-[--cv-radius-md] border-[--cv-border-default] min-w-[160px] p-1"
      >
        <DropdownMenuItem
          onClick={onEdit}
          className="text-[--cv-text-primary] text-sm focus:bg-[--cv-bg-surface] cursor-pointer flex items-center gap-2"
        >
          <Edit2 size={14} className="text-[--cv-text-secondary]" /> Edit
        </DropdownMenuItem>
        <DropdownMenuItem
          onClick={onDuplicate}
          className="text-[--cv-text-primary] text-sm focus:bg-[--cv-bg-surface] cursor-pointer flex items-center gap-2"
        >
          <Copy size={14} className="text-[--cv-text-secondary]" /> Duplicate
        </DropdownMenuItem>
        <DropdownMenuItem
          onClick={onArchive}
          className="text-[--cv-text-primary] text-sm focus:bg-[--cv-bg-surface] cursor-pointer flex items-center gap-2"
        >
          <Archive size={14} className="text-[--cv-text-secondary]" /> Archive
        </DropdownMenuItem>
        <DropdownMenuSeparator className="bg-[--cv-border-default]" />
        <DropdownMenuItem
          onClick={onDelete}
          className="text-[--cv-text-error] text-sm focus:bg-[--cv-bg-destructive-subtle] cursor-pointer flex items-center gap-2"
        >
          <Trash2 size={14} /> Delete
        </DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  );
}
```

### Mobile

```tsx
import BottomSheet, { BottomSheetView } from "@gorhom/bottom-sheet";
import { View, Text, Pressable } from "react-native";
import { Edit2, Copy, Archive, Trash2, MoreHorizontal } from "lucide-react-native";
import { useRef, useState } from "react";

const ACTIONS = [
  { id: "edit", label: "Edit", icon: Edit2, destructive: false },
  { id: "duplicate", label: "Duplicate", icon: Copy, destructive: false },
  { id: "archive", label: "Archive", icon: Archive, destructive: false },
  { id: "delete", label: "Delete", icon: Trash2, destructive: true },
];

function AgentActionsMenuMobile({ agent, onAction }) {
  const [open, setOpen] = useState(false);
  const sheetRef = useRef(null);

  return (
    <>
      <Pressable
        className="p-2 rounded-[--cv-radius-sm]"
        onPress={() => setOpen(true)}
        accessibilityRole="button"
        accessibilityLabel={`More actions for ${agent.name}`}
      >
        <MoreHorizontal size={20} color="var(--cv-text-secondary)" />
      </Pressable>

      <BottomSheet
        ref={sheetRef}
        index={open ? 0 : -1}
        snapPoints={["35%"]}
        enablePanDownToClose
        onClose={() => setOpen(false)}
        backgroundStyle={{ backgroundColor: "var(--cv-bg-surface-elevated)" }}
      >
        <BottomSheetView className="pb-8">
          {ACTIONS.map((action, i) => (
            <View key={action.id}>
              {action.destructive && i > 0 && (
                <View className="h-px bg-[--cv-border-default] mx-4 my-1" />
              )}
              <Pressable
                className="flex-row items-center gap-4 px-6 py-4 active:bg-[--cv-bg-surface]"
                onPress={() => { onAction(action.id); setOpen(false); }}
                accessibilityRole="menuitem"
                accessibilityLabel={action.label}
              >
                <action.icon
                  size={18}
                  color={action.destructive ? "var(--cv-status-error)" : "var(--cv-text-secondary)"}
                />
                <Text style={{ color: action.destructive ? "var(--cv-text-error)" : "var(--cv-text-primary)", fontSize: 16 }}>
                  {action.label}
                </Text>
              </Pressable>
            </View>
          ))}
        </BottomSheetView>
      </BottomSheet>
    </>
  );
}
```

## Anti-Patterns

- Never put more than 8–9 items in a single dropdown menu — consider grouping or a different pattern.
- Never use Dropdown Menu for value selection in a form — use Select or Combobox.
- Never disable arrow-key navigation in a dropdown — it is expected and required for keyboard accessibility.
- Never omit `aria-label` on an icon-only trigger — the user needs to know what menu they are opening.
- Never put the destructive action first — place it last, separated by a Separator, so users do not accidentally trigger it.
