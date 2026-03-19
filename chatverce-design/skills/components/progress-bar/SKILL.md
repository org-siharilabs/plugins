---
name: progress-bar
description: Horizontal bar indicating determinate completion progress of a process or task
user_invocable: false
---

# Progress Bar

**Gallery reference:** https://component.gallery/components/progress-bar/
**Aliases:** Progress indicator, Completion bar, Loading bar

## When to Use

Use Progress Bar for processes with a known total and measurable completion — file uploads, onboarding step completion, bulk processing tasks. The bar communicates "here is how far along we are" with a concrete value. It reduces anxiety during waits by making progress visible.

## When NOT to Use

Do not use Progress Bar for indeterminate processes where the end point is unknown — use Spinner or the AI Working Indicator instead. Do not use Progress Bar for multi-step flows where individual steps are meaningful — use the Progress Indicator (step) component. Do not use for very fast operations (under 1 second) where a flash of a bar creates more confusion than reassurance.

## Anatomy

- **Track:** Full-width background container. `cv-bg-surface-sunken`, `cv-radius-full`.
- **Fill:** Expanding inner bar. `cv-bg-accent`. Width transitions as value changes.
- **Label (optional):** Percentage or step count above or beside the bar.

## Variants

| Variant | Description |
|---------|-------------|
| Default | Track + fill. No label. Minimal. Used in tight spaces. |
| Labeled | Track + fill + percentage label. For uploads and processing tasks where exact progress matters. |

## Chatverce Styling

- Track: `cv-bg-surface-sunken`, height 8px, `cv-radius-full`
- Fill: `cv-bg-accent`, `cv-radius-full`, transition width `cv-duration-normal`
- Label: `cv-text-sm`, `cv-text-secondary`, shown above-right
- Height variants: 4px (minimal), 8px (default), 12px (prominent)

## Platform Considerations

Web: Use the native `<progress>` element or a styled `<div>` with `role="progressbar"`. Mobile: Use `Animated.View` with width interpolation or the Expo `ProgressBar` component from `expo-linear-gradient`.

## Accessibility

- Set `role="progressbar"` on the fill element (or the native `<progress>`).
- `aria-valuenow`: current value (0–100 or raw number).
- `aria-valuemin="0"` and `aria-valuemax` (total).
- `aria-label` or `aria-labelledby` describing what is progressing.
- Mobile: `accessibilityRole="progressbar"`, `accessibilityValue={{ min: 0, max: 100, now: value }}`.

## Code Example

```tsx
function ProgressBar({ value, max = 100, label }: { value: number; max?: number; label: string }) {
  const percent = Math.round((value / max) * 100);
  return (
    <div className="flex flex-col gap-1">
      <div className="flex justify-between">
        <span className="text-sm text-[--cv-text-secondary]">{label}</span>
        <span className="text-sm text-[--cv-text-secondary]">{percent}%</span>
      </div>
      <div
        role="progressbar"
        aria-valuenow={percent}
        aria-valuemin={0}
        aria-valuemax={100}
        aria-label={label}
        className="w-full h-2 bg-[--cv-bg-surface-sunken] rounded-full overflow-hidden"
      >
        <div
          className="h-full bg-[--cv-bg-accent] rounded-full transition-[width] duration-[--cv-duration-normal]"
          style={{ width: `${percent}%` }}
        />
      </div>
    </div>
  );
}
```

## Anti-Patterns

- Never use Progress Bar for indeterminate processes — the bar with an unknown endpoint misleads users.
- Never animate the bar backwards (regression) without a clear reason — it breaks the mental model.
- Never omit `aria-valuenow` — the bar is invisible to screen readers without it.
