---
name: radio-button
description: Single-selection control for choosing one mutually exclusive option from a visible group
user_invocable: false
---

# Radio Button

**Gallery reference:** https://component.gallery/components/radio-button/
**Aliases:** Radio, Option selector, Single select, Radio group, Option button

## When to Use

Use Radio Button when the user must select exactly one option from a small set (2–6 options) and all options should be visible simultaneously. Radio communicates mutual exclusivity — selecting one deselects the others. Use it for pricing plans, role assignment, notification frequency, content type selection.

## When NOT to Use

Do not use Radio for more than 6–7 options — use Select instead (too many visible options creates scanning fatigue). Do not use Radio for binary on/off states that take effect immediately — use Toggle. Do not use Radio for multiple simultaneous selections — use Checkbox.

## Anatomy

- **Circle:** 16px diameter, `cv-border-default`, `cv-bg-surface-sunken`
- **Fill dot:** 8px teal dot (`cv-bg-accent`) centered in circle when selected
- **Label:** Beside the circle, `cv-text-primary`, Body size, full-row clickable
- **Helper text (optional):** Below label, `cv-text-secondary`, Caption size
- **Group container:** Fieldset with legend

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Vertical group | Stacked list of options | Default — most readable for 3+ options |
| Horizontal group | Side-by-side options | 2–3 short options where space allows |
| Card radio | Option wrapped in a card with description | Pricing plans, feature tiers |
| Error | cv-border-error on circles | Required group not selected on submit |
| Disabled | Muted circles + labels | Option not available |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Unselected | Empty circle, cv-border-default | Available, unchosen |
| Hover (web) | cv-border-accent on circle | Responsive |
| Selected | cv-bg-accent dot centered in circle | Chosen, confirmed |
| Focus | 2px cv-border-accent ring | Keyboard-navigable |
| Disabled unselected | Muted circle | Unavailable |
| Disabled selected | Muted teal dot | Locked in |
| Error | cv-border-error on all circles in group | Group needs selection |

## Chatverce Styling

- Circle bg: `cv-bg-surface-sunken`
- Circle border: `cv-border-default` unselected, `cv-bg-accent` selected
- Selected fill: `cv-bg-accent` 8px dot
- Border radius: 50% (circle)
- Touch target: 44×44px minimum (mobile)

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<RadioGroup>` + `<RadioGroupItem>` (built on Radix UI). Radix handles `role="radiogroup"`, arrow-key navigation within the group, and `aria-checked`. Wrap in `<fieldset>` + `<legend>` for proper group labeling.

### iOS (Expo + NativeWind)
React Native has no native radio button. Build with `Pressable` rows managed by parent state. Only one row can be selected at a time. Use `accessibilityRole="radio"` and `accessibilityState={{ checked }}`. Trigger `Haptics.selectionAsync()` on selection change.

### Android (Expo + NativeWind)
Same as iOS. TalkBack reads `accessibilityRole="radio"` correctly. Ensure touch target meets 48dp minimum.

## Accessibility

- Web: `role="radiogroup"` on the group container. Each `<RadioGroupItem>` has `role="radio"`, `aria-checked`. Group must have a visible label via `<legend>` or `aria-labelledby`. Arrow keys navigate within the group.
- Mobile: `accessibilityRole="radio"`, `accessibilityState={{ checked }}`, `accessibilityLabel`. The group context is established by proximity and visual label.

## Code Examples

### Web

```tsx
import { RadioGroup, RadioGroupItem } from "@/components/ui/radio-group";
import { Label } from "@/components/ui/label";

function PlanSelector({ value, onChange }) {
  return (
    <fieldset>
      <legend className="text-[--cv-text-primary] text-sm font-medium mb-3">Choose a plan</legend>
      <RadioGroup value={value} onValueChange={onChange} className="space-y-3">
        {[
          { id: "starter", label: "Starter", desc: "1 agent, 500 conversations/mo" },
          { id: "growth", label: "Growth", desc: "5 agents, 5,000 conversations/mo" },
          { id: "pro", label: "Pro", desc: "Unlimited agents and conversations" },
        ].map(plan => (
          <div key={plan.id} className="flex items-start gap-3">
            <RadioGroupItem
              value={plan.id}
              id={plan.id}
              className="mt-0.5 border-[--cv-border-default] text-[--cv-bg-accent] data-[state=checked]:border-[--cv-bg-accent]"
            />
            <div>
              <Label htmlFor={plan.id} className="text-[--cv-text-primary] text-sm font-medium cursor-pointer">
                {plan.label}
              </Label>
              <p className="text-[--cv-text-secondary] text-xs mt-0.5">{plan.desc}</p>
            </div>
          </div>
        ))}
      </RadioGroup>
    </fieldset>
  );
}
```

### Mobile

```tsx
import { View, Text, Pressable } from "react-native";
import * as Haptics from "expo-haptics";

function RadioGroup({ label, options, value, onChange }) {
  return (
    <View className="mb-5">
      <Text className="text-[--cv-text-primary] text-sm font-medium mb-3">{label}</Text>
      {options.map(option => {
        const selected = value === option.value;
        return (
          <Pressable
            key={option.value}
            className="flex-row items-center gap-3 py-2 min-h-[44px]"
            onPress={async () => { await Haptics.selectionAsync(); onChange(option.value); }}
            accessibilityRole="radio"
            accessibilityState={{ checked: selected }}
            accessibilityLabel={`${option.label}${option.description ? `, ${option.description}` : ""}`}
          >
            <View style={{
              width: 20, height: 20, borderRadius: 10,
              borderWidth: selected ? 0 : 1.5,
              borderColor: "var(--cv-border-default)",
              backgroundColor: selected ? "var(--cv-bg-accent)" : "var(--cv-bg-surface-sunken)",
              alignItems: "center", justifyContent: "center",
            }}>
              {selected && (
                <View style={{ width: 8, height: 8, borderRadius: 4, backgroundColor: "white" }} />
              )}
            </View>
            <View className="flex-1">
              <Text className="text-[--cv-text-primary] text-base">{option.label}</Text>
              {option.description && (
                <Text className="text-[--cv-text-secondary] text-xs mt-0.5">{option.description}</Text>
              )}
            </View>
          </Pressable>
        );
      })}
    </View>
  );
}
```

## Anti-Patterns

- Never use Radio for more than 6–7 visible options — use Select.
- Never allow a radio group to have no default selection if one option should always be active (e.g., a required setting).
- Never split a radio group across multiple columns unless options are very short (1–2 words each).
- Never use Radio for multiple simultaneous selections — that is Checkbox's role.
- Never make the circle the only clickable target — the full label must be tappable.
