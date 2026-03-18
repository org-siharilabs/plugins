---
name: badge
description: Compact visual label for status, count, category, or channel affiliation
user_invocable: false
---

# Badge

**Gallery reference:** https://component.gallery/components/badge/
**Aliases:** Chip, Tag, Label, Lozenge, Pill, Status badge, Count badge

## When to Use

Use Badge to convey a discrete piece of metadata that belongs to an item without taking layout space: the status of an agent (Live, Draft, Paused), the channel of a conversation (WhatsApp, SMS, Email), a notification count on a navigation item, or a category tag. Badge is purely informational — it triggers no action on its own.

## When NOT to Use

Do not use Badge as a button or interactive element — use Button. Do not use more than 2–3 badges on a single item — information overload. Do not use Badge for longer text (more than 3–4 words) — use a Label component. Do not use color alone to convey meaning without supplementary text or icon.

## Anatomy

- **Container:** Inline, `cv-radius-sm` or `cv-radius-full` (pill), 4–6px vertical padding, 8–10px horizontal padding
- **Text:** Caption size (11–12px), weight 500 (Medium)
- **Leading icon (optional):** 10–12px, same color as text
- **Count number:** Right-aligned inside container, or standalone pill with just a number

## Variants

| Variant | Description | Token / Color |
|---------|-------------|--------------|
| Success | Positive status (Agent Live, task complete) | `cv-status-success` bg/text tint |
| Warning | Caution status (Agent paused, degraded) | `cv-status-warning` bg/text tint |
| Error | Failure status (Error, offline, blocked) | `cv-status-error` bg/text tint |
| Info | Neutral informational | `cv-status-info` bg/text tint |
| Neutral | Category tags, no semantic color | `cv-bg-surface-sunken`, `cv-text-secondary` |
| WhatsApp channel | WhatsApp green | `#25D366` bg / white text |
| SMS channel | Slate blue | `#4A6FA5` bg / white text |
| Email channel | Amber | `#E8A838` bg / dark text |
| Count | Notification count | Red dot (`cv-status-error`) |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | Static, colored | Informational, scannable |
| Hover (if dismissible) | X icon appears | Removable tag |
| Count at limit | "99+" text | Overflow indicator |

## Chatverce Styling

Badge backgrounds use a 10–15% opacity tint of the status color, with the full status color for text. This prevents oversaturation when multiple badges appear together.

- Success: `rgba(cv-status-success, 0.12)` bg, `cv-text-success` text
- Warning: `rgba(cv-status-warning, 0.12)` bg, `cv-text-warning` text
- Error: `rgba(cv-status-error, 0.12)` bg, `cv-text-error` text
- Info: `cv-bg-accent-subtle` bg, `cv-text-accent` text
- Neutral: `cv-bg-surface-sunken` bg, `cv-text-secondary` text
- Channel badges: solid fill, white text (or dark if amber)
- Border radius: `cv-radius-full` (pill) or `cv-radius-sm` (rectangular)
- Count badge max: "99+" — never show raw numbers above 99

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Badge>` as the base. Override with variant classes. For count badges on nav items, position absolutely over the icon using `absolute -top-1 -right-1`.

### iOS (Expo + NativeWind)
Build a custom `View` with `borderRadius: 20` (pill). React Native has no native badge component. For count badges on tab bar icons, use Expo Router's `tabBarBadge` prop.

### Android (Expo + NativeWind)
Same as iOS. For tab bar counts, Expo Router's `tabBarBadge` renders a Material-style badge automatically.

## Accessibility

- Web: Badge text must be readable — do not rely on color alone. Use `aria-label` on count badges to provide the full context ("12 unread notifications"). If a badge is purely decorative, `aria-hidden="true"`.
- Mobile: `accessibilityLabel` describing the badge meaning ("Agent is live", "12 unread"). `accessibilityElementsHidden={true}` for purely decorative status dots.

## Code Examples

### Web

```tsx
import { Badge } from "@/components/ui/badge";

