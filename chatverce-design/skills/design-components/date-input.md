
# Date Input

**Gallery reference:** https://component.gallery/components/date-input/

## When to Use

Use Date Input when users need to type a known date — birthdays, report start dates, contract effective dates. Separate fields (DD / MM / YYYY) are more accessible and reliable than a single string field or a datepicker, especially for users who know their date exactly and don't need to navigate a calendar.

## Chatverce Styling

- Three adjacent `<input type="text">` fields: Day (2 digits), Month (2 digits), Year (4 digits)
- Each field: `cv-border-default`, `cv-radius-md`, `cv-space-2` padding, `cv-text-primary`
- Field width: Day 48px, Month 48px, Year 72px
- Separators: `/` character in `cv-text-tertiary` between fields
- Error state: `cv-border-destructive` on invalid fields + inline error message

## Accessibility

- Each field has its own `<label>` (or `aria-label`): "Day", "Month", "Year".
- Group in a `<fieldset>` with `<legend>` matching the overall date label (e.g., "Date of birth").
- `inputmode="numeric"` on each field for mobile numeric keyboard.
- `autocomplete="bday-day"`, `"bday-month"`, `"bday-year"` for birthday fields.

## Code Example

```tsx
<fieldset className="border-0 p-0 m-0">
  <legend className="text-sm font-medium text-[--cv-text-primary] mb-2">Date of birth</legend>
  <div className="flex items-center gap-1">
    <input
      aria-label="Day" inputMode="numeric" maxLength={2} placeholder="DD"
      className="w-12 text-center border border-[--cv-border-default] rounded-[--cv-radius-md] px-2 py-2 text-sm text-[--cv-text-primary]"
    />
    <span className="text-[--cv-text-tertiary]">/</span>
    <input
      aria-label="Month" inputMode="numeric" maxLength={2} placeholder="MM"
      className="w-12 text-center border border-[--cv-border-default] rounded-[--cv-radius-md] px-2 py-2 text-sm text-[--cv-text-primary]"
    />
    <span className="text-[--cv-text-tertiary]">/</span>
    <input
      aria-label="Year" inputMode="numeric" maxLength={4} placeholder="YYYY"
      className="w-16 text-center border border-[--cv-border-default] rounded-[--cv-radius-md] px-2 py-2 text-sm text-[--cv-text-primary]"
    />
  </div>
</fieldset>
```
