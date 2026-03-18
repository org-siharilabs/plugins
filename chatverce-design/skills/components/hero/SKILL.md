---
name: hero
description: Large, prominent banner at the top of a page combining a headline, subtext, and a primary CTA
user_invocable: false
---

# Hero

**Gallery reference:** https://component.gallery/components/hero/
**Aliases:** Banner, Jumbotron, Page hero, Feature banner

## When to Use

Use Hero on marketing pages, landing pages, or feature introduction screens to establish the page's purpose immediately and guide users toward the primary action. A Hero earns its vertical real estate by presenting the single most important message on the screen.

## When NOT to Use

Do not use Hero inside app dashboards or transactional screens — it wastes space users need for tasks. Do not use multiple Heroes on one page. Do not use Hero when there is no clear primary action to drive toward.

## Anatomy

- **Container:** Full-width section, `cv-bg-accent` or `cv-bg-primary`, or a background image with overlay.
- **Headline:** `h1`, `cv-text-display` or `cv-text-h1`, `cv-text-on-accent` or `cv-text-primary`.
- **Subtext (optional):** `cv-text-body-lg`, `cv-text-on-accent` at 80% opacity or `cv-text-secondary`.
- **CTA Button:** Primary Button. One only. Below the subtext.
- **Media (optional):** Illustration, image, or video on the right (desktop) or below (mobile).

## Variants

| Variant | Description |
|---------|-------------|
| Accent fill | `cv-bg-accent` background. White headline and CTA. High-contrast, brand-forward. |
| Surface | `cv-bg-surface` or `cv-bg-primary` background. Dark headline. For lighter-tone pages. |

## Chatverce Styling

- Background: `cv-bg-accent` (accent fill) or `cv-bg-primary` (surface variant)
- Headline: `cv-text-display` or `cv-text-h1`, `cv-text-on-accent` (accent) / `cv-text-primary` (surface)
- Subtext: `cv-text-body-lg`, reduced opacity or `cv-text-secondary`
- CTA: Primary Button, white label on accent background; standard Primary on surface variant
- Vertical padding: `cv-space-16` (desktop), `cv-space-10` (mobile)
- Max content width: `max-w-screen-xl mx-auto`

## Platform Considerations

Web: Full-width `<section>` with constrained inner content. Mobile: Reduce vertical padding significantly. Consider a condensed hero at the top of a scrollable screen rather than full-bleed height.

## Accessibility

- Headline must be an `<h1>` — there should be exactly one per page.
- Background images need an empty `alt=""`. Do not put content-critical information only in the background image.
- Ensure text contrast meets WCAG AA (4.5:1) against the background, especially with image overlays.
- CTA button must have a descriptive label — "Get started" is acceptable; "Click here" is not.

## Code Example

```tsx
<section className="w-full bg-[--cv-bg-accent] py-16 px-6">
  <div className="max-w-screen-xl mx-auto flex flex-col items-center text-center gap-6">
    <h1 className="text-5xl font-bold text-[--cv-text-on-accent] tracking-tight leading-tight">
      AI Agents for Every Conversation
    </h1>
    <p className="text-lg text-[--cv-text-on-accent] opacity-80 max-w-2xl">
      Build, deploy, and manage intelligent agents across WhatsApp, Telegram, and more — no code required.
    </p>
    <a
      href="/signup"
      className="bg-white text-[--cv-bg-accent] font-semibold rounded-[--cv-radius-md] px-6 py-3 min-h-[44px] flex items-center hover:opacity-90"
    >
      Get started free
    </a>
  </div>
</section>
```

## Anti-Patterns

- Never use Hero without a CTA — if there is no action to take, use a standard page heading.
- Never auto-play video in a Hero — it violates the motion principle and may distract or harm users.
- Never use Hero inside a dashboard or task-completion screen.
