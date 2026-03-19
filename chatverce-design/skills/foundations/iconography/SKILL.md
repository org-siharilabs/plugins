---
name: iconography
description: Lucide icon standards — sizing, stroke weight, touch targets, fill rules, and channel icon specifications
user_invocable: false
---

# Iconography Foundation

## Icon Set

Chatverce uses **Lucide Icons** exclusively. Lucide's design language — thin, consistent stroke weight, geometric precision — matches the professional tone of the product. Do not mix in icons from other libraries (Heroicons, Phosphor, Material, etc.). Inconsistent icon families create visual noise even when the individual icons look fine in isolation.

Lucide provides thousands of icons. If a concept does not exist in Lucide, work with the design system owner to select the closest semantic match before considering a custom icon.

## Sizing

Chatverce uses three icon sizes. Do not use intermediate values.

| Size | px | Stroke Width | Use |
|---|---|---|---|
| **Default** | 20px | 1.5px | Navigation, buttons, list items, form fields, inline actions — the vast majority of icons |
| **Small** | 16px | 1.5px | Badges, inline text icons, dense table cells, metadata labels |
| **Large** | 48px | 1.5px | Empty state illustrations, hero moments, onboarding |

Maintain 1.5px stroke across all sizes. Do not scale stroke width proportionally with icon size — the visual weight of the icon library is calibrated at 1.5px.

```tsx
// Web — lucide-react
import { MessageSquare } from 'lucide-react';

<MessageSquare size={20} strokeWidth={1.5} />        // default
<MessageSquare size={16} strokeWidth={1.5} />        // small
<MessageSquare size={48} strokeWidth={1.5} />        // large
```

```tsx
// Mobile — lucide-react-native
import { MessageSquare } from 'lucide-react-native';

<MessageSquare size={20} strokeWidth={1.5} color={colors.icon} />
```

## Touch Targets

On mobile, a 20px icon alone is too small to tap reliably. The minimum tappable area is **44px × 44px** per Apple HIG and Android Material guidelines.

Add padding around the icon — do not enlarge the icon itself:

```tsx
// Mobile: 20px icon inside a 44px touch target
<TouchableOpacity
  style={{
    width: 44,
    height: 44,
    alignItems: 'center',
    justifyContent: 'center',
  }}
  onPress={handlePress}
>
  <ChevronRight size={20} strokeWidth={1.5} color={colors.icon} />
</TouchableOpacity>
```

On web, icon buttons should have a minimum clickable area of 44px via padding, not just the icon's rendered size.

## Fill Rules

Lucide icons are outline icons by default. This is the correct default state.

| State | Style | Rule |
|---|---|---|
| Default / inactive | Outline (no fill) | Always outline unless the item is active or selected |
| Active / selected | Filled variant | Use only to communicate the current active state |

Examples:
- Navigation tab — inactive: outline `Home`, active: filled `Home`
- Bookmarked item — not saved: outline `Bookmark`, saved: filled `Bookmark`
- Liked — not liked: outline `Heart`, liked: filled `Heart`

Never use fill for decoration or emphasis. Fill communicates "this is selected" — using it arbitrarily breaks that semantic contract.

Not every Lucide icon has a filled variant. If the active state requires a filled icon and Lucide does not provide one, use a background highlight or color change on the outline icon instead of reaching for a different icon library.

## Icon-Text Pairing

When an icon appears alongside text (button labels, list item labels, nav links), follow these rules:

- **Gap:** 8px between icon and text. No more, no less.
- **Alignment:** Icon vertically centered to the text's cap height (not the full line height). In Flexbox: `align-items: center` handles this correctly for single-line text.
- **Never offset manually** — do not use `margin-top` hacks to align icon to text. Fix the layout system instead.

```tsx
// Web
<button className="flex items-center gap-2">
  <PlusCircle size={20} strokeWidth={1.5} />
  <span>Add Channel</span>
</button>
```

```tsx
// Mobile
<View style={{ flexDirection: 'row', alignItems: 'center', gap: 8 }}>
  <PlusCircle size={20} strokeWidth={1.5} color={colors.icon} />
  <Text style={styles.label}>Add Channel</Text>
</View>
```

## Channel Icons

Channel icons are a special category. They identify the communication platform (WhatsApp, Telegram, etc.) and carry brand-recognition weight. They are not standard Lucide icons — they are custom or brand-sourced SVGs.

### Channel Color Rules

Channel colors are **the only place channel-specific colors appear in the UI.** Do not use WhatsApp green, Telegram blue, or any other channel color outside of the channel icon itself.

| Channel | Icon Color | Notes |
|---|---|---|
| WhatsApp | `#25D366` | Official WhatsApp green |
| Telegram | `#2AABEE` | Official Telegram blue |
| Webchat | `cv-color-teal-500` | Chatverce's own teal — this is the Chatverce brand channel |
| Phone / Voice | `#6C757D` | Neutral gray — telephony is channel-agnostic |
| Email | `#EA4335` | Conventional email red (Gmail-derived convention) |

These colors are used on the icon badge/logo only. Channel-related UI elements (rows, cards, labels) use the standard Chatverce palette — not the channel color.

### Usage

- Render channel icons at 20px for inline contexts (list rows, channel selector), 24px for prominent placements (channel header).
- Always pair the channel icon with the channel name as a text label — never icon alone as the only identifier.
- Channel icons must include an accessible `aria-label` (web) or `accessibilityLabel` (mobile) with the channel name.

```tsx
// Web example
<img
  src="/icons/whatsapp.svg"
  width={20}
  height={20}
  aria-label="WhatsApp"
  style={{ color: '#25D366' }}
/>
```

```tsx
// Mobile example
<WhatsAppIcon
  width={20}
  height={20}
  accessibilityLabel="WhatsApp"
/>
```

## Platform Libraries

| Platform | Package |
|---|---|
| Web | `lucide-react` |
| Mobile (React Native / Expo) | `lucide-react-native` |

Both packages expose the same icon names and props API. The only required difference is the import path and the addition of the `color` prop on mobile (React Native SVGs require explicit color; they do not inherit CSS `currentColor`).

Always pass `color` explicitly on mobile:

```tsx
// Mobile — always pass color prop explicitly
import { Settings } from 'lucide-react-native';
import { useTheme } from '@/theme';

const { colors } = useTheme();
<Settings size={20} strokeWidth={1.5} color={colors.icon} />
```

On web, icons inherit `currentColor` from CSS, so they respond to text color and Tailwind color utilities correctly:

```tsx
// Web — color inherited via CSS currentColor
import { Settings } from 'lucide-react';

<Settings size={20} strokeWidth={1.5} className="text-cv-fg-secondary" />
```
