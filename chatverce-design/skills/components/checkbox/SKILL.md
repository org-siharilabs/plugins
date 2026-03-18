---
name: checkbox
description: Binary or group-selection control for independent multi-select choices
user_invocable: false
---

# Checkbox

**Gallery reference:** https://component.gallery/components/checkbox/
**Aliases:** Multi-select, Check, Tick box, Checkbox input

## When to Use

Use Checkbox for independent binary choices (agree to terms, enable a feature) or for multi-select from a group (select multiple notification types, apply multiple filters). Each checkbox in a group is independent — checking one does not affect others.

## When NOT to Use

Do not use Checkbox for mutually exclusive choices — use Radio Button. Do not use a single checkbox for an on/off state that takes effect immediately — use Toggle. Do not use more than 7–8 checkboxes in a group without a Select or Combobox alternative.

## Anatomy

- **Box:** 16×16px square, `cv-border-default`, `cv-radius-sm`, `cv-bg-surface-sunken`
- **Check icon:** White checkmark on `cv-bg-accent` background when checked
- **Indeterminate dash:** White dash on `cv-bg-accent` background when indeterminate
- **Label:** Beside the box, `cv-text-primary`, Body size, clickable area extends to full row
- **Helper text (optional):** Below label, `cv-text-secondary`, Caption size

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Single | Standalone checkbox + label | Consent, agreement, single feature toggle |
| Group | Multiple checkboxes under a fieldset | Multi-select options, filter panels |
| With indeterminate | Three-state: checked, unchecked, indeterminate | "Select all" parent over a partially-selected group |
| Error | cv-border-error box | Validation failed (e.g., required agreement not checked) |
| Disabled | Muted box, no interaction | Option not available |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Unchecked | Empty box, cv-border-default | Available, unselected |
| Hover (web) | cv-border-accent | Responsive, inviting |
| Checked | Teal fill (cv-bg-accent) + white checkmark | Selected, confirmed |
| Indeterminate | Teal fill + white dash | Partial, mixed |
| Focus | 2px cv-border-accent focus ring | Keyboard-navigable |
| Disabled unchecked | Muted box, cv-text-tertiary label | Unavailable |
| Disabled checked | Muted teal fill | Locked in |
| Error | cv-border-error | Issue |

## Chatverce Styling

- Box bg unchecked: `cv-bg-surface-sunken`
- Box bg checked/indeterminate: `cv-bg-accent`
- Border unchecked: `cv-border-default`
- Border checked: `cv-bg-accent` (border matches fill)
- Check/dash color: white (`cv-text-on-accent`)
- Border radius: `cv-radius-sm` (4px)
- Touch target: 44×44px minimum (box is centered in touch area on mobile)

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Checkbox>` (built on Radix UI). It handles `aria-checked` (including `"mixed"` for indeterminate), keyboard interaction (Space to toggle), and focus management. Associate with a `<Label>` via `htmlFor` or wrap both in a `<label>`.

### iOS (Expo + NativeWind)
React Native has no native checkbox. Build with `Pressable` containing a `View` that renders as a box. Manage checked state via `useState`. Use `accessibilityRole="checkbox"` and `accessibilityState={{ checked: boolean }}`. Trigger `Haptics.selectionAsync()` on state change.

### Android (Expo + NativeWind)
Same as iOS. Android TalkBack reads `accessibilityRole="checkbox"` and `accessibilityState` correctly. Ensure the touch target is at least 48×48dp.

## Accessibility

- Web: `role="checkbox"`, `aria-checked` (true/false/mixed). When in a group, wrap in a `<fieldset>` with `<legend>`. All `<Checkbox>` elements must have an associated label.
- Mobile: `accessibilityRole="checkbox"`, `accessibilityState={{ checked }}`, `accessibilityLabel` including the label text.

## Code Examples

### Web

```tsx
import { Checkbox } from "@/components/ui/checkbox";
import { Label } from "@/components/ui/label";

