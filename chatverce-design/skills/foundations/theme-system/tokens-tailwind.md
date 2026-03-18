# Tailwind Token Config

> Shared `tailwind.config.js` extension for **web (Tailwind CSS v3/v4)** and **mobile (NativeWind v4)**.
>
> **NativeWind compatibility notes:**
> - CSS variable references (`var(--cv-*)`) work for web only. On mobile, NativeWind resolves them at build time — configure `resolveConfig` in your NativeWind setup or use the React Native constants from `tokens-rn.md` for dynamic/Reanimated styles.
> - `boxShadow` tokens are web-only; use `elevation` + `shadowColor` on iOS/Android.
> - `fontFamily` arrays follow the web convention; NativeWind maps the first matching installed font on mobile.

---

```js
// tailwind.config.js — Chatverce Design (CDG) v2.0
// Shared config for web (Tailwind CSS) + mobile (NativeWind).
//
// NativeWind note: import this file in your tailwind.config.js `theme.extend`
// block. NativeWind v4 reads the same config; CSS variable references are
// resolved at build time for web and fallen back to the primitive hex values
// via the `darkMode` strategy on mobile.

/** @type {import('tailwindcss').Config} */
const cvTokens = {
  theme: {
    extend: {
      // ─────────────────────────────────────────────────────────────────────
      // COLORS
      // ─────────────────────────────────────────────────────────────────────
      colors: {
        cv: {
          // ── Primitive scales ─────────────────────────────────────────────
          // Never reference these directly in application code.
          // Use the semantic tokens below (cv-bg-*, cv-text-*, etc.) instead.
          navy: {
            50:  '#EEF1F7', // Near-white blue-gray
            100: '#D5DCE9',
            200: '#B3BFCE',
            300: '#8E9DB5',
            400: '#617090',
            500: '#3D5070',
            600: '#2A3D5E',
            700: '#1B2A4A', // ANCHOR — Midnight Navy, Chatverce primary brand
            800: '#111C33',
            900: '#0A1120',
            950: '#060B15', // Near-black navy — darkest dark-mode surface
          },
          teal: {
            50:  '#EDFAF8', // Subtle AI glow backgrounds
            100: '#C8F1EC',
            200: '#99E5DC',
            300: '#61D4C9',
            400: '#42CBB9', // Dark-mode accent
            500: '#2EC4B6', // ANCHOR — Chatverce Teal, primary CTA
            600: '#24A99D', // Hover / pressed states
            700: '#1A8880',
            800: '#116661',
            900: '#0A4440',
            950: '#052623', // Dark-mode subtle teal background
          },
          gray: {
            50:  '#FAFBFC', // ANCHOR — Snow, page background
            100: '#F1F3F5', // ANCHOR — Cloud, card/panel background
            200: '#E2E8F0', // ANCHOR — Frost, default border
            300: '#CBD5E1',
            400: '#94A3B8', // ANCHOR — Mist, placeholder/disabled text
            500: '#64748B', // ANCHOR — Slate, secondary text
            600: '#475569',
            700: '#334155',
            800: '#1E293B',
            900: '#1A1D23', // ANCHOR — Ink, primary text
            950: '#0F1117', // Near-black — darkest text/surfaces
          },
          red: {
            50:  '#FEF2F2', // Error background
            100: '#FEE2E2',
            200: '#FECACA',
            300: '#FCA5A5',
            400: '#F87171', // Dark-mode error
            500: '#EF4444', // ANCHOR — Coral, error/destructive
            600: '#DC2626', // Error text
            700: '#B91C1C',
            800: '#991B1B',
            900: '#7F1D1D',
            950: '#450A0A', // Dark-mode subtle error background
          },
          amber: {
            50:  '#FFFBEB', // Warning background
            100: '#FEF3C7',
            200: '#FDE68A',
            300: '#FCD34D',
            400: '#FBBF24', // Dark-mode warning
            500: '#F59E0B', // ANCHOR — Amber, warning
            600: '#D97706',
            700: '#B45309', // Warning text
            800: '#92400E',
            900: '#78350F',
            950: '#451A03', // Dark-mode subtle warning background
          },
          green: {
            50:  '#ECFDF5', // Success background
            100: '#D1FAE5',
            200: '#A7F3D0',
            300: '#6EE7B7',
            400: '#34D399', // Dark-mode success
            500: '#10B981', // ANCHOR — Meadow, success
            600: '#059669',
            700: '#047857', // Success text
            800: '#065F46',
            900: '#064E3B',
            950: '#022C22', // Dark-mode subtle success background
          },

          // ── Channel colors ────────────────────────────────────────────────
          // Fixed external platform brand colors. Never use as primary/accent.
          channel: {
            whatsapp:  '#25D366', // WhatsApp brand green
            telegram:  '#26A5E4', // Telegram brand blue
            webchat:   '#64748B', // cv-gray-500 — neutral web indicator
            telephone: '#1B2A4A', // cv-navy-700 — navy telephone indicator
            email:     '#1B2A4A', // cv-navy-700 — navy email indicator
          },

          // ── Semantic tokens — CSS variable references ─────────────────────
          // These resolve to the correct light/dark value at runtime via CSS
          // custom properties. On web, Tailwind generates `color: var(--cv-*)`.
          // NativeWind resolves these at build time via its theme resolver;
          // for runtime dynamic styles use tokens-rn.md constants instead.

          // Backgrounds (9 tokens)
          'bg-primary':           'var(--cv-bg-primary)',
          'bg-surface':           'var(--cv-bg-surface)',
          'bg-surface-elevated':  'var(--cv-bg-surface-elevated)',
          'bg-surface-sunken':    'var(--cv-bg-surface-sunken)',
          'bg-accent':            'var(--cv-bg-accent)',
          'bg-accent-subtle':     'var(--cv-bg-accent-subtle)',
          'bg-accent-hover':      'var(--cv-bg-accent-hover)',
          'bg-destructive':       'var(--cv-bg-destructive)',
          'bg-destructive-subtle':'var(--cv-bg-destructive-subtle)',

          // Text (9 tokens)
          'text-primary':         'var(--cv-text-primary)',
          'text-secondary':       'var(--cv-text-secondary)',
          'text-tertiary':        'var(--cv-text-tertiary)',
          'text-on-accent':       'var(--cv-text-on-accent)',
          'text-on-destructive':  'var(--cv-text-on-destructive)',
          'text-link':            'var(--cv-text-link)',
          'text-success':         'var(--cv-text-success)',
          'text-warning':         'var(--cv-text-warning)',
          'text-error':           'var(--cv-text-error)',

          // Borders (4 tokens)
          'border-default':       'var(--cv-border-default)',
          'border-strong':        'var(--cv-border-strong)',
          'border-accent':        'var(--cv-border-accent)',
          'border-error':         'var(--cv-border-error)',

          // Status (4 tokens)
          'status-success':       'var(--cv-status-success)',
          'status-warning':       'var(--cv-status-warning)',
          'status-error':         'var(--cv-status-error)',
          'status-info':          'var(--cv-status-info)',

          // Overlay (referenced in components, not a color utility class)
          'overlay':              'var(--cv-overlay)',
        },
      },

      // ─────────────────────────────────────────────────────────────────────
      // TYPOGRAPHY
      // ─────────────────────────────────────────────────────────────────────
      fontFamily: {
        // NativeWind note: list the exact font family name as registered with
        // `expo-font` / React Native's font loading system as the first entry.
        'cv-sans': [
          'Inter',
          'Inter var',
          'ui-sans-serif',
          'system-ui',
          '-apple-system',
          'BlinkMacSystemFont',
          '"Segoe UI"',
          'Roboto',
          '"Helvetica Neue"',
          'Arial',
          '"Noto Sans"',
          'sans-serif',
        ],
        'cv-mono': [
          '"JetBrains Mono"',
          '"JetBrainsMono-Regular"', // React Native font asset name
          'ui-monospace',
          'SFMono-Regular',
          'Menlo',
          'Monaco',
          'Consolas',
          '"Liberation Mono"',
          '"Courier New"',
          'monospace',
        ],
      },

      // ─────────────────────────────────────────────────────────────────────
      // BORDER RADIUS
      // ─────────────────────────────────────────────────────────────────────
      // NativeWind: borderRadius values are passed directly to StyleSheet —
      // all values here are supported natively.
      borderRadius: {
        'cv-sm':   '6px',    // Small: badges, chips, tight elements
        'cv-md':   '8px',    // Medium: buttons, inputs, cards
        'cv-lg':   '12px',   // Large: modals, panels, drawers
        'cv-full': '9999px', // Full: pill buttons, avatar rings, toggles
      },

      // ─────────────────────────────────────────────────────────────────────
      // BOX SHADOW
      // ─────────────────────────────────────────────────────────────────────
      // NativeWind NOTE: boxShadow is WEB-ONLY. On React Native use the
      // `elevation` prop (Android) and `shadowColor`/`shadowOffset`/
      // `shadowOpacity`/`shadowRadius` (iOS) from tokens-rn.md.
      // --cv-shadow-color is "0,0,0" (RGB components only, no alpha).
      boxShadow: {
        'cv-sm': '0 1px 2px 0 rgba(var(--cv-shadow-color), 0.05)',
        'cv-md': '0 4px 6px -1px rgba(var(--cv-shadow-color), 0.10), 0 2px 4px -2px rgba(var(--cv-shadow-color), 0.10)',
        'cv-lg': '0 10px 15px -3px rgba(var(--cv-shadow-color), 0.10), 0 4px 6px -4px rgba(var(--cv-shadow-color), 0.10)',
      },

      // ─────────────────────────────────────────────────────────────────────
      // Z-INDEX
      // ─────────────────────────────────────────────────────────────────────
      // NativeWind: zIndex maps directly to React Native's `zIndex` style.
      zIndex: {
        'cv-dropdown':  '100',  // Dropdowns, select menus
        'cv-sticky':    '200',  // Sticky headers, frozen table columns
        'cv-overlay':   '300',  // Drawer / sheet backdrops
        'cv-modal':     '400',  // Modal dialogs
        'cv-popover':   '500',  // Popovers, command palettes
        'cv-toast':     '600',  // Toast notifications
        'cv-tooltip':   '700',  // Tooltips — always on top
      },

      // ─────────────────────────────────────────────────────────────────────
      // TRANSITION DURATION
      // ─────────────────────────────────────────────────────────────────────
      // NativeWind note: use these values with Reanimated `withTiming` or
      // `withSpring` duration options. Import from tokens-rn.md `duration`.
      transitionDuration: {
        'cv-fast':   '150ms', // Micro-interactions: hover, focus rings
        'cv-normal': '300ms', // Standard transitions: modals, drawers
        'cv-slow':   '500ms', // Emphasis animations: onboarding, empty states
      },

      // ─────────────────────────────────────────────────────────────────────
      // TRANSITION TIMING FUNCTION (EASING)
      // ─────────────────────────────────────────────────────────────────────
      // NativeWind note: Reanimated uses Easing functions. Equivalents:
      //   cv-default → Easing.bezier(0.4, 0, 0.2, 1)
      //   cv-out     → Easing.bezier(0, 0, 0.2, 1)
      //   cv-in      → Easing.bezier(0.4, 0, 1, 1)
      transitionTimingFunction: {
        'cv-default': 'cubic-bezier(0.4, 0, 0.2, 1)', // Standard — ease-in-out feel
        'cv-out':     'cubic-bezier(0, 0, 0.2, 1)',    // Decelerate — elements entering
        'cv-in':      'cubic-bezier(0.4, 0, 1, 1)',    // Accelerate — elements leaving
      },

      // ─────────────────────────────────────────────────────────────────────
      // SPACING
      // ─────────────────────────────────────────────────────────────────────
      // 4px base grid. All values align to the 4px design grid.
      // NativeWind: spacing maps directly to React Native layout props
      // (padding, margin, width, height, gap, etc.).
      spacing: {
        'cv-1':  '4px',  //  4px — tight spacing, icon padding
        'cv-2':  '8px',  //  8px — compact elements
        'cv-3':  '12px', // 12px — small gaps
        'cv-4':  '16px', // 16px — base unit, standard padding
        'cv-5':  '20px', // 20px
        'cv-6':  '24px', // 24px — section padding, card padding
        'cv-7':  '28px', // 28px
        'cv-8':  '32px', // 32px — large element spacing
        'cv-9':  '36px', // 36px
        'cv-10': '40px', // 40px — component heights (buttons, inputs)
        'cv-11': '44px', // 44px — minimum tap target (Apple HIG)
        'cv-12': '48px', // 48px — large tap targets, nav items
        'cv-14': '56px', // 56px — FAB size, hero spacing
        'cv-16': '64px', // 64px — section breaks
        'cv-18': '72px', // 72px
        'cv-20': '80px', // 80px — hero/banner padding
        'cv-22': '88px', // 88px
        'cv-24': '96px', // 96px — max section padding
      },
    },
  },
};

module.exports = cvTokens;
```
