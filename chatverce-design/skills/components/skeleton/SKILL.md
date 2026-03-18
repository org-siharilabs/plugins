---
name: skeleton
description: Animated placeholder that mirrors content layout during loading to prevent layout shift and blank states
user_invocable: false
---

# Skeleton

**Gallery reference:** https://component.gallery/components/skeleton/
**Aliases:** Loading placeholder, Shimmer, Ghost, Content loader, Skeleton loader

## When to Use

Use Skeleton whenever content is being fetched and you know the approximate shape of what will appear: table rows loading, card grids loading, a profile page waiting for user data, a conversation thread loading messages. Skeleton prevents the blank-screen flash and sets visual expectations before content arrives.

## When NOT to Use

Do not use Skeleton for very short loads (<300ms) — the skeleton flash itself is more jarring than waiting. Do not use Skeleton when the number/layout of items is unknown — use Spinner instead. Do not use Skeleton for background operations the user isn't waiting for. Do not use a generic rectangle skeleton when you know the exact content shape — specific shapes are significantly less jarring.

## Anatomy

- **Base shape:** `cv-bg-surface-sunken` tinted rectangle, circle, or custom shape
- **Shimmer animation:** Left-to-right gradient sweep, ~1.5s loop
- **Shape varieties:** Text lines (varying widths), circles (avatars), rectangles (images, cards), rounded rectangles (buttons, badges)
- **Container:** Mirrors the layout of the real content it represents

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Text line | Rounded rectangle, varying width | Paragraph text, labels, descriptions |
| Avatar | Circle | Profile pictures, user avatars |
| Image | Rectangle, aspect ratio matched | Image cards, thumbnails |
| Card | Full card shape with text lines inside | Card grids, agent list |
| Table row | Header + multiple data columns | Table loading state |
| Button | Rounded rectangle, button-sized | Action areas loading |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Loading (default) | Shimmer animation | Content is coming, system is working |
| Content arrived | Skeleton removed, real content fades in | Smooth, non-jarring reveal |

## Chatverce Styling

- Base color: `cv-bg-surface-sunken`
- Shimmer highlight: slightly lighter gradient (10–15% brighter)
- Shimmer direction: left-to-right
- Shimmer duration: 1.5s loop, ease-in-out
- Border radius: match the content it represents (text=4px, avatar=50%, card=cv-radius-md)
- `aria-busy="true"` on skeleton containers, `aria-label="Loading content"`

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Skeleton>` with CSS `animate-pulse` (Tailwind). For shimmer (moving gradient) instead of pulse, add a custom keyframe. Compose multiple `<Skeleton>` elements to match the content layout exactly.

### iOS (Expo + NativeWind)
Use `Animated.loop` with `Animated.sequence` for a pulse effect, or use the `react-native-skeleton-placeholder` package for shimmer. Alternatively, NativeWind's `animate-pulse` utility works if configured. Match shape and size to the real component.

### Android (Expo + NativeWind)
Same as iOS. Shimmer performance on Android is good via `Animated.timing` with `useNativeDriver: true`.

## Accessibility

- Web: Set `aria-busy="true"` on the container that holds skeleton content. Set `aria-label="Loading"` or an appropriate context label. Screen readers will announce the loading state. When content arrives, `aria-busy` becomes `false` and the real content is announced.
- Mobile: `accessibilityElementsHidden={true}` on skeleton shapes — they carry no meaningful information. The parent container should have `accessibilityLabel="Loading"` and `accessibilityLiveRegion="polite"` so screen readers announce when content loads.

## Code Examples

### Web

```tsx
import { Skeleton } from "@/components/ui/skeleton";

