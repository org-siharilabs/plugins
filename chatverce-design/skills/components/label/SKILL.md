---
name: label
description: Visible text label placed above a form input, always associated with its control
user_invocable: false
---

# Label

**Gallery reference:** https://component.gallery/components/label/
**Aliases:** Input label, Form label, Field label

## When to Use

Use Label on every form input — text fields, selects, checkboxes, radios, file uploads. A visible label is required for every form control except icon-only inputs in constrained toolbars (which must instead carry an `aria-label`). Never rely on placeholder text as a substitute for a label.

## When NOT to Use

Do not use Label as a decorative heading — use Heading instead. Do not float labels (Material Design style) — Chatverce always places labels above inputs. Do not omit labels and use placeholder text as the label — placeholders disappear when the user types.

## Anatomy

- **`<label>` element:** Associated with its input via `for`/`htmlFor` matching the input's `id`.
- **Label text:** `cv-text-label` (14px, weight 500, `cv-text-primary`).
- **Required indicator (optional):** `*` in `cv-text-destructive` after the label text, with `aria-hidden="true"`. Pair with a legend or hint explaining the `*` convention.
- **Optional indicator (optional):** "(optional)" in `cv-text-tertiary` 13px after label text.

## Variants

| Variant | Description |
|---------|-------------|
| Required | Label + `*` indicator in `cv-text-destructive`. Input is mandatory. |
| Optional | Label + "(optional)" in `cv-text-tertiary`. Reduces burden when most fields are required. |

## Chatverce Styling

- Font size: 14px (`cv-text-sm`)
- Font weight: 500 (medium)
- Color: `cv-text-primary`
- Position: always above the input, never floating or inline
- Spacing below: `cv-space-1` (4px) between label and input
- Required `*`: `cv-text-destructive`, `aria-hidden="true"`

## Platform Considerations

Web: Use the native `<label>` element with `htmlFor`. This provides a larger click target for the input. Mobile: React Native has no `<label>` equivalent — render a `<Text>` above the input. Use `nativeID` on the Text and `aria-labelledby` on the TextInput to associate them.

## Accessibility

- `htmlFor` (web) must match the input's `id` — this is required, not optional.
- The `*` required indicator must be `aria-hidden="true"`; the required state is communicated via `aria-required="true"` on the input itself.
- Mobile: `nativeID` + `aria-labelledby` pairing is the React Native equivalent.

## Code Example

```tsx
{/* Web */}
<div className="flex flex-col gap-1">
  <label htmlFor="agent-name" className="text-sm font-medium text-[--cv-text-primary]">
    Agent name
    <span aria-hidden="true" className="text-[--cv-text-destructive] ml-0.5">*</span>
  </label>
  <input
    id="agent-name"
    type="text"
    required
    aria-required="true"
    className="border border-[--cv-border-default] rounded-[--cv-radius-md] px-3 py-2 text-[--cv-text-primary]"
  />
</div>

{/* Optional variant */}
<div className="flex flex-col gap-1">
  <label htmlFor="agent-description" className="text-sm font-medium text-[--cv-text-primary]">
    Description
    <span className="text-xs text-[--cv-text-tertiary] ml-1.5">(optional)</span>
  </label>
  <input id="agent-description" type="text" className="border border-[--cv-border-default] rounded-[--cv-radius-md] px-3 py-2" />
</div>
```

## Anti-Patterns

- Never use placeholder text as a label substitute — it vanishes when the user types.
- Never use floating labels — they are ambiguous when the field is filled.
- Never omit `htmlFor` on web — clicking the label won't focus the input without it.
- Never style labels as `cv-text-tertiary` — they must be `cv-text-primary` to meet contrast requirements.
