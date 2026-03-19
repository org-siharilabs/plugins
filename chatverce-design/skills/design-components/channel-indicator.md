
# Channel Indicator

**Gallery reference:** https://component.gallery/components/badge/
**Aliases:** Channel badge, Channel chip, Platform badge

## When to Use

Use Channel Indicator wherever the communication channel of a conversation, agent, or contact needs to be identified at a glance — conversation lists, contact detail views, agent channel configuration summaries, and analytics breakdowns. It provides instant recognition of the channel without requiring text labels in dense lists.

## When NOT to Use

Do not use Channel Indicator for general status badges or labels unrelated to communication channels. Do not use channel brand colors anywhere else in the UI — these colors are reserved exclusively for Channel Indicators. Do not replace visible channel names entirely with icons in contexts where channel literacy cannot be assumed.

## Anatomy

- **Container:** Small rounded pill or circle badge.
- **Icon:** Channel-specific SVG icon, 14–16px.
- **Background:** Channel brand color (see table below). **Only place these colors appear in Chatverce.**
- **Label (optional):** Channel name text beside the badge in list contexts.

## Variants

| Variant | Description |
|---------|-------------|
| Icon-only badge | Small circle, icon centered. Used in tight list rows where space is constrained. |
| Icon + label | Badge + channel name text. Used in detail views and channel selection UI. |

## Channel Color Reference

| Channel | Background Color | Hex | Icon |
|---------|-----------------|-----|------|
| WhatsApp | `cv-channel-whatsapp` | `#25D366` | WhatsApp logo SVG |
| Telegram | `cv-channel-telegram` | `#26A5E4` | Telegram logo SVG |
| Webchat | `cv-gray-500` | (gray) | `MessageCircle` Lucide |
| Phone | `cv-navy-700` | (navy) | `Phone` Lucide |
| Email | `cv-navy-700` | (navy) | `Mail` Lucide |

Icon color on all channel backgrounds: white (`cv-text-on-accent` or `#FFFFFF`).

## Chatverce Styling

- Badge size (icon-only): 20px × 20px, `cv-radius-full`
- Badge size (pill): 20px height, `cv-space-2` horizontal padding, `cv-radius-full`
- Icon: 12px (icon-only), 14px (pill with label)
- Label text: `cv-text-xs` (11–12px), white, weight 500
- **Channel colors are design-system constants** — never substitute with `cv-bg-accent` or other generic tokens

## Platform Considerations

Web + Mobile: The channel indicator is identical across platforms. On mobile, ensure the touch target wrapping the indicator is at least 44px if it is interactive. For display-only uses, 20px is acceptable.

## Accessibility

- Icon-only badge: must have `aria-label="WhatsApp"` (or the channel name) on the container.
- Icon + label: icon is `aria-hidden="true"`, the visible label text communicates the channel name.
- Do not convey channel type solely through color — the icon and/or label must also be present.

## Code Example

```tsx
const CHANNEL_CONFIG = {
  whatsapp: { bg: "#25D366", label: "WhatsApp", Icon: WhatsAppIcon },
  telegram: { bg: "#26A5E4", label: "Telegram", Icon: TelegramIcon },
  webchat:  { bg: "var(--cv-gray-500)", label: "Webchat", Icon: MessageCircle },
  phone:    { bg: "var(--cv-navy-700)", label: "Phone", Icon: Phone },
  email:    { bg: "var(--cv-navy-700)", label: "Email", Icon: Mail },
};

function ChannelIndicator({ channel, showLabel = false }) {
  const { bg, label, Icon } = CHANNEL_CONFIG[channel];
  return (
    <span
      aria-label={showLabel ? undefined : label}
      className="inline-flex items-center gap-1 rounded-full px-1.5"
      style={{ backgroundColor: bg }}
    >
      <Icon size={12} color="white" aria-hidden="true" />
      {showLabel && (
        <span className="text-xs font-medium text-white">{label}</span>
      )}
    </span>
  );
}

// Usage
<ChannelIndicator channel="whatsapp" />
<ChannelIndicator channel="telegram" showLabel />
```

## Anti-Patterns

- Never use channel brand colors anywhere other than the Channel Indicator — they are not general-purpose palette colors.
- Never show only the background color without an icon — color alone is insufficient for accessibility.
- Never use the same navy color for phone and email without an icon to differentiate — the icons are the distinguishing element.
