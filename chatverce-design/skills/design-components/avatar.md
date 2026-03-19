
# Avatar

**Gallery reference:** https://component.gallery/components/avatar/
**Aliases:** Profile picture, User image, Initials avatar, User icon

## When to Use

Use Avatar to represent a person (user, team member, contact) or an entity (agent, channel) visually throughout the product. Avatar appears in Navigation (user profile), conversation threads (sender identity), team settings, contact lists, and anywhere person-level context is useful.

## When NOT to Use

Do not use Avatar for abstract concepts that don't map to a person or named entity — use an Icon instead. Do not use Avatar at sizes smaller than 24px — initials become illegible. Do not use Avatar without a fallback (initials or icon) — broken image states are jarring.

## Anatomy

- **Container:** Circle, size-dependent diameter
- **Photo:** User photo, `object-cover`, fills container
- **Initials fallback:** 1–2 characters from name, centered, weight 500, on a background color from the avatar color palette
- **Online indicator (optional):** 10px green dot, bottom-right, `cv-status-success` fill, white border
- **Badge/count (optional):** Number badge on corner (notification count)

## Variants

| Variant | Size | Use Case |
|---------|------|----------|
| xs | 24px | Compact mentions, inline references |
| sm | 32px | Table rows, dense lists, message timestamps |
| md | 40px | Standard lists, card headers, conversation view |
| lg | 64px | Profile headers, contact detail, onboarding |
| xl | 96px | Profile page hero, large feature displays |

## Avatar Color Palette

