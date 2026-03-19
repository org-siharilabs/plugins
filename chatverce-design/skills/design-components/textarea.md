
# Textarea

**Gallery reference:** https://component.gallery/components/textarea/
**Aliases:** Multiline input, Text area, Message box, Long-form input

## When to Use

Use Textarea for any input that expects more than one line: agent persona instructions, message templates, customer notes, descriptions, feedback. Textarea signals to the user that their content has room to grow.

## When NOT to Use

Do not use Textarea for inputs that are always a single value (name, email, URL) — use Text Input. Do not use Textarea for structured formatting — use Rich Text Editor. Do not cap textarea to a height so small that the user cannot read their own input; minimum visible height should show 3–4 lines.

## Anatomy

- **Container:** Same as Text Input — `cv-bg-surface-sunken`, `cv-border-default`, `cv-radius-md`
- **Label:** Above, `cv-text-primary`
- **Input text:** `cv-text-primary`, Body size — 16px minimum on mobile
- **Placeholder:** `cv-text-tertiary`
- **Resize handle (web):** Bottom-right corner, vertical resize only
- **Character count (optional):** Bottom-right inside or below, Caption size, `cv-text-tertiary` → `cv-text-warning` at 80% → `cv-text-error` at limit
- **Helper/error text:** Below, Caption size

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Default | Vertically resizable, min 3 rows | General multi-line input |
| Fixed height | Non-resizable, set row count | Comment box, chat input, constrained forms |
| Auto-grow | Grows with content, no scrollbar | Chat-style message input |
| With character count | Shows chars used / limit | When content length matters (SMS templates, short bios) |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | cv-border-default, resting | Ready, open |
| Focus | cv-border-accent + ring | Engaged, writing |
| Filled | cv-text-primary | Content present |
| Character limit warning | Count turns cv-text-warning | Heads up, not urgent |
| Character limit reached | Count turns cv-text-error, input stops | Hard stop, clear |
| Error | cv-border-error, error text below | Issue found |
| Disabled | cv-text-tertiary | Not editable |

## Chatverce Styling

- Background: `cv-bg-surface-sunken`
- Border: `cv-border-default` rest, `cv-border-accent` focus, `cv-border-error` error
- Border radius: `cv-radius-md`
- Resize: `resize-y` (vertical only) on web; no resize handle on mobile
- Min height: 96px (3 rows web), 88px (mobile)
- Font size: 14px web, 16px mobile (minimum)
- Padding: 12px horizontal, 10px vertical

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Textarea>` with cv-token overrides. Add `resize-y` class and set `min-h-[96px]`. For auto-grow: use a library like `react-textarea-autosize` or a CSS trick with a hidden mirror div.

### iOS (Expo + NativeWind)
Use `<TextInput multiline numberOfLines={4} textAlignVertical="top" />`. Set `fontSize: 16` to prevent zoom. Wrap in `KeyboardAvoidingView` behavior `"padding"`. Controlled height via state for auto-grow pattern.

### Android (Expo + NativeWind)
Same as iOS. `textAlignVertical="top"` is especially important on Android — without it, text starts at vertical center. Set `underlineColorAndroid="transparent"`.

## Accessibility

- Web: `<textarea>` with associated `<label>` via `htmlFor`. `aria-describedby` for character count (updates dynamically). `aria-invalid` + `aria-describedby` on error.
- Mobile: `accessibilityLabel` matching visible label. `accessibilityHint` for helper text. Character count should have `accessibilityLiveRegion="polite"` so screen readers announce it without interrupting.

## Code Examples

### Web

```tsx
import { Textarea } from "@/components/ui/textarea";
import { Label } from "@/components/ui/label";
import { useState } from "react";

const MAX_CHARS = 500;

function AgentPersonaField() {
  const [value, setValue] = useState("");
  const remaining = MAX_CHARS - value.length;
  const countColor = remaining < 50 ? (remaining < 0 ? "text-[--cv-text-error]" : "text-[--cv-text-warning]") : "text-[--cv-text-tertiary]";

  return (
    <div className="space-y-1.5">
      <Label htmlFor="persona" className="text-[--cv-text-primary] text-sm font-medium">
        Agent Persona
      </Label>
      <div className="relative">
        <Textarea
          id="persona"
          value={value}
          onChange={(e) => setValue(e.target.value)}
          maxLength={MAX_CHARS}
          placeholder="You are a friendly support agent for Acme Inc. You help customers with order tracking, returns, and product questions…"
          className="bg-[--cv-bg-surface-sunken] border-[--cv-border-default] rounded-[--cv-radius-md] resize-y min-h-[120px] text-[--cv-text-primary] placeholder:text-[--cv-text-tertiary] focus-visible:border-[--cv-border-accent] focus-visible:ring-2 focus-visible:ring-[--cv-border-accent]/20 text-sm"
          aria-describedby="persona-count"
        />
        <span
          id="persona-count"
          className={`absolute bottom-2 right-3 text-xs ${countColor}`}
          aria-live="polite"
        >
          {remaining}
        </span>
      </div>
      <p className="text-[--cv-text-secondary] text-xs">Instructions that shape how your agent communicates.</p>
    </div>
  );
}
```

### Mobile

```tsx
import { View, Text, TextInput } from "react-native";
import { useState } from "react";

const MAX_CHARS = 500;

function AgentPersonaFieldMobile() {
  const [value, setValue] = useState("");
  const remaining = MAX_CHARS - value.length;
  const countColor = remaining < 50 ? (remaining < 0 ? "var(--cv-text-error)" : "var(--cv-text-warning)") : "var(--cv-text-tertiary)";

  return (
    <View className="mb-5">
      <Text className="text-[--cv-text-primary] text-sm font-medium mb-1.5">Agent Persona</Text>
      <TextInput
        value={value}
        onChangeText={setValue}
        multiline
        numberOfLines={5}
        textAlignVertical="top"
        maxLength={MAX_CHARS}
        placeholder="You are a friendly support agent…"
        placeholderTextColor="var(--cv-text-tertiary)"
        style={{
          backgroundColor: "var(--cv-bg-surface-sunken)",
          borderWidth: 1,
          borderColor: "var(--cv-border-default)",
          borderRadius: 8,
          padding: 12,
          fontSize: 16,
          color: "var(--cv-text-primary)",
          minHeight: 120,
          underlineColorAndroid: "transparent",
        }}
        accessibilityLabel="Agent persona instructions"
      />
      <Text
        style={{ color: countColor, fontSize: 12, marginTop: 4, textAlign: "right" }}
        accessibilityLiveRegion="polite"
      >
        {remaining} characters remaining
      </Text>
    </View>
  );
}
```

## Anti-Patterns

- Never set textarea to resize both horizontally and vertically (web) — horizontal resize breaks page layouts.
- Never make textarea shorter than 3 visible lines — users cannot read multi-line content in a single-line window.
- Never use placeholder as a label — placeholder disappears as soon as the user starts typing.
- Never omit `textAlignVertical="top"` on Android — text starts at the vertical center by default, which looks broken.
- Never skip the character count when there is a hard limit — surprises at submission cause frustration.
