
# Image

**Gallery reference:** https://component.gallery/components/image/
**Aliases:** Photo, Figure, Media

## When to Use

Use Image to display photographs, product screenshots, agent avatars, or any raster/vector media. Always define width and height to prevent layout shift. Always provide meaningful `alt` text so screen reader users receive equivalent information.

## When NOT to Use

Do not use Image for icons — use the Icon component. Do not use Image for purely decorative backgrounds — use CSS `background-image` with `aria-hidden` semantics or an empty `alt`. Do not use Image without a defined aspect ratio in a layout context — undefined dimensions cause cumulative layout shift (CLS).

## Anatomy

- **Container:** Fixed or responsive width with a defined aspect ratio (`aspect-video`, `aspect-square`, etc.)
- **`<img>` element:** `loading="lazy"` (below fold), `decoding="async"`.
- **`alt` attribute:** Meaningful description for content images; empty string `alt=""` for purely decorative images.
- **Fallback (optional):** Skeleton or placeholder background shown until image loads.

## Variants

| Variant | Description |
|---------|-------------|
| Responsive | `width: 100%`, defined aspect ratio. Adapts to container width. Default. |
| Fixed | Explicit px dimensions. For avatars, thumbnails, and UI imagery. |

## Chatverce Styling

- Border radius: `cv-radius-md` for card images; `cv-radius-full` for avatars
- Object fit: `object-cover` (fills container), `object-contain` (shows full image)
- Background placeholder: `cv-bg-surface-sunken` while loading
- Transition: 150ms opacity fade-in on load for perceived smoothness

## Platform Considerations

Web: Use Next.js `<Image>` component for automatic optimization (WebP, lazy loading, size hints). Always provide `width` and `height` props or `fill` with a positioned container. Mobile: Use `<Image>` from `expo-image` for optimized caching and cross-fade on load. Set `contentFit="cover"` or `contentFit="contain"` explicitly.

## Accessibility

- `alt` is required on every `<img>`. Empty string for decorative images; descriptive text for content images.
- Do not include "image of", "photo of" in alt text — screen readers already announce it is an image.
- If the image conveys complex information (e.g., a chart), supplement with a long description in the surrounding text.

## Code Example

```tsx
import NextImage from "next/image";

{/* Responsive card image */}
<div className="relative aspect-video w-full rounded-[--cv-radius-md] overflow-hidden bg-[--cv-bg-surface-sunken]">
  <NextImage
    src="/screenshots/agent-builder.png"
    alt="The Chatverce agent builder canvas showing a flow with three nodes"
    fill
    className="object-cover"
    loading="lazy"
  />
</div>

{/* Fixed avatar */}
<NextImage
  src="/avatars/support-agent.png"
  alt="Support Agent avatar"
  width={40}
  height={40}
  className="rounded-full object-cover"
/>
```

## Anti-Patterns

- Never omit the `alt` attribute — an `<img>` without `alt` is an accessibility violation.
- Never use `alt` text that duplicates adjacent visible text — it creates redundant announcements.
- Never set images to `width: 100%; height: auto` without also constraining the container's aspect ratio — this causes layout shift.
