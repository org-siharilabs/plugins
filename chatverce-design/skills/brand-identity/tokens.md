## CSS Custom Properties

Paste into your global stylesheet. All Chatverce tokens as CSS custom properties:

```css
:root {
  /* Colors */
  --cv-color-primary: #1B2A4A;
  --cv-color-accent: #2EC4B6;
  --cv-color-background: #FAFBFC;
  --cv-color-surface: #F1F3F5;
  --cv-color-surface-elevated: #FFFFFF;
  --cv-color-text-primary: #1A1D23;
  --cv-color-text-secondary: #64748B;
  --cv-color-text-tertiary: #94A3B8;
  --cv-color-border: #E2E8F0;
  --cv-color-success: #10B981;
  --cv-color-warning: #F59E0B;
  --cv-color-error: #EF4444;
  --cv-color-ai-glow: rgba(46, 196, 182, 0.1);

  /* Channel Colors */
  --cv-channel-whatsapp: #25D366;
  --cv-channel-telegram: #26A5E4;
  --cv-channel-webchat: #64748B;
  --cv-channel-telephone: #1B2A4A;
  --cv-channel-email: #1B2A4A;

  /* Typography */
  --cv-font-sans: 'Inter', system-ui, -apple-system, sans-serif;
  --cv-font-mono: 'JetBrains Mono', monospace;

  /* Spacing */
  --cv-space-1: 4px;
  --cv-space-2: 8px;
  --cv-space-3: 12px;
  --cv-space-4: 16px;
  --cv-space-5: 20px;
  --cv-space-6: 24px;
  --cv-space-8: 32px;
  --cv-space-10: 40px;
  --cv-space-12: 48px;
  --cv-space-16: 64px;
  --cv-space-20: 80px;
  --cv-space-24: 96px;

  /* Radius */
  --cv-radius-sm: 6px;
  --cv-radius-md: 8px;
  --cv-radius-lg: 12px;
  --cv-radius-full: 9999px;

  /* Shadows */
  --cv-shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --cv-shadow-md: 0 4px 12px rgba(0, 0, 0, 0.08);
  --cv-shadow-lg: 0 12px 32px rgba(0, 0, 0, 0.12);
}
```

## Tailwind Config Extension

Add to `tailwind.config.js` under `theme.extend`:

```js
// Add to tailwind.config.js theme.extend
module.exports = {
  theme: {
    extend: {
      colors: {
        cv: {
          primary: '#1B2A4A',
          accent: '#2EC4B6',
          background: '#FAFBFC',
          surface: '#F1F3F5',
          'surface-elevated': '#FFFFFF',
          'text-primary': '#1A1D23',
          'text-secondary': '#64748B',
          'text-tertiary': '#94A3B8',
          border: '#E2E8F0',
          success: '#10B981',
          warning: '#F59E0B',
          error: '#EF4444',
          'ai-glow': 'rgba(46, 196, 182, 0.1)',
        },
        channel: {
          whatsapp: '#25D366',
          telegram: '#26A5E4',
          webchat: '#64748B',
          telephone: '#1B2A4A',
          email: '#1B2A4A',
        },
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', '-apple-system', 'sans-serif'],
        mono: ['JetBrains Mono', 'monospace'],
      },
      borderRadius: {
        cv: {
          sm: '6px',
          md: '8px',
          lg: '12px',
        },
      },
      boxShadow: {
        'cv-sm': '0 1px 2px rgba(0, 0, 0, 0.05)',
        'cv-md': '0 4px 12px rgba(0, 0, 0, 0.08)',
        'cv-lg': '0 12px 32px rgba(0, 0, 0, 0.12)',
      },
    },
  },
};
```

## TypeScript Constants (Mobile)

Drop-in replacement for `mobile/constants/design-system.ts`:

