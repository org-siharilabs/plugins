
# Stepper

**Gallery reference:** https://component.gallery/components/stepper/
**Aliases:** Number input, Quantity picker, Incrementer, Spinner (ambiguous — prefer Stepper)

## When to Use

Use Stepper when users need to adjust a small integer quantity with discrete steps — setting a message retry count, choosing the number of agents in a batch, or configuring a timeout in minutes. The explicit +/– buttons make the affordance obvious and reduce error for non-technical users.

## When NOT to Use

Do not use Stepper for large ranges where direct keyboard entry is more efficient — use a text input with `type="number"` instead. Do not use Stepper for non-integer or continuous values — use Slider. Do not use Stepper where the range exceeds ~20 steps; the repetitive button tapping becomes tedious.

## Anatomy

- **Decrement button (–):** Left. Disabled at min value.
- **Value display:** Center. Numeric value. Editable text input or read-only display.
- **Increment button (+):** Right. Disabled at max value.
- **Group container:** `role="group"` with an `aria-label` describing what is being adjusted.

## Variants

| Variant | Description |
|---------|-------------|
| Default | – [value] + inline. Standard quantity input. |
| Detached | Label above, then – [value] + row. For form contexts with a visible Label component. |

## Chatverce Styling

- Container: `flex items-center`, `cv-border-default` border, `cv-radius-md`
- +/– buttons: 40px × 40px (web), 44px × 44px (mobile), `cv-bg-surface` hover `cv-bg-surface-elevated`
- Button icons: `Minus` / `Plus` from Lucide, 16px, `cv-text-primary`
- Value display: `cv-text-primary`, 16px, weight 500, min-width 40px, centered
- Disabled button: `cv-text-tertiary`, `pointer-events-none`
- Dividers: `cv-border-default` between buttons and value

## Platform Considerations

Web: Build as a controlled component with `<button>` for +/– and `<input type="number">` (or display `<span>`) for the value. Mobile: Use `Pressable` for +/– with `Haptics.selectionAsync()` on each tap. Display value in a `Text` or editable `TextInput`.

## Accessibility

- Wrap in `<div role="group" aria-label="[what is being counted]">`.
- Decrement/Increment buttons: `aria-label="Decrease [thing]"` / `aria-label="Increase [thing]"`.
- When at min/max: `aria-disabled="true"` on the respective button.
- If value is in a text input: `aria-label` or `aria-labelledby` referencing the label above.
- Mobile: `accessibilityRole="adjustable"` on the value display with `onAccessibilityAction`.

## Code Example

```tsx
function Stepper({ value, min = 0, max = 99, step = 1, onChange, label }) {
  return (
    <div role="group" aria-label={label} className="flex flex-col gap-1">
      <span className="text-sm font-medium text-[--cv-text-primary]">{label}</span>
      <div className="inline-flex items-center border border-[--cv-border-default] rounded-[--cv-radius-md] overflow-hidden">
        <button
          aria-label={`Decrease ${label}`}
          aria-disabled={value <= min}
          onClick={() => value > min && onChange(value - step)}
          className="w-10 h-10 flex items-center justify-center text-[--cv-text-primary] hover:bg-[--cv-bg-surface-elevated] disabled:text-[--cv-text-tertiary]"
        >
          <Minus size={16} aria-hidden="true" />
        </button>
        <span className="min-w-[40px] text-center text-base font-medium text-[--cv-text-primary] border-x border-[--cv-border-default]">
          {value}
        </span>
        <button
          aria-label={`Increase ${label}`}
          aria-disabled={value >= max}
          onClick={() => value < max && onChange(value + step)}
          className="w-10 h-10 flex items-center justify-center text-[--cv-text-primary] hover:bg-[--cv-bg-surface-elevated] disabled:text-[--cv-text-tertiary]"
        >
          <Plus size={16} aria-hidden="true" />
        </button>
      </div>
    </div>
  );
}
```

## Anti-Patterns

- Never use Stepper for ranges larger than ~20 — a text input is faster.
- Never silently clamp values without visual feedback at the boundary (disabled button state is the feedback).
- Never omit the label — a + / – without context is meaningless to screen reader and non-technical users alike.
