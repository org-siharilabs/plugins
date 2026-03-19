
# Color Picker

**Gallery reference:** https://component.gallery/components/color-picker/

## When to Use

Use Color Picker in settings where users need to customize brand colors — chat widget accent color, agent avatar background, or branded notification colors. This component is rarely needed in Chatverce core flows. Prefer predefined color swatches over an open picker whenever the valid values are finite.

## Chatverce Styling

- For predefined palettes: render as a row of 24px color swatches, `cv-radius-sm`, selected swatch has `cv-border-accent` 2px ring
- For free-form: use native `<input type="color">` styled with a custom trigger button showing the selected color
- Trigger: 32px × 32px swatch + hex value text, `cv-border-default` border, `cv-radius-md`

## Accessibility

- Label the input with a visible `<label>` using the Label component — always above the picker.
- `<input type="color">` carries native accessibility; pair with a text field for hex input as a fallback.
- Selected swatch in a palette: `aria-pressed="true"` or `aria-checked="true"` on the active swatch button; `aria-label="Color: #25D366"`.

## Code Example

```tsx
{/* Predefined palette */}
<fieldset className="border-0 p-0 m-0">
  <legend className="text-sm font-medium text-[--cv-text-primary] mb-2">Widget color</legend>
  <div className="flex gap-2">
    {["#25D366", "#26A5E4", "#6366F1", "#F59E0B"].map((color) => (
      <button
        key={color}
        aria-label={`Color: ${color}`}
        aria-pressed={selected === color}
        onClick={() => setSelected(color)}
        className={`w-6 h-6 rounded-sm transition-all ${selected === color ? "ring-2 ring-offset-1 ring-[--cv-border-accent]" : ""}`}
        style={{ backgroundColor: color }}
      />
    ))}
  </div>
</fieldset>
```
