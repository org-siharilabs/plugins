---
name: select
description: Single-selection control for choosing one value from a fixed, predefined list
user_invocable: false
---

# Select

**Gallery reference:** https://component.gallery/components/select/
**Aliases:** Dropdown select, Picker, Single select, Dropdown, Choice input

## When to Use

Use Select when the user must choose exactly one value from a list of 5–15 options where all options are known in advance. Select is appropriate when showing all options at once (radio group) would consume too much vertical space.

## When NOT to Use

Use Radio Button for 2–4 options — showing them all at once is clearer. Use Combobox for 15+ options or when the user should be able to search. Use Toggle for binary on/off choices. Do not use Select for more than one simultaneous selection — that pattern needs a Combobox with multi-select or a Checkbox group.

## Anatomy

- **Trigger button:** Displays current selection or placeholder, `cv-bg-surface-sunken`, `cv-border-default`, `cv-radius-md`, chevron-down icon trailing
- **Dropdown panel:** `cv-bg-surface-elevated`, `cv-shadow-md`, `cv-radius-md`, z-index elevated
- **Option items:** `cv-text-primary`, hover `cv-bg-surface-elevated` tint, selected = check icon + `cv-text-accent`
- **Placeholder text:** `cv-text-tertiary`

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Default | Trigger + dropdown | Most select use cases |
| With label group | Optgroup-style visual separation | Long lists with categories |
| With icon | Each option has a leading icon | Status selects, channel selects |
| Error | cv-border-error trigger | Validation failed |
| Disabled | Muted trigger, no interaction | Not selectable |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | Resting trigger | Stable, selectable |
| Hover (trigger) | cv-border-strong | Responsive |
| Focus (trigger) | cv-border-accent + ring | Keyboard-navigable |
| Open | Dropdown visible, trigger highlighted | Choosing |
| Option hover | cv-bg-surface-elevated tint | Hoverable |
| Option selected | Check + cv-text-accent | Confirmed |
| Closed (selection made) | Selected value in trigger | Done |
| Error | cv-border-error | Needs attention |

## Chatverce Styling

- Trigger bg: `cv-bg-surface-sunken`
- Border: `cv-border-default` rest, `cv-border-accent` open/focus
- Border radius: `cv-radius-md`
- Dropdown bg: `cv-bg-surface-elevated`
- Dropdown shadow: `cv-shadow-md`
- Selected indicator: Teal check icon, `cv-text-accent`
- Placeholder: `cv-text-tertiary`

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Select>` (built on Radix UI). It handles positioning, keyboard navigation (arrow keys, type-ahead), and ARIA automatically. Never use a native `<select>` element on web — it cannot be styled to Chatverce standards and does not support icons in options.

### iOS (Expo + NativeWind)
Use `ActionSheet` (via `@expo/react-native-action-sheet`) for fewer than 8 options. For more options, use a modal with a native-feeling `ScrollView` list or `expo-picker`. The iOS native wheel picker (`@react-native-picker/picker`) works for numeric or date-adjacent selections.

### Android (Expo + NativeWind)
Use `@react-native-picker/picker` with `mode="dialog"` (modal picker dialog) or implement a custom BottomSheet-based picker for full styling control. Android's default Spinner widget cannot be styled to match Chatverce design.

## Accessibility

- Web: Radix `<Select>` handles `role="combobox"`, `aria-expanded`, `aria-haspopup`, keyboard navigation, and focus management automatically. Ensure the trigger has an accessible name via a visible `<Label>` with `htmlFor`.
- Mobile: The trigger `Pressable` must have `accessibilityRole="combobox"`, `accessibilityLabel`, and `accessibilityState={{ expanded: isOpen }}`. Selected option should announce via `accessibilityLiveRegion="polite"`.

## Code Examples

### Web

```tsx
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select";
import { Label } from "@/components/ui/label";

<div className="space-y-1.5">
  <Label htmlFor="channel-select" className="text-[--cv-text-primary] text-sm font-medium">
    Default Channel
  </Label>
  <Select onValueChange={(val) => setChannel(val)}>
    <SelectTrigger
      id="channel-select"
      className="bg-[--cv-bg-surface-sunken] border-[--cv-border-default] rounded-[--cv-radius-md] text-[--cv-text-primary] focus:border-[--cv-border-accent] focus:ring-2 focus:ring-[--cv-border-accent]/20 h-10"
    >
      <SelectValue placeholder="Choose a channel…" className="text-[--cv-text-tertiary]" />
    </SelectTrigger>
    <SelectContent className="bg-[--cv-bg-surface-elevated] shadow-[--cv-shadow-md] rounded-[--cv-radius-md] border-[--cv-border-default]">
      <SelectItem value="whatsapp" className="text-[--cv-text-primary] focus:bg-[--cv-bg-surface] focus:text-[--cv-text-accent]">WhatsApp</SelectItem>
      <SelectItem value="sms" className="text-[--cv-text-primary] focus:bg-[--cv-bg-surface] focus:text-[--cv-text-accent]">SMS</SelectItem>
      <SelectItem value="email" className="text-[--cv-text-primary] focus:bg-[--cv-bg-surface] focus:text-[--cv-text-accent]">Email</SelectItem>
      <SelectItem value="webchat" className="text-[--cv-text-primary] focus:bg-[--cv-bg-surface] focus:text-[--cv-text-accent]">Web Chat</SelectItem>
    </SelectContent>
  </Select>
</div>
```

### Mobile

```tsx
import { ActionSheetProvider, useActionSheet } from "@expo/react-native-action-sheet";
import { Pressable, View, Text } from "react-native";
import { ChevronDown } from "lucide-react-native";

function CvSelect({ label, options, value, onChange }) {
  const { showActionSheetWithOptions } = useActionSheet();

  const handlePress = () => {
    const optionLabels = [...options.map(o => o.label), "Cancel"];
    showActionSheetWithOptions(
      { options: optionLabels, cancelButtonIndex: optionLabels.length - 1 },
      (index) => {
        if (index !== undefined && index < options.length) {
          onChange(options[index].value);
        }
      }
    );
  };

  const selectedLabel = options.find(o => o.value === value)?.label;

  return (
    <View className="mb-5">
      <Text className="text-[--cv-text-primary] text-sm font-medium mb-1.5">{label}</Text>
      <Pressable
        className="flex-row items-center justify-between bg-[--cv-bg-surface-sunken] border border-[--cv-border-default] rounded-[--cv-radius-md] px-4 min-h-[44px]"
        onPress={handlePress}
        accessibilityRole="combobox"
        accessibilityLabel={`${label}: ${selectedLabel || "not selected"}`}
      >
        <Text style={{ color: selectedLabel ? "var(--cv-text-primary)" : "var(--cv-text-tertiary)", fontSize: 16 }}>
          {selectedLabel || "Choose…"}
        </Text>
        <ChevronDown size={16} color="var(--cv-text-secondary)" />
      </Pressable>
    </View>
  );
}
```

## Anti-Patterns

- Never use a native `<select>` on web — it cannot be styled and does not support icons or custom option rendering.
- Never use Select for binary choices — Toggle or two Radio Buttons are clearer.
- Never use Select for more than ~15 options without a search mechanism — use Combobox instead.
- Never open a Select dropdown immediately on page load — let the user initiate.
- Never disable the entire Select without explaining why the field is unavailable.
