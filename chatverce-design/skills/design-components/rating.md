
# Rating

**Gallery reference:** https://component.gallery/components/rating/

## When to Use

Use Rating to display or collect a 1–5 star satisfaction score — CSAT ratings on resolved conversations, agent performance feedback, or user satisfaction surveys. Use the read-only variant for displaying aggregated scores; use the interactive variant for collection.

## Chatverce Styling

- Star icon: `Star` from Lucide, 20px
- Filled star: `cv-bg-accent` fill color (teal), or `#F59E0B` (amber) when a warm satisfaction color is appropriate — confirm with design lead per context
- Empty star: `cv-text-tertiary` stroke only
- Interactive stars: 44px touch target per star, hover fills stars up to hovered position
- Read-only: `aria-hidden="true"` on all stars, value communicated via visually hidden text

## Accessibility

- Interactive: use `role="radiogroup"` with 5 `role="radio"` stars. `aria-checked="true"` on selected and lower stars.
- Read-only: wrap stars with `aria-hidden="true"` and include `<span className="sr-only">Rating: 4 out of 5 stars</span>`.
- Do not convey rating only through color — the filled/empty visual state also changes stroke.

## Code Example

```tsx
import { Star } from "lucide-react";

{/* Read-only display */}
function RatingDisplay({ value, max = 5 }) {
  return (
    <span className="flex items-center gap-0.5">
      {Array.from({ length: max }).map((_, i) => (
        <Star
          key={i}
          size={20}
          aria-hidden="true"
          className={i < value ? "fill-[--cv-bg-accent] text-[--cv-bg-accent]" : "text-[--cv-text-tertiary]"}
        />
      ))}
      <span className="sr-only">Rating: {value} out of {max} stars</span>
    </span>
  );
}

{/* Usage */}
<RatingDisplay value={4} />
```
