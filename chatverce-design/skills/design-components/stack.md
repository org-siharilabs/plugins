
# Stack

**Gallery reference:** https://component.gallery/components/stack/

## When to Use

Use Stack as a semantic layout wrapper whenever you need uniform spacing between a list of children — form field groups, card content sections, button rows, or any vertical sequence of elements. Using Stack ensures spacing always comes from `cv-space-*` tokens rather than ad-hoc margin values.

## Chatverce Styling

- Vertical (default): `flex flex-col gap-[--cv-space-{n}]`
- Horizontal: `flex flex-row gap-[--cv-space-{n}]`
- Gap sizes use `cv-space-*` tokens exclusively — never arbitrary pixel values
- Common gaps: `cv-space-2` (8px) for tight groups, `cv-space-4` (16px) for standard, `cv-space-6` (24px) for sections
- No padding, no background — Stack is purely a spacing primitive

## Accessibility

Stack is a layout utility with no semantic role. It renders as a `<div>` (or `<View>` on mobile). Apply semantic roles to the content within the stack, not to the stack itself.

## Code Example

```tsx
{/* Vertical stack — form fields */}
<div className="flex flex-col gap-4">
  <div className="flex flex-col gap-1">
    <label htmlFor="name" className="text-sm font-medium text-[--cv-text-primary]">Name</label>
    <input id="name" type="text" className="border border-[--cv-border-default] rounded-[--cv-radius-md] px-3 py-2" />
  </div>
  <div className="flex flex-col gap-1">
    <label htmlFor="email" className="text-sm font-medium text-[--cv-text-primary]">Email</label>
    <input id="email" type="email" className="border border-[--cv-border-default] rounded-[--cv-radius-md] px-3 py-2" />
  </div>
</div>

{/* Horizontal stack — button row */}
<div className="flex flex-row gap-3">
  <button className="bg-[--cv-bg-accent] text-[--cv-text-on-accent] rounded-[--cv-radius-md] px-4 py-2">Save</button>
  <button className="border border-[--cv-border-default] text-[--cv-text-primary] rounded-[--cv-radius-md] px-4 py-2">Cancel</button>
</div>
```
