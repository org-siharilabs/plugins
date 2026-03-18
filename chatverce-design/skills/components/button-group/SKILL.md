---
name: button-group
description: A set of related buttons visually grouped with shared border treatment
user_invocable: false
---

# Button Group

**Gallery reference:** https://component.gallery/components/button-group/
**Aliases:** Toolbar, Action group, Split button

## When to Use

Use Button Group when presenting 2–4 tightly related actions that operate on the same subject — for example, Bold/Italic/Underline in a text editor, or Approve/Reject on a review screen. Grouping communicates that actions are semantically related.

## When NOT to Use

Do not use Button Group for unrelated actions — separate them with spacing instead. Do not use more than 4 buttons in a group; beyond that, use a Dropdown Menu or Toolbar. Do not use Button Group for mutually exclusive selections — use Segmented Control or Radio buttons.

## Anatomy

- **Container:** `display: flex`, no gap between buttons.
- **Buttons:** Adjacent siblings. Inner buttons have no outer radius. First button: left radius only. Last button: right radius only.
- **Shared border:** Buttons separated by a single 1px `cv-border-default` divider (not double borders).

## Variants

| Variant | Description |
|---------|-------------|
| Default | Secondary-style buttons sharing a border radius. Most common. |
| Icon-only | Icon buttons grouped. Each requires its own `aria-label`. |

## Chatverce Styling

- First child: `cv-radius-md` on left corners only (`rounded-l-[--cv-radius-md]`)
- Last child: `cv-radius-md` on right corners only (`rounded-r-[--cv-radius-md]`)
- Middle children: no border radius
- Button borders: `cv-border-default`. Collapse adjacent borders with negative margin or divide utilities.
- Background: `cv-bg-surface` (resting), `cv-bg-surface-elevated` (hover)
- Text: `cv-text-primary`

## Platform Considerations

Web: Apply Tailwind `divide-x divide-[--cv-border-default]` on the container and `first:rounded-l-[--cv-radius-md] last:rounded-r-[--cv-radius-md]` on children. Mobile: Use a row `View` with a shared border. Ensure 44px min height.

## Accessibility

- Wrap in a `<div role="group" aria-label="...">` describing the group's purpose.
- Each button retains its individual `aria-label` (especially for icon-only variants).
- Keyboard: standard Tab navigation between buttons.

## Code Example

```tsx
<div
  role="group"
  aria-label="Text formatting"
  className="flex divide-x divide-[--cv-border-default] border border-[--cv-border-default] rounded-[--cv-radius-md] overflow-hidden"
>
  <button
    aria-label="Bold"
    className="px-3 py-2 bg-[--cv-bg-surface] hover:bg-[--cv-bg-surface-elevated] text-[--cv-text-primary]"
  >
    B
  </button>
  <button
    aria-label="Italic"
    className="px-3 py-2 bg-[--cv-bg-surface] hover:bg-[--cv-bg-surface-elevated] text-[--cv-text-primary]"
  >
    I
  </button>
  <button
    aria-label="Underline"
    className="px-3 py-2 bg-[--cv-bg-surface] hover:bg-[--cv-bg-surface-elevated] text-[--cv-text-primary]"
  >
    U
  </button>
</div>
```

## Anti-Patterns

- Never mix Primary and Secondary variants inside the same group — visual weight becomes incoherent.
- Never use Button Group for navigation — use Tabs.
- Never let the group span full width without intentional justification; groups imply compactness.
