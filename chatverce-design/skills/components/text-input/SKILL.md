---
name: text-input
description: Single-line text field for capturing names, emails, URLs, and short free-form input
user_invocable: false
---

# Text Input

**Gallery reference:** https://component.gallery/components/text-input/
**Aliases:** Input, Text field, Input field, Single-line input

## When to Use

Use Text Input for any single-line text capture: names, email addresses, URLs, API keys, search terms, short descriptions. It is the foundational data-entry component and appears in almost every form in Chatverce.

## When NOT to Use

Do not use Text Input for multi-line content — use Textarea. Do not use Text Input for selecting from a fixed list — use Select or Combobox. Do not use it for dates — use Datepicker. Do not use it for numeric increment/decrement — use Stepper.

## Anatomy

- **Container:** `cv-bg-surface-sunken`, `cv-border-default`, `cv-radius-md`, height 40px (web), 44px (mobile)
- **Label:** Above the field, `cv-text-primary`, Body size (required); never inside the input
- **Leading icon (optional):** 16px, `cv-text-secondary`
- **Input text:** `cv-text-primary`, Body size — 16px minimum on mobile to prevent iOS zoom
- **Placeholder:** `cv-text-tertiary`
- **Trailing icon / clear button (optional):** 16px, `cv-text-secondary`
- **Helper text:** Below field, `cv-text-secondary`, Caption size
- **Error message:** Below field, `cv-text-error`, Caption size, with icon

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Default | Plain text field | Most text inputs |
| With leading icon | Icon inside left edge | Email (envelope), URL (link), Search |
| With trailing action | Button or icon inside right edge | Password toggle, clear input, copy button |
| Prefix/suffix | Text inside border, left or right | Currency symbol, domain suffix |
| Error | cv-border-error, error message below | Validation failed |
| Disabled | cv-text-tertiary, cv-bg-surface-sunken, muted | Not editable |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | cv-border-default, resting | Ready, stable |
| Hover (web) | cv-border-strong | Responsive, alive |
| Focus | cv-border-accent + 2px ring (cv-border-accent/20) | Clear, active, system responding |
| Filled | cv-text-primary value | Input received |
| Error | cv-border-error, error text below | Issue found, user can fix |
| Disabled | cv-text-tertiary, no interaction | Not editable |
| Read-only | cv-bg-surface-sunken, no edit cursor | Display only |

## Chatverce Styling

- Background: `cv-bg-surface-sunken`
- Border: `cv-border-default` rest, `cv-border-accent` focus, `cv-border-error` error
- Border radius: `cv-radius-md`
- Focus ring: `2px solid cv-border-accent, offset 0` (or ring utility)
- Font size: 14px web, 16px mobile (minimum — prevents iOS auto-zoom)
- Height: 40px web, 44px mobile (touch target compliance)
- Padding: 12px horizontal, 10px vertical

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Input>` with cv-token overrides. Add `autoComplete` attributes for password managers and autofill. Use `type="email"`, `type="url"`, `type="password"` appropriately — the browser provides built-in validation UI.

### iOS (Expo + NativeWind)
Use React Native `<TextInput>`. Set `fontSize: 16` (minimum) to prevent iOS viewport zoom on focus. Use `keyboardType` to match content: `"email-address"`, `"url"`, `"default"`. Use `autoCapitalize="none"` for emails and URLs. Use `secureTextEntry` for passwords.

### Android (Expo + NativeWind)
Same as iOS. Set `underlineColorAndroid="transparent"` to remove the default Material underline. Use `inputMode` for modern React Native (`"email"`, `"url"`, etc.).

## Accessibility

- Web: `<input>` must be associated with a `<label>` via `htmlFor`/`id`. Use `aria-describedby` for helper text. Use `aria-invalid="true"` + `aria-describedby` pointing to the error message when validation fails.
- Mobile: `accessibilityLabel` (matches visible label). `accessibilityHint` for helper text. When error: `accessibilityInvalid={true}` and update `accessibilityLabel` to include error context.

## Code Examples

### Web

```tsx
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";

// Default field
<div className="space-y-1.5">
  <Label htmlFor="agent-name" className="text-[--cv-text-primary] text-sm font-medium">
    Agent Name
  </Label>
  <Input
    id="agent-name"
    placeholder="e.g. Support Assistant"
    className="bg-[--cv-bg-surface-sunken] border-[--cv-border-default] rounded-[--cv-radius-md] text-[--cv-text-primary] placeholder:text-[--cv-text-tertiary] focus-visible:border-[--cv-border-accent] focus-visible:ring-2 focus-visible:ring-[--cv-border-accent]/20 h-10"
  />
  <p className="text-[--cv-text-secondary] text-xs">This name is shown to your customers.</p>
</div>

// Error state
<div className="space-y-1.5">
  <Label htmlFor="email" className="text-[--cv-text-primary] text-sm font-medium">Email</Label>
  <Input
    id="email"
    type="email"
    aria-invalid="true"
    aria-describedby="email-error"
    className="bg-[--cv-bg-surface-sunken] border-[--cv-border-error] rounded-[--cv-radius-md] focus-visible:ring-[--cv-border-error]/20"
  />
  <p id="email-error" className="text-[--cv-text-error] text-xs flex items-center gap-1">
    <AlertCircle size={12} /> Enter a valid email address.
  </p>
</div>
```

### Mobile

```tsx
import { View, Text, TextInput } from "react-native";
import { useRef } from "react";

function CvTextInput({ label, placeholder, value, onChangeText, error, helperText, keyboardType = "default" }) {
  return (
    <View className="mb-5">
      <Text className="text-[--cv-text-primary] text-sm font-medium mb-1.5">{label}</Text>
      <TextInput
        value={value}
        onChangeText={onChangeText}
        placeholder={placeholder}
        placeholderTextColor="var(--cv-text-tertiary)"
        keyboardType={keyboardType}
        autoCapitalize="none"
        style={{
          backgroundColor: "var(--cv-bg-surface-sunken)",
          borderWidth: 1,
          borderColor: error ? "var(--cv-border-error)" : "var(--cv-border-default)",
          borderRadius: 8,
          paddingHorizontal: 12,
          paddingVertical: 10,
          fontSize: 16, // CRITICAL: prevents iOS zoom
          color: "var(--cv-text-primary)",
          minHeight: 44,
          underlineColorAndroid: "transparent",
        }}
        accessibilityLabel={label}
        accessibilityInvalid={!!error}
        accessibilityHint={helperText}
      />
      {error
        ? <Text className="text-[--cv-text-error] text-xs mt-1" accessibilityLiveRegion="assertive">{error}</Text>
        : helperText && <Text className="text-[--cv-text-secondary] text-xs mt-1">{helperText}</Text>
      }
    </View>
  );
}
```

## Anti-Patterns

- Never use font-size below 16px on mobile inputs — iOS triggers automatic viewport zoom, which is disorienting.
- Never use placeholder text as a label substitute — placeholder disappears on focus.
- Never show validation errors before the user has interacted with the field.
- Never disable autocomplete for non-security-sensitive fields — it reduces friction for users.
- Never remove the visible focus ring — it is a critical keyboard accessibility signal.
