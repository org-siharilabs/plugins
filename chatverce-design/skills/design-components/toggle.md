
# Toggle

**Gallery reference:** https://component.gallery/components/toggle/
**Aliases:** Switch, On/off switch, Toggle switch

## When to Use

Use Toggle for binary settings that take effect immediately without a save action: enabling/disabling a notification type, activating an AI feature, switching a preference on or off. Toggle communicates "this is live right now" — the effect is instant.

## When NOT to Use

Do not use Toggle for a form field that requires saving before taking effect — use Checkbox instead. Do not use Toggle for multi-value choices — use Radio or Select. Do not use Toggle where the on/off meaning is ambiguous — always include a clear label and, if needed, state description ("Notifications on" / "Notifications off").

## Anatomy

- **Track:** Rounded rectangle, `cv-bg-surface-sunken` (off) or `cv-bg-accent` (on), width ~40px, height ~24px
- **Thumb:** White circle, slightly smaller than track height, with subtle shadow
- **Label:** Beside the toggle, `cv-text-primary`, Body size
- **State description (optional):** Caption below, `cv-text-secondary` — describes the current state ("Agent will respond automatically" / "Agent responses paused")

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Default | Track + thumb + label | Settings rows, feature flags |
| With description | Track + label + current-state description | When on/off meanings need explanation |
| Compact | Smaller track (32×18px) | Dense settings panels |
| Disabled | Muted track, no interaction | Not configurable |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Off | cv-bg-surface-sunken track, thumb left | Inactive, paused |
| Off hover (web) | cv-border-strong ring on track | Responsive |
| Turning on | Smooth thumb slide right, 200ms ease | Action in progress |
| On | cv-bg-accent track, thumb right | Active, live |
| On hover (web) | cv-bg-accent-hover | Responsive |
| Focus | 2px cv-border-accent ring around track | Keyboard-accessible |
| Disabled off | Muted track | Unavailable |
| Disabled on | Muted teal track | Locked active |

## Chatverce Styling

- Track off: `cv-bg-surface-sunken` with `cv-border-default`
- Track on: `cv-bg-accent`
- Track dimensions: 40×22px (default), 32×18px (compact)
- Thumb: white, circular, `cv-shadow-sm`
- Thumb size: 18px (default), 14px (compact)
- Transition: thumb slide 200ms ease-in-out, track color 150ms ease
- Focus ring: 2px `cv-border-accent`, 2px offset

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Switch>` (built on Radix UI). Radix handles `role="switch"`, `aria-checked`, keyboard interaction (Space to toggle), and focus management. Apply cv-token color overrides.

### iOS (Expo + NativeWind)
Use React Native `<Switch>` component. Set `trackColor={{ false: "var(--cv-bg-surface-sunken)", true: "var(--cv-bg-accent)" }}` and `thumbColor="white"`. Trigger `Haptics.impactAsync(ImpactFeedbackStyle.Light)` on value change. Set `accessibilityRole="switch"`.

### Android (Expo + NativeWind)
Same as iOS. Android renders a Material-style toggle by default — override colors with `trackColor` and `thumbColor`. The Switch component on Android also benefits from `Haptics.impactAsync`.

## Accessibility

- Web: `role="switch"`, `aria-checked` (true/false). Associated label must be provided via `<label htmlFor>` or `aria-labelledby`. If the toggle has a state description, include `aria-describedby`.
- Mobile: `accessibilityRole="switch"`, `accessibilityState={{ checked: value }}`, `accessibilityLabel` including the setting name. State description should use `accessibilityHint`.

## Code Examples

### Web

```tsx
import { Switch } from "@/components/ui/switch";
import { Label } from "@/components/ui/label";

// Simple toggle in settings
function SettingToggle({ id, label, description, checked, onChange }) {
  return (
    <div className="flex items-start justify-between gap-4 py-4 border-b border-[--cv-border-default] last:border-b-0">
      <div>
        <Label htmlFor={id} className="text-[--cv-text-primary] text-sm font-medium cursor-pointer">
          {label}
        </Label>
        {description && (
          <p className="text-[--cv-text-secondary] text-xs mt-0.5">{description}</p>
        )}
      </div>
      <Switch
        id={id}
        checked={checked}
        onCheckedChange={onChange}
        className="data-[state=checked]:bg-[--cv-bg-accent] data-[state=unchecked]:bg-[--cv-bg-surface-sunken] focus-visible:ring-2 focus-visible:ring-[--cv-border-accent]"
      />
    </div>
  );
}

// Usage
<SettingToggle
  id="auto-respond"
  label="Auto-respond"
  description={checked ? "Agent will respond automatically to new messages." : "Agent responses are paused."}
  checked={autoRespond}
  onChange={setAutoRespond}
/>
```

### Mobile

```tsx
import { View, Text, Switch } from "react-native";
import * as Haptics from "expo-haptics";

function SettingToggleMobile({ label, description, value, onChange }) {
  const handleChange = async (newValue) => {
    await Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light);
    onChange(newValue);
  };

  return (
    <View
      className="flex-row items-center justify-between py-4 border-b border-[--cv-border-default]"
      accessibilityRole="switch"
      accessibilityState={{ checked: value }}
      accessibilityLabel={label}
      accessibilityHint={description}
    >
      <View className="flex-1 pr-4">
        <Text className="text-[--cv-text-primary] text-base font-medium">{label}</Text>
        {description && (
          <Text className="text-[--cv-text-secondary] text-sm mt-0.5">{description}</Text>
        )}
      </View>
      <Switch
        value={value}
        onValueChange={handleChange}
        trackColor={{ false: "var(--cv-bg-surface-sunken)", true: "var(--cv-bg-accent)" }}
        thumbColor="white"
        ios_backgroundColor="var(--cv-bg-surface-sunken)"
      />
    </View>
  );
}
```

## Anti-Patterns

- Never use Toggle for a form field that does not take effect immediately — use Checkbox for settings that need explicit save.
- Never leave Toggle without a visible label — the control alone does not communicate what it controls.
- Never use Toggle for more than two states — it is binary only.
- Never skip haptic feedback on mobile — the tactile response is part of the "this took effect" communication.
- Never use Toggle in a form alongside other inputs and submit buttons — the immediate-effect paradigm conflicts with form-submit paradigm. Keep Toggle in settings contexts, not form contexts.