When no photo is available, initials are displayed on a generated background color. The color is deterministic (based on the user's name or ID hash) so it never changes between sessions.

8 avatar colors (all pass WCAG AA against white initials):
1. Midnight Navy (`#1B2A4A`) — matches brand
2. Chatverce Teal (`#2EC4B6`) — matches brand
3. Slate Blue (`#4A6FA5`)
4. Warm Amber (`#E8A838`)
5. Sage Green (`#52B788`)
6. Muted Rose (`#C9606A`)
7. Dusty Purple (`#7B5EA7`)
8. Coral (`#E07057`)

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | Photo or initials | Identity, recognition |
| Online indicator | Green dot bottom-right | Active, available |
| Offline | No indicator | Not active |
| Image loading | Initials shown until photo loads | No flash, graceful |
| Broken image | Initials fallback | No broken image icons |
| Hover (when clickable) | Slight opacity change or ring | Clickable |
| Focus | 2px cv-border-accent ring | Keyboard-accessible |

## Chatverce Styling

- Shape: circle (100% border radius)
- Sizes: 24 / 32 / 40 / 64 / 96px
- Font size for initials: 40% of diameter
- Online dot: 10px, `cv-status-success`, 2px white border
- Clickable avatar: hover ring `cv-border-accent`

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Avatar>`, `<AvatarImage>`, `<AvatarFallback>`. The `<AvatarFallback>` renders initials when the image fails or is loading. Apply deterministic color via a `getAvatarColor(userId)` utility.

### iOS (Expo + NativeWind)
Use Expo's `<Image>` component from `expo-image` (which has built-in caching and fade-in). Wrap in a `View` with `borderRadius: size / 2`. Show initials `Text` absolutely centered when image is unavailable.

### Android (Expo + NativeWind)
Same as iOS. `expo-image` handles Android caching efficiently. Ensure `contentFit="cover"` is set to prevent image distortion.

## Accessibility

- Web: Avatar image must have `alt` text — the user's name or a contextual description. Decorative-only avatars in a list where the name is already visible should have `alt=""`. Online indicator uses `aria-label="Online"` on the dot or is conveyed through the surrounding text.
- Mobile: `accessibilityLabel` with the user's name. If the avatar is a button (opens profile), `accessibilityRole="button"` with `accessibilityLabel="View [name]'s profile"`.

## Code Examples

### Web

```tsx
import { Avatar, AvatarImage, AvatarFallback } from "@/components/ui/avatar";

const AVATAR_COLORS = [
  "#1B2A4A", "#2EC4B6", "#4A6FA5", "#E8A838",
  "#52B788", "#C9606A", "#7B5EA7", "#E07057",
];

function getAvatarColor(identifier: string): string {
  let hash = 0;
  for (const char of identifier) hash = char.charCodeAt(0) + ((hash << 5) - hash);
  return AVATAR_COLORS[Math.abs(hash) % AVATAR_COLORS.length];
}

function getInitials(name: string): string {
  return name.split(" ").slice(0, 2).map(n => n[0]).join("").toUpperCase();
}

function CvAvatar({ user, size = "md", showOnline = false }) {
  const sizeMap = { xs: "h-6 w-6 text-[8px]", sm: "h-8 w-8 text-[10px]", md: "h-10 w-10 text-xs", lg: "h-16 w-16 text-lg", xl: "h-24 w-24 text-2xl" };
  const dotMap = { xs: "h-2 w-2", sm: "h-2.5 w-2.5", md: "h-3 w-3", lg: "h-4 w-4", xl: "h-5 w-5" };

  return (
    <div className="relative inline-block">
      <Avatar className={sizeMap[size]}>
        <AvatarImage src={user.photoUrl} alt={user.name} />
        <AvatarFallback
          style={{ backgroundColor: getAvatarColor(user.id) }}
          className="text-white font-medium"
        >
          {getInitials(user.name)}
        </AvatarFallback>
      </Avatar>
      {showOnline && (
        <span
          className={`absolute bottom-0 right-0 ${dotMap[size]} rounded-full bg-[--cv-status-success] ring-2 ring-white`}
          aria-label="Online"
        />
      )}
    </div>
  );
}
```

### Mobile

```tsx
import { View, Text, Pressable } from "react-native";
import { Image } from "expo-image";
import { useState } from "react";

const AVATAR_COLORS = ["#1B2A4A", "#2EC4B6", "#4A6FA5", "#E8A838", "#52B788", "#C9606A", "#7B5EA7", "#E07057"];

function getAvatarColor(id) {
  let hash = 0;
  for (const c of id) hash = c.charCodeAt(0) + ((hash << 5) - hash);
  return AVATAR_COLORS[Math.abs(hash) % AVATAR_COLORS.length];
}

function getInitials(name) {
  return name.split(" ").slice(0, 2).map(n => n[0]).join("").toUpperCase();
}

const SIZE_MAP = { xs: 24, sm: 32, md: 40, lg: 64, xl: 96 };
const FONT_MAP = { xs: 8, sm: 11, md: 14, lg: 22, xl: 32 };
const DOT_MAP = { xs: 7, sm: 9, md: 11, lg: 14, xl: 18 };

function CvAvatar({ user, size = "md", showOnline = false, onPress }) {
  const [imageError, setImageError] = useState(false);
  const d = SIZE_MAP[size];
  const Wrapper = onPress ? Pressable : View;

  return (
    <Wrapper
      style={{ width: d, height: d, position: "relative" }}
      onPress={onPress}
      accessibilityRole={onPress ? "button" : undefined}
      accessibilityLabel={onPress ? `View ${user.name}'s profile` : user.name}
    >
      {user.photoUrl && !imageError ? (
        <Image
          source={{ uri: user.photoUrl }}
          style={{ width: d, height: d, borderRadius: d / 2 }}
          contentFit="cover"
          onError={() => setImageError(true)}
          accessibilityLabel={user.name}
        />
      ) : (
        <View style={{ width: d, height: d, borderRadius: d / 2, backgroundColor: getAvatarColor(user.id), alignItems: "center", justifyContent: "center" }}>
          <Text style={{ color: "white", fontSize: FONT_MAP[size], fontWeight: "500" }}>
            {getInitials(user.name)}
          </Text>
        </View>
      )}
      {showOnline && (
        <View style={{
          position: "absolute", bottom: 0, right: 0,
          width: DOT_MAP[size], height: DOT_MAP[size],
          borderRadius: DOT_MAP[size] / 2,
          backgroundColor: "var(--cv-status-success)",
          borderWidth: 2, borderColor: "white",
        }} accessibilityElementsHidden={true} />
      )}
    </Wrapper>
  );
}
```

## Anti-Patterns

- Never show a broken image icon — always fall back to initials.
- Never use a generic person icon as the only fallback — initials with a color background are more personal and recognizable.
- Never use avatar at sizes below 24px — initials become unreadable.
- Never use photo-only avatars without a fallback — network can fail.
- Never skip `alt` text on web avatar images — screen readers need the identity information.
