# CSS Custom Properties

> Complete CSS custom property definitions for all Chatverce Design System tokens.
> Import this file (or its compiled output) once at the root of your web application.
>
> **Theme switching:** Add `data-theme="dark"` to `<html>` for explicit dark mode.
> System-preference auto-switching is handled by the `prefers-color-scheme` block.
> **Reduced motion:** All duration tokens are zeroed when the user prefers reduced motion.

---

```css
/* =========================================================================
   Chatverce Design System — CSS Custom Properties (CDG v2.0)
   Generated from: primitives.md + semantic-light.md + semantic-dark.md
   ========================================================================= */

/* =========================================================================
   :root — Light theme (default) + non-color tokens
   ========================================================================= */
:root {

  /* ── Semantic: Backgrounds ──────────────────────────────────────────── */
  --cv-bg-primary:            #FAFBFC; /* cv-gray-50   — Snow, page/app background */
  --cv-bg-surface:            #FFFFFF; /* white        — Cards, panels, dialogs */
  --cv-bg-surface-elevated:   #FFFFFF; /* white        — Modals, dropdowns, popovers */
  --cv-bg-surface-sunken:     #F1F3F5; /* cv-gray-100  — Input fields, inset areas */
  --cv-bg-accent:             #2EC4B6; /* cv-teal-500  — Primary CTA backgrounds, active nav */
  --cv-bg-accent-subtle:      #EDFAF8; /* cv-teal-50   — Teal-tinted backgrounds, AI glow states */
  --cv-bg-accent-hover:       #24A99D; /* cv-teal-600  — Accent hover/pressed state */
  --cv-bg-destructive:        #EF4444; /* cv-red-500   — Delete/destructive action backgrounds */
  --cv-bg-destructive-subtle: #FEF2F2; /* cv-red-50    — Error/warning banners, toast backgrounds */

  /* ── Semantic: Text ─────────────────────────────────────────────────── */
  --cv-text-primary:          #1A1D23; /* cv-gray-900  — Body text, headings — Ink */
  --cv-text-secondary:        #64748B; /* cv-gray-500  — Descriptions, labels, timestamps — Slate */
  --cv-text-tertiary:         #94A3B8; /* cv-gray-400  — Placeholders, disabled text — Mist */
  --cv-text-on-accent:        #FFFFFF; /* white        — Text/icons on teal backgrounds */
  --cv-text-on-destructive:   #FFFFFF; /* white        — Text/icons on red backgrounds */
  --cv-text-link:             #24A99D; /* cv-teal-600  — Inline links, clickable text */
  --cv-text-success:          #047857; /* cv-green-700 — Success messages, confirmations */
  --cv-text-warning:          #B45309; /* cv-amber-700 — Warning messages, attention text */
  --cv-text-error:            #DC2626; /* cv-red-600   — Error messages, validation feedback */

  /* ── Semantic: Borders ──────────────────────────────────────────────── */
  --cv-border-default:        #E2E8F0; /* cv-gray-200  — Dividers, default input borders — Frost */
  --cv-border-strong:         #CBD5E1; /* cv-gray-300  — Emphasized borders, focused inputs */
  --cv-border-accent:         #2EC4B6; /* cv-teal-500  — Focused state on accent elements */
  --cv-border-error:          #EF4444; /* cv-red-500   — Invalid input borders */

  /* ── Semantic: Status ───────────────────────────────────────────────── */
  --cv-status-success:        #10B981; /* cv-green-500 — Agent live, task complete — Meadow */
  --cv-status-warning:        #F59E0B; /* cv-amber-500 — Attention needed — Amber */
  --cv-status-error:          #EF4444; /* cv-red-500   — Failures, destructive actions — Coral */
  --cv-status-info:           #2EC4B6; /* cv-teal-500  — Informational states, AI activity */

  /* ── Semantic: Overlay ──────────────────────────────────────────────── */
  --cv-shadow-color:          0, 0, 0;              /* Black RGB components — use as rgba(var(--cv-shadow-color), alpha) */
  --cv-overlay:               rgba(26, 29, 35, 0.5); /* cv-gray-900 @ 50% — modal scrim, drawer backdrop */

  /* ── Non-color: Typography ──────────────────────────────────────────── */
  --cv-font-sans: 'Inter', 'Inter var', ui-sans-serif, system-ui, -apple-system,
                  BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial,
                  'Noto Sans', sans-serif;
  --cv-font-mono: 'JetBrains Mono', ui-monospace, SFMono-Regular, Menlo, Monaco,
                  Consolas, 'Liberation Mono', 'Courier New', monospace;

  /* ── Non-color: Border radius ───────────────────────────────────────── */
  --cv-radius-sm:   6px;    /* Small: badges, chips, tight elements */
  --cv-radius-md:   8px;    /* Medium: buttons, inputs, cards */
  --cv-radius-lg:   12px;   /* Large: modals, panels, drawers */
  --cv-radius-full: 9999px; /* Full: pill buttons, avatar rings, toggles */

  /* ── Non-color: Spacing (4px base grid) ────────────────────────────── */
  --cv-space-1:  4px;   /*  4px — icon padding, tight spacing */
  --cv-space-2:  8px;   /*  8px — compact elements */
  --cv-space-3:  12px;  /* 12px — small gaps */
  --cv-space-4:  16px;  /* 16px — base unit, standard padding */
  --cv-space-5:  20px;  /* 20px */
  --cv-space-6:  24px;  /* 24px — section padding, card padding */
  --cv-space-7:  28px;  /* 28px */
  --cv-space-8:  32px;  /* 32px — large element spacing */
  --cv-space-9:  36px;  /* 36px */
  --cv-space-10: 40px;  /* 40px — component heights (buttons, inputs) */
  --cv-space-11: 44px;  /* 44px — minimum tap target (Apple HIG) */
  --cv-space-12: 48px;  /* 48px — large tap targets, nav items */
  --cv-space-14: 56px;  /* 56px — FAB size, hero spacing */
  --cv-space-16: 64px;  /* 64px — section breaks */
  --cv-space-18: 72px;  /* 72px */
  --cv-space-20: 80px;  /* 80px — hero/banner padding */
  --cv-space-22: 88px;  /* 88px */
  --cv-space-24: 96px;  /* 96px — max section padding */

  /* ── Non-color: Box shadows ─────────────────────────────────────────── */
  --cv-shadow-sm: 0 1px 2px 0 rgba(var(--cv-shadow-color), 0.05);
  --cv-shadow-md: 0 4px 6px -1px rgba(var(--cv-shadow-color), 0.10),
                  0 2px 4px -2px rgba(var(--cv-shadow-color), 0.10);
  --cv-shadow-lg: 0 10px 15px -3px rgba(var(--cv-shadow-color), 0.10),
                  0 4px 6px -4px rgba(var(--cv-shadow-color), 0.10);

  /* ── Non-color: Z-index scale ───────────────────────────────────────── */
  --cv-z-dropdown: 100;  /* Dropdowns, select menus */
  --cv-z-sticky:   200;  /* Sticky headers, frozen table columns */
  --cv-z-overlay:  300;  /* Drawer / sheet backdrops */
  --cv-z-modal:    400;  /* Modal dialogs */
  --cv-z-popover:  500;  /* Popovers, command palettes */
  --cv-z-toast:    600;  /* Toast notifications */
  --cv-z-tooltip:  700;  /* Tooltips — always on top */

  /* ── Non-color: Motion — duration ──────────────────────────────────── */
  --cv-duration-fast:   150ms; /* Micro-interactions: hover, focus rings */
  --cv-duration-normal: 300ms; /* Standard transitions: modals, drawers */
  --cv-duration-slow:   500ms; /* Emphasis animations: onboarding, empty states */

  /* ── Non-color: Motion — easing ─────────────────────────────────────── */
  --cv-ease-default: cubic-bezier(0.4, 0, 0.2, 1); /* Standard — ease-in-out feel */
  --cv-ease-out:     cubic-bezier(0, 0, 0.2, 1);   /* Decelerate — elements entering */
  --cv-ease-in:      cubic-bezier(0.4, 0, 1, 1);   /* Accelerate — elements leaving */

  /* ── Non-color: Breakpoints ─────────────────────────────────────────── */
  /* Reference-only; use Tailwind's `screens` config or media queries directly. */
  --cv-bp-sm:  640px;   /* Small screens: wide mobile landscape */
  --cv-bp-md:  768px;   /* Medium screens: tablets */
  --cv-bp-lg:  1024px;  /* Large screens: laptops */
  --cv-bp-xl:  1280px;  /* X-large screens: desktops */
  --cv-bp-2xl: 1536px;  /* 2x-large screens: wide monitors */
}

/* =========================================================================
   Dark theme — explicit via data attribute or class
   Usage: <html data-theme="dark"> or <html class="dark">
   ========================================================================= */
[data-theme="dark"],
.dark {

  /* ── Semantic: Backgrounds ──────────────────────────────────────────── */
  --cv-bg-primary:            #060B15; /* cv-navy-950  — Page/app background, near-black navy */
  --cv-bg-surface:            #0A1120; /* cv-navy-900  — Cards, panels, dialogs */
  --cv-bg-surface-elevated:   #111C33; /* cv-navy-800  — Modals, dropdowns, popovers */
  --cv-bg-surface-sunken:     #060B15; /* cv-navy-950  — Input fields, inset areas */
  --cv-bg-accent:             #42CBB9; /* cv-teal-400  — Primary CTA backgrounds — brighter for contrast */
  --cv-bg-accent-subtle:      #052623; /* cv-teal-950  — Teal-tinted backgrounds, AI glow states */
  --cv-bg-accent-hover:       #61D4C9; /* cv-teal-300  — Accent hover/pressed state */
  --cv-bg-destructive:        #F87171; /* cv-red-400   — Delete/destructive action backgrounds */
  --cv-bg-destructive-subtle: #450A0A; /* cv-red-950   — Error/warning banners, toast backgrounds */

  /* ── Semantic: Text ─────────────────────────────────────────────────── */
  --cv-text-primary:          #FAFBFC; /* cv-gray-50   — Body text, headings on dark */
  --cv-text-secondary:        #94A3B8; /* cv-gray-400  — Descriptions, labels, timestamps */
  --cv-text-tertiary:         #64748B; /* cv-gray-500  — Placeholders, disabled text */
  --cv-text-on-accent:        #060B15; /* cv-navy-950  — Text/icons on teal backgrounds */
  --cv-text-on-destructive:   #FFFFFF; /* white        — Text/icons on red backgrounds */
  --cv-text-link:             #42CBB9; /* cv-teal-400  — Inline links, clickable text */
  --cv-text-success:          #34D399; /* cv-green-400 — Success messages, confirmations */
  --cv-text-warning:          #FBBF24; /* cv-amber-400 — Warning messages, attention text */
  --cv-text-error:            #F87171; /* cv-red-400   — Error messages, validation feedback */

  /* ── Semantic: Borders ──────────────────────────────────────────────── */
  --cv-border-default:        #1B2A4A; /* cv-navy-700  — Dividers, default input borders */
  --cv-border-strong:         #2A3D5E; /* cv-navy-600  — Emphasized borders, focused inputs */
  --cv-border-accent:         #42CBB9; /* cv-teal-400  — Focused state on accent elements */
  --cv-border-error:          #F87171; /* cv-red-400   — Invalid input borders */

  /* ── Semantic: Status ───────────────────────────────────────────────── */
  --cv-status-success:        #34D399; /* cv-green-400 — Agent live, task complete */
  --cv-status-warning:        #FBBF24; /* cv-amber-400 — Attention needed */
  --cv-status-error:          #F87171; /* cv-red-400   — Failures, destructive actions */
  --cv-status-info:           #42CBB9; /* cv-teal-400  — Informational states, AI activity */

  /* ── Semantic: Overlay ──────────────────────────────────────────────── */
  --cv-shadow-color:          0, 0, 0;         /* Black RGB components (same as light) */
  --cv-overlay:               rgba(0, 0, 0, 0.6); /* black @ 60% — deeper scrim for dark mode */
}

/* =========================================================================
   System-preference dark mode — auto-switching without explicit class/attr
   Applies only when no explicit theme is set on the root element.
   ========================================================================= */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme]) {

    /* ── Semantic: Backgrounds ────────────────────────────────────────── */
    --cv-bg-primary:            #060B15; /* cv-navy-950  — Page/app background */
    --cv-bg-surface:            #0A1120; /* cv-navy-900  — Cards, panels, dialogs */
    --cv-bg-surface-elevated:   #111C33; /* cv-navy-800  — Modals, dropdowns, popovers */
    --cv-bg-surface-sunken:     #060B15; /* cv-navy-950  — Input fields, inset areas */
    --cv-bg-accent:             #42CBB9; /* cv-teal-400  — Primary CTA backgrounds */
    --cv-bg-accent-subtle:      #052623; /* cv-teal-950  — Teal-tinted backgrounds, AI glow */
    --cv-bg-accent-hover:       #61D4C9; /* cv-teal-300  — Accent hover/pressed state */
    --cv-bg-destructive:        #F87171; /* cv-red-400   — Destructive action backgrounds */
    --cv-bg-destructive-subtle: #450A0A; /* cv-red-950   — Error banners, toast backgrounds */

    /* ── Semantic: Text ───────────────────────────────────────────────── */
    --cv-text-primary:          #FAFBFC; /* cv-gray-50   — Body text, headings */
    --cv-text-secondary:        #94A3B8; /* cv-gray-400  — Descriptions, labels, timestamps */
    --cv-text-tertiary:         #64748B; /* cv-gray-500  — Placeholders, disabled text */
    --cv-text-on-accent:        #060B15; /* cv-navy-950  — Text/icons on teal backgrounds */
    --cv-text-on-destructive:   #FFFFFF; /* white        — Text/icons on red backgrounds */
    --cv-text-link:             #42CBB9; /* cv-teal-400  — Inline links, clickable text */
    --cv-text-success:          #34D399; /* cv-green-400 — Success messages */
    --cv-text-warning:          #FBBF24; /* cv-amber-400 — Warning messages */
    --cv-text-error:            #F87171; /* cv-red-400   — Error messages */

    /* ── Semantic: Borders ────────────────────────────────────────────── */
    --cv-border-default:        #1B2A4A; /* cv-navy-700  — Dividers, default borders */
    --cv-border-strong:         #2A3D5E; /* cv-navy-600  — Emphasized borders */
    --cv-border-accent:         #42CBB9; /* cv-teal-400  — Focused accent elements */
    --cv-border-error:          #F87171; /* cv-red-400   — Invalid input borders */

    /* ── Semantic: Status ─────────────────────────────────────────────── */
    --cv-status-success:        #34D399; /* cv-green-400 — Agent live, task complete */
    --cv-status-warning:        #FBBF24; /* cv-amber-400 — Attention needed */
    --cv-status-error:          #F87171; /* cv-red-400   — Failures, destructive */
    --cv-status-info:           #42CBB9; /* cv-teal-400  — Informational, AI activity */

    /* ── Semantic: Overlay ────────────────────────────────────────────── */
    --cv-shadow-color:          0, 0, 0;
    --cv-overlay:               rgba(0, 0, 0, 0.6); /* black @ 60% */
  }
}

/* =========================================================================
   Reduced motion — zero all animation/transition durations
   Users who prefer reduced motion get instant transitions.
   ========================================================================= */
@media (prefers-reduced-motion: reduce) {
  :root {
    --cv-duration-fast:   0ms; /* Zeroed: micro-interactions */
    --cv-duration-normal: 0ms; /* Zeroed: standard transitions */
    --cv-duration-slow:   0ms; /* Zeroed: emphasis animations */
  }
}
```
