
# Header

**Gallery reference:** https://component.gallery/components/header/
**Aliases:** Site header, App bar, Top bar, Masthead, Navbar

## When to Use

Use Header as the persistent top element of every full-page layout in the Chatverce web app. It anchors the user's sense of location, provides access to primary navigation, and surfaces global actions like account settings and notifications.

## When NOT to Use

Do not render Header inside modals, drawers, or focused task flows (e.g., onboarding wizard) where the full navigation would distract. Do not use Header on mobile — use the bottom Navigation bar pattern or a screen-level header component instead.

## Anatomy

- **Container:** Full-width, `cv-bg-surface`, `cv-border-default` bottom border.
- **Branding:** Chatverce logo/wordmark on the left.
- **Primary nav:** Horizontal link list, center or left-aligned.
- **Global actions:** Right side — Search, Notifications, Avatar/Account menu.
- **Sticky wrapper (optional):** `position: sticky; top: 0; z-index: var(--cv-z-sticky)`.

## Variants

| Variant | Description |
|---------|-------------|
| Static | Scrolls with content. Use for content-heavy pages. |
| Sticky | Remains fixed at top on scroll. Use for app-like screens. Apply `cv-z-sticky`. |

## Chatverce Styling

- Background: `cv-bg-surface`
- Bottom border: `cv-border-default`, 1px
- Height: 56px (web default)
- Horizontal padding: `cv-space-6` (desktop), `cv-space-4` (tablet)
- Sticky z-index: `cv-z-sticky`
- Logo color: matches brand token, never cv-accent alone

## Platform Considerations

Web: Rendered as `<header>` with `role="banner"`. Sticky variant uses `sticky top-0 z-[--cv-z-sticky]`. Use shadcn/ui navigation menu for the nav section. Mobile: Replace with a screen-specific `<View>` header, typically 44–56pt tall, with a back button and screen title only.

## Accessibility

- Use the semantic `<header>` element — it carries `role="banner"` implicitly.
- Skip link (see Skip Link component) must target the main content landmark and appear as the first focusable element before the Header.
- Nav items must be wrapped in `<nav aria-label="Main">`.

## Code Example

```tsx
<header className="sticky top-0 z-[--cv-z-sticky] w-full bg-[--cv-bg-surface] border-b border-[--cv-border-default] h-14 flex items-center px-6">
  <div className="flex items-center justify-between w-full max-w-screen-xl mx-auto">
    {/* Branding */}
    <a href="/dashboard" aria-label="Chatverce home">
      <img src="/logo.svg" alt="Chatverce" className="h-7" />
    </a>

    {/* Primary nav */}
    <nav aria-label="Main">
      <ul className="flex items-center gap-6">
        <li><a href="/agents" className="text-sm text-[--cv-text-secondary] hover:text-[--cv-text-primary]">Agents</a></li>
        <li><a href="/conversations" className="text-sm text-[--cv-text-secondary] hover:text-[--cv-text-primary]">Conversations</a></li>
      </ul>
    </nav>

    {/* Global actions */}
    <div className="flex items-center gap-3">
      <button aria-label="Notifications" className="text-[--cv-text-secondary]">
        {/* Bell icon */}
      </button>
      {/* Avatar / account menu */}
    </div>
  </div>
</header>
```

## Anti-Patterns

- Never place the Header inside a page `<main>` — it must be a sibling of `<main>`, not a child.
- Never use `position: fixed` without a matching Skip Link — keyboard users will be trapped behind the header.
- Never stack two Headers or nest a secondary header — use Breadcrumbs or a sub-nav instead.
