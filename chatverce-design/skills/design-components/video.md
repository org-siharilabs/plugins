
# Video

**Gallery reference:** https://component.gallery/components/video/

## When to Use

Use Video for product demonstrations, onboarding walkthroughs, or knowledge content embedded in the Chatverce app or marketing pages. Video is powerful for showing complex flows that would be harder to convey in screenshots. Captions are always required.

## Chatverce Styling

- Container: `cv-radius-lg`, `overflow-hidden`, `cv-bg-surface-sunken` (shown as background before video loads)
- Aspect ratio: `aspect-video` (16:9) by default
- Controls: visible (`controls` attribute) — do not hide native browser controls
- Poster: required — show a meaningful thumbnail before play, never a black frame
- Max width: constrained to content column width; never full-bleed on desktop without design intent

## Accessibility

- `<track kind="captions">` is required — captions for spoken content, sound effects.
- Do not auto-play — violates the Chatverce motion principle and WCAG 1.4.2.
- `aria-label` on the `<video>` element describing the video content.
- Poster image conveys what the video is about before play.

## Code Example

```tsx
<div className="aspect-video rounded-[--cv-radius-lg] overflow-hidden bg-[--cv-bg-surface-sunken]">
  <video
    controls
    poster="/thumbnails/agent-builder-demo.jpg"
    aria-label="Demo: building your first agent in Chatverce"
    className="w-full h-full object-cover"
    preload="metadata"
  >
    <source src="/videos/agent-builder-demo.mp4" type="video/mp4" />
    <track
      kind="captions"
      src="/captions/agent-builder-demo.vtt"
      srcLang="en"
      label="English"
      default
    />
    Your browser does not support the video element.
  </video>
</div>
```