// Single checkbox
<div className="flex items-center gap-2">
  <Checkbox
    id="terms"
    className="border-[--cv-border-default] data-[state=checked]:bg-[--cv-bg-accent] data-[state=checked]:border-[--cv-bg-accent] rounded-[--cv-radius-sm]"
  />
  <Label htmlFor="terms" className="text-[--cv-text-primary] text-sm cursor-pointer">
    I agree to the <a href="/terms" className="text-[--cv-text-link] underline">Terms of Service</a>
  </Label>
</div>

// Checkbox group with indeterminate "select all"
function NotificationSettings({ options, selected, onChange }) {
  const allChecked = selected.length === options.length;
  const someChecked = selected.length > 0 && !allChecked;

  return (
    <fieldset className="space-y-3">
      <legend className="text-[--cv-text-primary] text-sm font-medium mb-3">Notify me about</legend>
      <div className="flex items-center gap-2">
        <Checkbox
          id="all"
          checked={allChecked}
          data-state={someChecked ? "indeterminate" : allChecked ? "checked" : "unchecked"}
          onCheckedChange={(checked) => onChange(checked ? options.map(o => o.id) : [])}
          className="border-[--cv-border-default] data-[state=checked]:bg-[--cv-bg-accent] data-[state=indeterminate]:bg-[--cv-bg-accent]"
        />
        <Label htmlFor="all" className="text-[--cv-text-primary] text-sm font-medium cursor-pointer">All notifications</Label>
      </div>
      {options.map(option => (
        <div key={option.id} className="flex items-center gap-2 pl-6">
          <Checkbox
            id={option.id}
            checked={selected.includes(option.id)}
            onCheckedChange={(checked) => onChange(checked ? [...selected, option.id] : selected.filter(id => id !== option.id))}
            className="border-[--cv-border-default] data-[state=checked]:bg-[--cv-bg-accent] data-[state=checked]:border-[--cv-bg-accent]"
          />
          <Label htmlFor={option.id} className="text-[--cv-text-primary] text-sm cursor-pointer">{option.label}</Label>
        </div>
      ))}
    </fieldset>
  );
}
```

### Mobile

```tsx
import { Pressable, View, Text } from "react-native";
import * as Haptics from "expo-haptics";
import { Check, Minus } from "lucide-react-native";

function CvCheckbox({ label, checked, indeterminate = false, onChange, disabled = false }) {
  const handlePress = async () => {
    if (disabled) return;
    await Haptics.selectionAsync();
    onChange(!checked);
  };

  const isActive = checked || indeterminate;
  return (
    <Pressable
      className="flex-row items-center gap-3 py-1 min-h-[44px]"
      onPress={handlePress}
      disabled={disabled}
      accessibilityRole="checkbox"
      accessibilityState={{ checked: indeterminate ? "mixed" : checked, disabled }}
      accessibilityLabel={label}
    >
      <View style={{
        width: 20, height: 20, borderRadius: 4,
        backgroundColor: isActive ? "var(--cv-bg-accent)" : "var(--cv-bg-surface-sunken)",
        borderWidth: isActive ? 0 : 1,
        borderColor: "var(--cv-border-default)",
        alignItems: "center", justifyContent: "center",
        opacity: disabled ? 0.4 : 1,
      }}>
        {checked && !indeterminate && <Check size={12} color="white" strokeWidth={3} />}
        {indeterminate && <Minus size={12} color="white" strokeWidth={3} />}
      </View>
      <Text className="text-[--cv-text-primary] text-base flex-1">{label}</Text>
    </Pressable>
  );
}
```

## Anti-Patterns

- Never use Checkbox for mutually exclusive options — that is Radio Button's purpose.
- Never make the checkbox box the only clickable area — the full label row must be tappable/clickable.
- Never omit the indeterminate state when building a "select all" checkbox — users expect it.
- Never skip the group `<fieldset>` + `<legend>` wrapper — screen readers need the group context.
- Never use Checkbox for an action that takes effect immediately — Toggle communicates "this is live now" more clearly.