// Agent card skeleton
function AgentCardSkeleton() {
  return (
    <div className="bg-[--cv-bg-surface] rounded-[--cv-radius-md] shadow-[--cv-shadow-sm] p-6" aria-busy="true" aria-label="Loading agent">
      <div className="flex items-center gap-3 mb-4">
        <Skeleton className="h-10 w-10 rounded-full bg-[--cv-bg-surface-sunken]" />
        <div className="space-y-2">
          <Skeleton className="h-4 w-32 bg-[--cv-bg-surface-sunken] rounded" />
          <Skeleton className="h-3 w-20 bg-[--cv-bg-surface-sunken] rounded" />
        </div>
      </div>
      <Skeleton className="h-3 w-full bg-[--cv-bg-surface-sunken] rounded mb-2" />
      <Skeleton className="h-3 w-3/4 bg-[--cv-bg-surface-sunken] rounded" />
    </div>
  );
}

// Table row skeletons
function TableSkeleton({ rows = 5 }) {
  return (
    <div className="bg-[--cv-bg-surface] rounded-[--cv-radius-md] shadow-[--cv-shadow-sm]" aria-busy="true" aria-label="Loading data">
      {/* Header */}
      <div className="flex px-4 py-3 border-b border-[--cv-border-default] bg-[--cv-bg-surface-sunken] gap-4">
        <Skeleton className="h-3 w-24 bg-[--cv-bg-surface-sunken]" />
        <Skeleton className="h-3 w-16 bg-[--cv-bg-surface-sunken]" />
        <Skeleton className="h-3 w-20 bg-[--cv-bg-surface-sunken]" />
      </div>
      {Array.from({ length: rows }).map((_, i) => (
        <div key={i} className="flex items-center px-4 py-3 border-b border-[--cv-border-default] gap-4">
          <Skeleton className="h-4 w-32 bg-[--cv-bg-surface-sunken] rounded" />
          <Skeleton className="h-5 w-16 bg-[--cv-bg-surface-sunken] rounded-full" />
          <Skeleton className="h-4 w-12 bg-[--cv-bg-surface-sunken] rounded" />
        </div>
      ))}
    </div>
  );
}
```

### Mobile

```tsx
import { View } from "react-native";
import SkeletonPlaceholder from "react-native-skeleton-placeholder";

function AgentCardSkeletonMobile() {
  return (
    <SkeletonPlaceholder backgroundColor="var(--cv-bg-surface-sunken)" highlightColor="rgba(255,255,255,0.1)">
      <SkeletonPlaceholder.Item flexDirection="row" alignItems="center" gap={12} marginBottom={16}>
        <SkeletonPlaceholder.Item width={40} height={40} borderRadius={20} />
        <SkeletonPlaceholder.Item>
          <SkeletonPlaceholder.Item width={120} height={14} borderRadius={4} marginBottom={6} />
          <SkeletonPlaceholder.Item width={80} height={11} borderRadius={4} />
        </SkeletonPlaceholder.Item>
      </SkeletonPlaceholder.Item>
      <SkeletonPlaceholder.Item width="100%" height={12} borderRadius={4} marginBottom={8} />
      <SkeletonPlaceholder.Item width="75%" height={12} borderRadius={4} />
    </SkeletonPlaceholder>
  );
}

// Simple pulse approach without library
function SimpleSkeleton({ width, height, borderRadius = 4, style }) {
  const opacity = useRef(new Animated.Value(0.5)).current;
  useEffect(() => {
    Animated.loop(
      Animated.sequence([
        Animated.timing(opacity, { toValue: 1, duration: 750, useNativeDriver: true }),
        Animated.timing(opacity, { toValue: 0.5, duration: 750, useNativeDriver: true }),
      ])
    ).start();
  }, []);

  return (
    <Animated.View
      style={[{ width, height, borderRadius, backgroundColor: "var(--cv-bg-surface-sunken)", opacity }, style]}
      accessibilityElementsHidden={true}
    />
  );
}
```

## Anti-Patterns

- Never use a single large rectangle for everything — match the skeleton to the actual content shapes.
- Never keep skeleton visible after content arrives — always replace it with real content.
- Never use skeleton for content that loads in under 300ms — the flash of skeleton is worse than briefly waiting.
- Never make skeleton shapes larger than the real content — this causes jarring layout shift on content arrival.
- Never omit `aria-busy` — screen readers need to know content is loading.