// Status badges
function AgentStatusBadge({ status }) {
  const config = {
    live: { label: "Live", className: "bg-green-50 text-[--cv-text-success] border-0" },
    draft: { label: "Draft", className: "bg-[--cv-bg-surface-sunken] text-[--cv-text-secondary] border-0" },
    paused: { label: "Paused", className: "bg-amber-50 text-[--cv-text-warning] border-0" },
    error: { label: "Error", className: "bg-red-50 text-[--cv-text-error] border-0" },
  }[status] || { label: status, className: "bg-[--cv-bg-surface-sunken] text-[--cv-text-secondary]" };

  return (
    <Badge className={`text-xs font-medium px-2 py-0.5 rounded-full ${config.className}`}>
      {config.label}
    </Badge>
  );
}

// Channel badge
function ChannelBadge({ channel }) {
  const config = {
    whatsapp: { label: "WhatsApp", bg: "#25D366", text: "white" },
    sms: { label: "SMS", bg: "#4A6FA5", text: "white" },
    email: { label: "Email", bg: "#E8A838", text: "#1B2A4A" },
    webchat: { label: "Web Chat", bg: "#2EC4B6", text: "white" },
  }[channel];

  if (!config) return null;
  return (
    <span
      className="inline-flex items-center px-2 py-0.5 rounded-full text-xs font-medium"
      style={{ backgroundColor: config.bg, color: config.text }}
    >
      {config.label}
    </span>
  );
}

// Count badge on nav icon
function NavItemWithBadge({ href, icon: Icon, label, count }) {
  return (
    <Link href={href} className="relative flex items-center gap-3 px-3 py-2.5 rounded-[--cv-radius-md]">
      <span className="relative">
        <Icon size={20} />
        {count > 0 && (
          <span
            className="absolute -top-1.5 -right-1.5 bg-[--cv-status-error] text-white text-[9px] font-medium rounded-full min-w-[16px] h-4 flex items-center justify-center px-1"
            aria-label={`${count > 99 ? "99+" : count} unread`}
          >
            {count > 99 ? "99+" : count}
          </span>
        )}
      </span>
      {label}
    </Link>
  );
}
```

### Mobile

```tsx
import { View, Text } from "react-native";

function CvBadge({ label, variant = "neutral" }) {
  const config = {
    success: { bg: "rgba(82, 183, 136, 0.12)", color: "var(--cv-text-success)" },
    warning: { bg: "rgba(232, 168, 56, 0.12)", color: "var(--cv-text-warning)" },
    error: { bg: "rgba(192, 96, 106, 0.12)", color: "var(--cv-text-error)" },
    info: { bg: "var(--cv-bg-accent-subtle)", color: "var(--cv-text-accent)" },
    neutral: { bg: "var(--cv-bg-surface-sunken)", color: "var(--cv-text-secondary)" },
  }[variant];

  return (
    <View style={{ backgroundColor: config.bg, borderRadius: 20, paddingHorizontal: 8, paddingVertical: 3, alignSelf: "flex-start" }}>
      <Text style={{ color: config.color, fontSize: 11, fontWeight: "500" }} accessibilityLabel={label}>
        {label}
      </Text>
    </View>
  );
}

function ChannelBadgeMobile({ channel }) {
  const config = {
    whatsapp: { label: "WhatsApp", bg: "#25D366", color: "white" },
    sms: { label: "SMS", bg: "#4A6FA5", color: "white" },
    email: { label: "Email", bg: "#E8A838", color: "#1B2A4A" },
  }[channel];
  if (!config) return null;
  return (
    <View style={{ backgroundColor: config.bg, borderRadius: 20, paddingHorizontal: 8, paddingVertical: 3, alignSelf: "flex-start" }}>
      <Text style={{ color: config.color, fontSize: 11, fontWeight: "500" }}>{config.label}</Text>
    </View>
  );
}
```

## Anti-Patterns

- Never use color as the only differentiator — always include a text label.
- Never show raw numbers above 99 in a count badge — truncate to "99+".
- Never use more than 3 badges on a single item — it overwhelms the scan.
- Never make a badge clickable without clear affordance — if it should be interactive, it is a Button or a Chip with an explicit delete icon.
- Never use WhatsApp green for anything other than the WhatsApp channel badge — it is a reserved semantic color in the Chatverce system.
