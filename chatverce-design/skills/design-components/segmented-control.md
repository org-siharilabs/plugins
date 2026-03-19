
# Segmented Control

**Gallery reference:** https://component.gallery/components/segmented-control/
**Aliases:** Toggle group, Button tabs, Pill selector, Filter pills

## When to Use

Use Segmented Control for a small set of 2–5 mutually exclusive options where switching between them changes the view or filters displayed data — for example, "All / Active / Paused" on an agents list, or "Day / Week / Month" on analytics. It is especially common on mobile for filter controls.

## When NOT to Use

Do not use Segmented Control for more than 5 options — use a Select or Dropdown Menu instead. Do not use it for navigation between sections of a page — use Tabs. Do not use it for binary on/off settings — use Toggle.

## Anatomy

- **Container:** Single row, `cv-radius-md`, `cv-border-default` border, `cv-bg-surface-sunken` background.
- **Segments:** Equal-width children. Each is a `<button>` with `role="radio"`.
- **Selected segment:** `cv-bg-accent`, `cv-text-on-accent`, `cv-radius-sm` inset.
- **Group role:** Container carries `role="radiogroup"`.

## Variants

| Variant | Description |
|---------|-------------|
| Text-only | Label text in each segment. Default. |
| Icon + text | 16px icon + label. Use when icons add meaning, not just decoration. |

## Chatverce Styling

- Container: `cv-bg-surface-sunken`, `cv-border-default` (1px border), `cv-radius-md`, `cv-space-1` padding
- Selected: `cv-bg-accent`, `cv-text-on-accent`, `cv-radius-sm`, `cv-shadow-sm`
- Unselected: transparent bg, `cv-text-secondary`, hover `cv-text-primary`
- Font: 14px, weight 500
- Height: 36px (web), 40px (mobile)
- Transition: `cv-duration-normal` on selection pill

## Platform Considerations

Web: Use `role="radiogroup"` + `role="radio"` pattern with `aria-checked`. Mobile: Segmented Control is a native iOS pattern (UISegmentedControl). In React Native, replicate with a row of Pressable components. On Android, Segmented Control is less conventional — evaluate if Tabs or a Chip group is more appropriate for the context.

## Accessibility

- Container: `role="radiogroup"` with `aria-label` describing what is being selected.
- Each segment: `role="radio"`, `aria-checked="true|false"`.
- Keyboard (web): Arrow keys move between segments within the group; Tab moves focus in/out.
- Mobile: `accessibilityRole="radio"`, `accessibilityState={{ checked: bool }}`.

## Code Example

```tsx
function SegmentedControl({ options, value, onChange }) {
  return (
    <div
      role="radiogroup"
      aria-label="Agent status filter"
      className="flex p-1 bg-[--cv-bg-surface-sunken] border border-[--cv-border-default] rounded-[--cv-radius-md]"
    >
      {options.map((option) => (
        <button
          key={option.value}
          role="radio"
          aria-checked={value === option.value}
          onClick={() => onChange(option.value)}
          className={`
            flex-1 px-3 py-1.5 text-sm font-medium rounded-[--cv-radius-sm] transition-colors duration-[--cv-duration-normal]
            ${value === option.value
              ? "bg-[--cv-bg-accent] text-[--cv-text-on-accent] shadow-sm"
              : "text-[--cv-text-secondary] hover:text-[--cv-text-primary]"
            }
          `}
        >
          {option.label}
        </button>
      ))}
    </div>
  );
}

// Usage
<SegmentedControl
  options={[
    { label: "All", value: "all" },
    { label: "Active", value: "active" },
    { label: "Paused", value: "paused" },
  ]}
  value={filter}
  onChange={setFilter}
/>
```

## Anti-Patterns

- Never use more than 5 segments — crowding destroys the touch target and readability.
- Never use Segmented Control for page navigation — use Tabs.
- Never allow a state where no segment is selected — one option must always be active.
