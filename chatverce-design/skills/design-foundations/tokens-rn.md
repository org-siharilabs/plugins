# React Native Token Constants

> **Use case:** For `StyleSheet.create` when NativeWind classes can't be used
> (e.g., Reanimated interpolation, dynamic styles, imperative animations).
>
> These constants mirror the design tokens in `primitives.md`, `semantic-light.md`,
> and `semantic-dark.md`. Hex values are identical to `tokens-css.md` and `tokens-tailwind.md`.
>
> **Usage:**
> ```typescript
> import { getThemeTokens, spacing, radii, duration } from './tokens-rn';
>
> const tokens = getThemeTokens(colorScheme === 'dark');
>
> const styles = StyleSheet.create({
>   card: {
>     backgroundColor: tokens.bg.surface,
>     borderRadius: radii.md,
>     padding: spacing[4],
>   },
> });
> ```

---

```typescript
// tokens-rn.ts — Chatverce Design System (CDG v2.0)
// React Native / Reanimated token constants.
// Generated from: primitives.md + semantic-light.md + semantic-dark.md

// ─────────────────────────────────────────────────────────────────────────────
// PRIMITIVE SCALES
// Raw values — do not reference in application code.
// Use semantic tokens (via getThemeTokens) or named constants below instead.
// ─────────────────────────────────────────────────────────────────────────────
export const primitives = {
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
    600: '#DC2626', // Error text (light)
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
    700: '#B45309', // Warning text (light)
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
    700: '#047857', // Success text (light)
    800: '#065F46',
    900: '#064E3B',
    950: '#022C22', // Dark-mode subtle success background
  },
} as const;

// ─────────────────────────────────────────────────────────────────────────────
// SEMANTIC TOKENS — LIGHT & DARK
// Use getThemeTokens(isDark) to get the correct set at runtime.
// ─────────────────────────────────────────────────────────────────────────────
export const semantic = {
  light: {
    bg: {
      primary:           '#FAFBFC', // cv-gray-50   — page/app background
      surface:           '#FFFFFF', // white        — cards, panels, dialogs
      surfaceElevated:   '#FFFFFF', // white        — modals, dropdowns, popovers
      surfaceSunken:     '#F1F3F5', // cv-gray-100  — input fields, inset areas
      accent:            '#2EC4B6', // cv-teal-500  — primary CTA, active nav
      accentSubtle:      '#EDFAF8', // cv-teal-50   — AI glow backgrounds
      accentHover:       '#24A99D', // cv-teal-600  — hover/pressed state
      destructive:       '#EF4444', // cv-red-500   — delete/destructive backgrounds
      destructiveSubtle: '#FEF2F2', // cv-red-50    — error banners, toasts
    },
    text: {
      primary:         '#1A1D23', // cv-gray-900  — body text, headings — Ink
      secondary:       '#64748B', // cv-gray-500  — descriptions, labels — Slate
      tertiary:        '#94A3B8', // cv-gray-400  — placeholders, disabled — Mist
      onAccent:        '#FFFFFF', // white        — text on teal backgrounds
      onDestructive:   '#FFFFFF', // white        — text on red backgrounds
      link:            '#24A99D', // cv-teal-600  — inline links
      success:         '#047857', // cv-green-700 — success messages
      warning:         '#B45309', // cv-amber-700 — warning messages
      error:           '#DC2626', // cv-red-600   — error messages
    },
    border: {
      default: '#E2E8F0', // cv-gray-200  — dividers, default input borders — Frost
      strong:  '#CBD5E1', // cv-gray-300  — emphasized borders, focused inputs
      accent:  '#2EC4B6', // cv-teal-500  — focused accent elements
      error:   '#EF4444', // cv-red-500   — invalid input borders
    },
    status: {
      success: '#10B981', // cv-green-500 — agent live, task complete — Meadow
      warning: '#F59E0B', // cv-amber-500 — attention needed — Amber
      error:   '#EF4444', // cv-red-500   — failures, destructive — Coral
      info:    '#2EC4B6', // cv-teal-500  — informational, AI activity
    },
    overlay: {
      scrim:       'rgba(26, 29, 35, 0.5)', // cv-gray-900 @ 50% — modal backdrop
      shadowColor: '#000000',               // for shadowColor prop
    },
  },

  dark: {
    bg: {
      primary:           '#060B15', // cv-navy-950  — page/app background
      surface:           '#0A1120', // cv-navy-900  — cards, panels, dialogs
      surfaceElevated:   '#111C33', // cv-navy-800  — modals, dropdowns, popovers
      surfaceSunken:     '#060B15', // cv-navy-950  — input fields, inset areas
      accent:            '#42CBB9', // cv-teal-400  — primary CTA (brighter for dark)
      accentSubtle:      '#052623', // cv-teal-950  — AI glow backgrounds
      accentHover:       '#61D4C9', // cv-teal-300  — hover/pressed state
      destructive:       '#F87171', // cv-red-400   — delete/destructive backgrounds
      destructiveSubtle: '#450A0A', // cv-red-950   — error banners, toasts
    },
    text: {
      primary:         '#FAFBFC', // cv-gray-50   — body text, headings on dark
      secondary:       '#94A3B8', // cv-gray-400  — descriptions, labels, timestamps
      tertiary:        '#64748B', // cv-gray-500  — placeholders, disabled
      onAccent:        '#060B15', // cv-navy-950  — text on teal backgrounds
      onDestructive:   '#FFFFFF', // white        — text on red backgrounds
      link:            '#42CBB9', // cv-teal-400  — inline links
      success:         '#34D399', // cv-green-400 — success messages
      warning:         '#FBBF24', // cv-amber-400 — warning messages
      error:           '#F87171', // cv-red-400   — error messages
    },
    border: {
      default: '#1B2A4A', // cv-navy-700  — dividers, default input borders
      strong:  '#2A3D5E', // cv-navy-600  — emphasized borders, focused inputs
      accent:  '#42CBB9', // cv-teal-400  — focused accent elements
      error:   '#F87171', // cv-red-400   — invalid input borders
    },
    status: {
      success: '#34D399', // cv-green-400 — agent live, task complete
      warning: '#FBBF24', // cv-amber-400 — attention needed
      error:   '#F87171', // cv-red-400   — failures, destructive
      info:    '#42CBB9', // cv-teal-400  — informational, AI activity
    },
    overlay: {
      scrim:       'rgba(0, 0, 0, 0.6)', // black @ 60% — deeper scrim for dark
      shadowColor: '#000000',
    },
  },
} as const;

// ─────────────────────────────────────────────────────────────────────────────
// SPACING — 4px base grid
// Values are numbers (React Native StyleSheet uses unitless numbers, not px).
// ─────────────────────────────────────────────────────────────────────────────
export const spacing = {
  1:  4,   //  4px — icon padding, tight spacing
  2:  8,   //  8px — compact elements
  3:  12,  // 12px — small gaps
  4:  16,  // 16px — base unit, standard padding
  5:  20,  // 20px
  6:  24,  // 24px — section padding, card padding
  7:  28,  // 28px
  8:  32,  // 32px — large element spacing
  9:  36,  // 36px
  10: 40,  // 40px — component heights (buttons, inputs)
  11: 44,  // 44px — minimum tap target (Apple HIG)
  12: 48,  // 48px — large tap targets, nav items
  14: 56,  // 56px — FAB size, hero spacing
  16: 64,  // 64px — section breaks
  18: 72,  // 72px
  20: 80,  // 80px — hero/banner padding
  22: 88,  // 88px
  24: 96,  // 96px — max section padding
} as const;

// ─────────────────────────────────────────────────────────────────────────────
// BORDER RADII — unitless numbers for React Native
// ─────────────────────────────────────────────────────────────────────────────
export const radii = {
  sm:   6,    // Small: badges, chips, tight elements
  md:   8,    // Medium: buttons, inputs, cards
  lg:   12,   // Large: modals, panels, drawers
  full: 9999, // Full: pill buttons, avatar rings, toggles
} as const;

// ─────────────────────────────────────────────────────────────────────────────
// MOTION — duration in milliseconds
// Use with Reanimated withTiming({ duration: duration.normal }) or
// Animated.timing({ duration: duration.normal }).
// ─────────────────────────────────────────────────────────────────────────────
export const duration = {
  fast:   150, // Micro-interactions: hover, focus rings
  normal: 300, // Standard transitions: modals, drawers
  slow:   500, // Emphasis animations: onboarding, empty states
} as const;

// ─────────────────────────────────────────────────────────────────────────────
// MOTION — easing (Reanimated Easing equivalents)
// import { Easing } from 'react-native-reanimated';
// ─────────────────────────────────────────────────────────────────────────────
export const easing = {
  // Easing.bezier(0.4, 0, 0.2, 1) — standard ease-in-out
  default: { x1: 0.4, y1: 0, x2: 0.2, y2: 1 },
  // Easing.bezier(0, 0, 0.2, 1) — decelerate (elements entering)
  out:     { x1: 0, y1: 0, x2: 0.2, y2: 1 },
  // Easing.bezier(0.4, 0, 1, 1) — accelerate (elements leaving)
  in:      { x1: 0.4, y1: 0, x2: 1, y2: 1 },
} as const;

// ─────────────────────────────────────────────────────────────────────────────
// Z-INDEX
// ─────────────────────────────────────────────────────────────────────────────
export const zIndex = {
  dropdown: 100, // Dropdowns, select menus
  sticky:   200, // Sticky headers, frozen table columns
  overlay:  300, // Drawer / sheet backdrops
  modal:    400, // Modal dialogs
  popover:  500, // Popovers, command palettes
  toast:    600, // Toast notifications
  tooltip:  700, // Tooltips — always on top
} as const;

// ─────────────────────────────────────────────────────────────────────────────
// CHANNEL CONFIG
// Fixed external platform brand colors. Never use as primary/accent.
// Chatverce identity is navy + teal.
// ─────────────────────────────────────────────────────────────────────────────
export const channelConfig = {
  whatsapp: {
    color: '#25D366', // WhatsApp brand green
    label: 'WhatsApp',
  },
  telegram: {
    color: '#26A5E4', // Telegram brand blue
    label: 'Telegram',
  },
  webchat: {
    color: '#64748B', // cv-gray-500 — neutral web indicator
    label: 'Web Chat',
  },
  telephone: {
    color: '#1B2A4A', // cv-navy-700 — navy telephone indicator
    label: 'Telephone',
  },
  email: {
    color: '#1B2A4A', // cv-navy-700 — navy email indicator
    label: 'Email',
  },
} as const;

// ─────────────────────────────────────────────────────────────────────────────
// THEME TOKEN ACCESSOR
// Returns the correct semantic token set for the active color scheme.
//
// Usage with React Native / Expo:
//   import { useColorScheme } from 'react-native';
//   const colorScheme = useColorScheme();
//   const tokens = getThemeTokens(colorScheme === 'dark');
//
// Usage with Expo Router + system dark mode:
//   import { useColorScheme } from 'expo-router';
//   const { colorScheme } = useColorScheme();
//   const tokens = getThemeTokens(colorScheme === 'dark');
// ─────────────────────────────────────────────────────────────────────────────
export function getThemeTokens(isDark: boolean): typeof semantic.light {
  return isDark ? semantic.dark : semantic.light;
}

// ─────────────────────────────────────────────────────────────────────────────
// TYPE EXPORTS
// ─────────────────────────────────────────────────────────────────────────────
export type Primitives   = typeof primitives;
export type Semantic     = typeof semantic;
export type ThemeTokens  = typeof semantic.light; // light and dark share the same shape
export type Spacing      = typeof spacing;
export type Radii        = typeof radii;
export type Duration     = typeof duration;
export type ChannelId    = keyof typeof channelConfig;
```
