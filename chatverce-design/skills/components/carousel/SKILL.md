---
name: carousel
description: Horizontally scrolling content slider with navigation controls for cycling through items
user_invocable: false
---

# Carousel

**Gallery reference:** https://component.gallery/components/carousel/

## When to Use

Use Carousel to display a collection of items — images, cards, or media — in a space-constrained context where showing all items simultaneously is impractical. Common on marketing pages, onboarding tours, and feature showcases. Mobile: swipe gesture is expected.

## Chatverce Styling

- Navigation arrows: `cv-text-primary`, 40px touch target, `cv-bg-surface` with `cv-shadow-sm`
- Dot indicators: `cv-bg-accent` (active), `cv-bg-surface-sunken` (inactive), 8px, `cv-radius-full`
- Auto-play: disabled by default — Chatverce motion principle prohibits unsolicited animation
- Swipe: enabled on mobile via touch events
- Item gap: `cv-space-4`

## Accessibility

- Wrap in `role="region" aria-label="[Content] carousel"`.
- Previous/Next buttons: `aria-label="Previous"` / `aria-label="Next"`.
- Auto-play must not be used — if implemented, provide a pause button (`aria-label="Pause carousel"`).
- Each visible slide: `role="group" aria-label="Slide N of M"`.
- Respect `prefers-reduced-motion`: disable transitions and auto-advance.

## Code Example

```tsx
import useEmblaCarousel from "embla-carousel-react";

function Carousel({ slides }) {
  const [emblaRef, embla] = useEmblaCarousel({ loop: false });
  return (
    <section role="region" aria-label="Feature carousel">
      <div ref={emblaRef} className="overflow-hidden">
        <div className="flex gap-4">
          {slides.map((slide, i) => (
            <div key={i} role="group" aria-label={`Slide ${i + 1} of ${slides.length}`} className="flex-shrink-0 w-full">
              {slide}
            </div>
          ))}
        </div>
      </div>
      <div className="flex justify-center gap-2 mt-3">
        <button aria-label="Previous" onClick={() => embla?.scrollPrev()} className="p-2 text-[--cv-text-primary]">‹</button>
        <button aria-label="Next" onClick={() => embla?.scrollNext()} className="p-2 text-[--cv-text-primary]">›</button>
      </div>
    </section>
  );
}
```
