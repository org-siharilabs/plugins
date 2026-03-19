
# Slider

**Gallery reference:** https://component.gallery/components/slider/
**Aliases:** Range input, Scrubber, Range slider

## When to Use

Use Slider when a user needs to select a value within a continuous range and the exact value matters less than the relative position — confidence thresholds, volume, response length, similarity scores in AI configuration. Pair with a numeric display so the exact value is always visible.

## When NOT to Use

Do not use Slider when users need to enter a precise number — use a numeric text input or Stepper instead. Do not use Slider for discrete options with specific meaning (e.g., "Low / Medium / High") — use Radio buttons or Segmented Control. Do not use Slider on forms submitted without JavaScript.

## Anatomy

- **Track:** Full-width horizontal bar. Filled portion (left of thumb): `cv-bg-accent`. Unfilled (right): `cv-bg-surface-sunken`.
- **Thumb:** Draggable circular handle. 20px visual size, 44px touch target. `cv-bg-accent` or white with `cv-border-accent`.
- **Min/Max labels (optional):** Text at track ends.
- **Current value display:** Number shown above thumb or in a separate input.

## Variants

| Variant | Description |
|---------|-------------|
| Single | One thumb. Selects a single value. Default. |
| Range | Two thumbs. Selects a min/max interval. Use for date ranges or confidence band. |

## Chatverce Styling

- Track height: 4px (web), 4px (mobile)
- Track fill (left): `cv-bg-accent`
- Track unfilled (right): `cv-bg-surface-sunken`
- Thumb: 20px, `cv-bg-accent` or white with `cv-border-accent` 2px border, `cv-shadow-sm`
- Thumb touch target: 44px × 44px (expand with padding or pseudo-element)
- Focus ring on thumb: 2px `cv-border-accent` offset 2px
- Transition: smooth fill update at `cv-duration-fast`

## Platform Considerations

Web: Use shadcn/ui `<Slider>` (Radix UI primitive). Override colors with `cv-*` token classes. Mobile: Use `@react-native-community/slider` or Expo's `Slider` from `expo-slider`. Set `minimumTrackTintColor` to `cv-bg-accent` token value and `thumbTintColor`.

## Accessibility

- Native `<input type="range">` or Radix Slider carries `role="slider"` automatically.
- `aria-valuenow`: current value.
- `aria-valuemin` and `aria-valuemax`: bounds.
- `aria-valuetext`: human-readable value (e.g., "75%" or "High").
- Keyboard: Arrow keys adjust value; Page Up/Down for larger steps; Home/End for min/max.
- Mobile: `accessibilityRole="adjustable"`, `accessibilityValue={{ min, max, now }}`, `onAccessibilityAction` for increment/decrement.

## Code Example

```tsx
import * as SliderPrimitive from "@radix-ui/react-slider";

function CvSlider({ value, min = 0, max = 100, step = 1, onChange, label }) {
  return (
    <div className="flex flex-col gap-2">
      <div className="flex justify-between">
        <span className="text-sm font-medium text-[--cv-text-primary]">{label}</span>
        <span className="text-sm text-[--cv-text-secondary]">{value[0]}</span>
      </div>
      <SliderPrimitive.Root
        value={value}
        min={min}
        max={max}
        step={step}
        onValueChange={onChange}
        className="relative flex items-center w-full h-5"
        aria-label={label}
      >
        <SliderPrimitive.Track className="relative h-1 grow rounded-full bg-[--cv-bg-surface-sunken]">
          <SliderPrimitive.Range className="absolute h-full rounded-full bg-[--cv-bg-accent]" />
        </SliderPrimitive.Track>
        <SliderPrimitive.Thumb
          className="block h-5 w-5 rounded-full bg-[--cv-bg-accent] shadow-sm border-2 border-white focus:outline-none focus-visible:ring-2 focus-visible:ring-[--cv-border-accent]"
        />
      </SliderPrimitive.Root>
    </div>
  );
}
```

## Anti-Patterns

- Never use Slider without displaying the current value — users cannot accurately read a position without a number.
- Never make the thumb smaller than 44px touch target on mobile.
- Never use Slider for a binary choice — use Toggle.
