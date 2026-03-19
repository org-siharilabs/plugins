
# Fieldset

**Gallery reference:** https://component.gallery/components/fieldset/
**Aliases:** Field group, Form section, Form group

## When to Use

Use Fieldset to group logically related form controls — radio buttons, checkboxes, or a cluster of inputs that belong to one concept (e.g., "Billing Address", "Notification Preferences"). The `<legend>` provides the label for the whole group, which screen readers announce before each control within.

## When NOT to Use

Do not use Fieldset for visual grouping alone — if the fields don't share a semantic relationship, use a `<section>` with a heading instead. Do not nest Fieldsets unless absolutely necessary; nesting creates complex screen reader announcements.

## Anatomy

- **`<fieldset>`:** Semantic container. Removes browser default border via CSS.
- **`<legend>`:** The group's label. Rendered as `cv-text-label` style. First child of `<fieldset>`.
- **Controls:** Form fields or radio/checkbox groups placed below the legend.
- **Optional helper text:** Below the legend, above the controls.

## Variants

| Variant | Description |
|---------|-------------|
| Radio group | A set of mutually exclusive radio buttons with a shared question as the legend. Most common use. |
| Input cluster | Two or more related text inputs (e.g., First Name + Last Name) under a section label. |

## Chatverce Styling

- `<fieldset>`: `border-none p-0 m-0` to reset browser defaults
- `<legend>`: `cv-text-label` (14px, weight 500, `cv-text-primary`), `mb-[--cv-space-2]`
- Group spacing: `cv-space-3` between controls within the fieldset
- Optional border variant: `cv-border-default`, `cv-radius-md`, `cv-space-4` padding

## Platform Considerations

Web: Use native `<fieldset>` + `<legend>`. Reset browser default border and padding. Mobile (React Native): There is no native equivalent — use a `View` with an accessible label text above the group, and set `accessibilityRole="radiogroup"` on the wrapping View for radio groups.

## Accessibility

- `<legend>` is announced by screen readers before each control in the group, providing essential context.
- Do not hide or visually replace `<legend>` with CSS — it must be rendered in the DOM (visually-hidden is acceptable if design requires it).
- For radio groups, `<fieldset>` + `<legend>` is required for WCAG 1.3.1 compliance.

## Code Example

```tsx
<fieldset className="border-0 p-0 m-0">
  <legend className="text-sm font-medium text-[--cv-text-primary] mb-2">
    Preferred notification channel
  </legend>
  <div className="flex flex-col gap-3">
    <label className="flex items-center gap-2 text-sm text-[--cv-text-primary]">
      <input type="radio" name="channel" value="whatsapp" />
      WhatsApp
    </label>
    <label className="flex items-center gap-2 text-sm text-[--cv-text-primary]">
      <input type="radio" name="channel" value="email" />
      Email
    </label>
    <label className="flex items-center gap-2 text-sm text-[--cv-text-primary]">
      <input type="radio" name="channel" value="sms" />
      SMS
    </label>
  </div>
</fieldset>
```

## Anti-Patterns

- Never omit `<legend>` — groups without a label are inaccessible.
- Never use Fieldset as a generic card container for visual layout.
- Never rely on browser default Fieldset styling — always reset it.
