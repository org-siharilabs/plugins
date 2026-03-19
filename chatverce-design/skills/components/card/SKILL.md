---
name: card
description: Surface container for grouping related content with consistent elevation and spacing
user_invocable: false
---

# Card

**Gallery reference:** https://component.gallery/components/card/
**Aliases:** Panel, Tile, Surface, Container, Content card

## When to Use

Use Card to group related content that belongs together visually and semantically: an agent summary, a stat on the dashboard, a settings section, a pricing plan. Card creates visual separation between content blocks without requiring hard borders or background breaks. If content needs to feel "owned" as a unit — a distinct thing the user can scan, interact with, or act on — it belongs in a Card.

## When NOT to Use

Do not use Card as a page wrapper — the page root already sits on `cv-bg-primary`. Do not stack Card inside Card (nesting elevation looks accidental). Do not use Card for list items in a dense list — use a flat row pattern with Separator instead. Do not add colored borders to cards — elevation and background contrast carry the visual hierarchy.

## Anatomy

- **Container:** `cv-bg-surface` background, `cv-shadow-sm`, `cv-radius-md`, padding 16–24px
- **Header area (optional):** Title (H3), description (Body/caption), leading icon or avatar
- **Content area:** Main content — can be any composition of text, data, controls
- **Footer area (optional):** Actions (Button/ghost), metadata, status

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Default | White/surface bg, sm shadow | Standard content grouping |
| Interactive | Adds hover shadow + cursor pointer | Clickable cards — agent list items, navigation cards |
| Flat | No shadow, cv-border-default border | Use inside already-elevated surfaces (modals, drawers) |
| Featured | cv-bg-accent-subtle tint | Highlighted recommendation, active plan, pinned item |
| Stat | Compact, icon + label + value layout | Dashboard metric tiles |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | cv-shadow-sm, cv-bg-surface | Stable, contained, readable |
| Hover (web) | cv-shadow-md, slight translateY(-1px) | Responsive, interactive |
| Pressed/Active | cv-shadow-sm returns, translateY(0) | Tactile confirmation |
| Focus | 2px cv-border-accent focus ring (interactive variant) | Keyboard-navigable |
| Disabled | cv-text-tertiary content, no pointer interaction | Inactive section |
| Loading | Content replaced with Skeleton component | Content is fetching |

## Chatverce Styling

- Background: `cv-bg-surface`
- Shadow: `cv-shadow-sm` at rest, `cv-shadow-md` on hover (interactive variant)
- Border radius: `cv-radius-md`
- Border: none (default), `cv-border-default` (flat variant)
- Padding: 16px (compact), 24px (default), 32px (spacious/feature card)
- Transition: 150ms ease for box-shadow and transform (web)

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Card>`, `<CardHeader>`, `<CardContent>`, `<CardFooter>` primitives. Apply cv-token overrides via Tailwind CSS custom properties. Interactive cards use a wrapping `<button>` or `<a>` for accessibility — do not make a `<div>` clickable.

### iOS (Expo + NativeWind)
React Native does not have CSS box-shadow — use `elevation` (Android) and `shadowColor/shadowOffset/shadowOpacity/shadowRadius` (iOS). Keep shadows subtle: `shadowOpacity: 0.08`, `shadowRadius: 4`. No hover state on mobile.

### Android (Expo + NativeWind)
Use `elevation={2}` for default, `elevation={4}` for active/selected state. Android elevation casts a Material Design shadow automatically. Ensure card background is explicitly set to `cv-bg-surface` to match the light/dark theme.

## Accessibility

- Web: Use semantic HTML inside cards. If the card is clickable, the entire card must be a `<button>` or `<a>` — not a `<div onClick>`. Include `aria-label` if the card title is not sufficient context.
- Mobile: `accessibilityRole="button"` on interactive cards, `accessibilityLabel` that summarizes the card content.

## Code Examples

### Web

```tsx
import { Card, CardHeader, CardContent, CardFooter } from "@/components/ui/card";

// Default card
<Card className="bg-[--cv-bg-surface] shadow-[--cv-shadow-sm] rounded-[--cv-radius-md] p-6">
  <CardHeader className="pb-3">
    <h3 className="text-[--cv-text-primary] font-medium text-lg">Support Agent</h3>
    <p className="text-[--cv-text-secondary] text-sm">Handles inbound customer queries via WhatsApp</p>
  </CardHeader>
  <CardContent>
    <p className="text-[--cv-text-primary] text-sm">14 active conversations</p>
  </CardContent>
  <CardFooter className="pt-4 flex gap-2">
    <Button variant="ghost" className="text-[--cv-text-primary]">Edit</Button>
    <Button className="bg-[--cv-bg-accent] text-[--cv-text-on-accent]">View Live</Button>
  </CardFooter>
</Card>

// Interactive card (clickable)
<button
  className="w-full text-left bg-[--cv-bg-surface] shadow-[--cv-shadow-sm] rounded-[--cv-radius-md] p-6
             hover:shadow-[--cv-shadow-md] hover:-translate-y-px transition-all duration-150"
  onClick={handleAgentSelect}
  aria-label="Open Support Agent settings"
>
  {/* card content */}
</button>
```

### Mobile

```tsx
import { View, Text, Pressable } from "react-native";

// Default card
<View className="bg-[--cv-bg-surface] rounded-[--cv-radius-md] p-6"
  style={{ shadowColor: "#000", shadowOffset: { width: 0, height: 2 }, shadowOpacity: 0.08, shadowRadius: 4, elevation: 2 }}>
  <Text className="text-[--cv-text-primary] font-medium text-lg mb-1">Support Agent</Text>
  <Text className="text-[--cv-text-secondary] text-sm">Handles inbound queries via WhatsApp</Text>
</View>

// Interactive card
<Pressable
  className="bg-[--cv-bg-surface] rounded-[--cv-radius-md] p-6 active:opacity-80"
  style={{ elevation: 2 }}
  onPress={handleAgentSelect}
  accessibilityRole="button"
  accessibilityLabel="Open Support Agent settings"
>
  <Text className="text-[--cv-text-primary] font-medium text-lg">Support Agent</Text>
</Pressable>
```

## Anti-Patterns

- Never nest a Card inside another Card — elevation becomes ambiguous and the visual hierarchy collapses.
- Never use colored borders on cards — the product spec prohibits this. Elevation (shadow) communicates depth; borders are for inputs.
- Never make a `<div>` clickable without making it a `<button>` — this breaks keyboard and screen reader access.
- Never use Card for dense list rows where a Separator-divided flat list would be cleaner.
- Never add a background color other than `cv-bg-surface` or `cv-bg-accent-subtle` — a rainbow of card colors fragments the visual system.
