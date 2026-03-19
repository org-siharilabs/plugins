
# Skip Link

**Gallery reference:** https://component.gallery/components/skip-link/

## When to Use

Include a Skip Link on every web page with a persistent Header or navigation. It must be the first focusable element in the DOM — placed before the Header. Without it, keyboard users must Tab through every navigation item on every page load to reach the main content, which is a WCAG 2.1 Level A failure.

## Chatverce Styling

- Visually hidden by default; visible only on `:focus`
- Position: fixed, top-left corner, high `z-index` (`cv-z-overlay`)
- Appearance on focus: `cv-bg-accent`, `cv-text-on-accent`, `cv-radius-md`, `cv-space-3` padding, `cv-shadow-md`
- Text: "Skip to main content"

## Accessibility

- Must be the first focusable element in the DOM — before `<header>`.
- Target `href` must point to the `id` of the `<main>` element (e.g., `href="#main-content"`).
- The `<main>` element must have `id="main-content"` and `tabIndex={-1}` so focus lands correctly.
- Do not make the skip link permanently visible — visible-on-focus is the standard pattern.

## Code Example

```tsx
{/* In _app.tsx or root layout — before <Header /> */}
<a
  href="#main-content"
  className="sr-only focus:not-sr-only focus:fixed focus:top-4 focus:left-4 focus:z-[--cv-z-overlay] focus:bg-[--cv-bg-accent] focus:text-[--cv-text-on-accent] focus:rounded-[--cv-radius-md] focus:px-4 focus:py-2 focus:shadow-md focus:outline-none"
>
  Skip to main content
</a>

<Header />

<main id="main-content" tabIndex={-1} className="outline-none">
  {/* page content */}
</main>
```