```typescript
// Replaces mobile/constants/design-system.ts
export const colors = {
  primary: '#1B2A4A',
  accent: '#2EC4B6',
  background: '#FAFBFC',
  surface: '#F1F3F5',
  surfaceElevated: '#FFFFFF',
  textPrimary: '#1A1D23',
  textSecondary: '#64748B',
  textTertiary: '#94A3B8',
  border: '#E2E8F0',
  success: '#10B981',
  warning: '#F59E0B',
  error: '#EF4444',
  aiGlow: 'rgba(46, 196, 182, 0.1)',
  // Channel colors
  channelWhatsapp: '#25D366',
  channelTelegram: '#26A5E4',
  channelWebchat: '#64748B',
  channelTelephone: '#1B2A4A',
  channelEmail: '#1B2A4A',
} as const;

export const spacing = {
  xs: 4, sm: 8, md: 12, lg: 16, xl: 20, '2xl': 24,
  '3xl': 32, '4xl': 40, '5xl': 48, '6xl': 64, '7xl': 80, '8xl': 96,
} as const;

export const radii = {
  sm: 6, md: 8, lg: 12, full: 9999,
} as const;

export const shadows = {
  sm: '0 1px 2px rgba(0, 0, 0, 0.05)',
  md: '0 4px 12px rgba(0, 0, 0, 0.08)',
  lg: '0 12px 32px rgba(0, 0, 0, 0.12)',
} as const;

export const avatarColors = [
  '#ef4444', '#f97316', '#eab308', '#22c55e',
  '#06b6d4', '#3b82f6', '#8b5cf6', '#ec4899',
];

export const channelConfig = {
  whatsapp: { icon: 'logo-whatsapp' as const, color: '#25D366', label: 'WhatsApp' },
  telegram: { icon: 'paper-plane' as const, color: '#26A5E4', label: 'Telegram' },
  webchat: { icon: 'globe-outline' as const, color: '#64748B', label: 'Webchat' },
  telephone: { icon: 'call-outline' as const, color: '#1B2A4A', label: 'Telephone' },
  email: { icon: 'mail-outline' as const, color: '#1B2A4A', label: 'Email' },
} as const;

// Utility functions
export function getAvatarColor(index: number): string {
  return avatarColors[index % avatarColors.length];
}

export function getInitials(name: string): string {
  return name
    .split(' ')
    .map((part) => part[0])
    .join('')
    .toUpperCase()
    .slice(0, 2);
}
```

## MUI Theme Override

Use with Material UI's `ThemeProvider`:

```typescript
import { createTheme } from '@mui/material/styles';

export const chatverceTheme = createTheme({
  palette: {
    primary: { main: '#1B2A4A' },
    secondary: { main: '#2EC4B6' },
    background: { default: '#FAFBFC', paper: '#FFFFFF' },
    text: { primary: '#1A1D23', secondary: '#64748B' },
    success: { main: '#10B981' },
    warning: { main: '#F59E0B' },
    error: { main: '#EF4444' },
    divider: '#E2E8F0',
  },
  typography: {
    fontFamily: "'Inter', system-ui, -apple-system, sans-serif",
    h1: { fontSize: '2rem', fontWeight: 600, lineHeight: 1.2 },
    h2: { fontSize: '1.5rem', fontWeight: 600, lineHeight: 1.3 },
    h3: { fontSize: '1.125rem', fontWeight: 500, lineHeight: 1.4 },
    body1: { fontSize: '1rem', fontWeight: 400, lineHeight: 1.5 },
    body2: { fontSize: '0.875rem', fontWeight: 400, lineHeight: 1.5 },
    caption: { fontSize: '0.8125rem', fontWeight: 400, lineHeight: 1.4 },
  },
  shape: { borderRadius: 8 },
  shadows: [
    'none',
    '0 1px 2px rgba(0, 0, 0, 0.05)',
    '0 4px 12px rgba(0, 0, 0, 0.08)',
    '0 12px 32px rgba(0, 0, 0, 0.12)',
    ...Array(21).fill('0 12px 32px rgba(0, 0, 0, 0.12)'),
  ] as any,
});
```
