
# Materials & Depth Foundation

Depth creates hierarchy. A well-layered interface tells the user what is in front of what, what is temporary, and what is permanent — without labels or explanations. Chatverce uses a strict five-level elevation system to make these relationships consistent across web and mobile.

## Elevation Levels

### Level 0 — Base

**Purpose:** The page background. Everything else sits on top of this.

- **Background token:** `cv-bg-primary`
- **Shadow:** None
- **Use cases:** Application background, full-bleed backgrounds behind content

### Level 1 — Surface

**Purpose:** Cards, panels, and contained content blocks that sit on the page background.

- **Background token:** `cv-bg-surface`
- **Shadow token:** `cv-shadow-sm`
- **Use cases:** Channel cards, metric panels, conversation list rows, settings sections

**Platform notes:**
- **Web:** `cv-shadow-sm` box-shadow applied.
- **iOS:** Subtle shadow acceptable; prefer a slight background-color contrast over shadow.
- **Android:** No shadow. Use background color contrast (`cv-bg-surface` vs `cv-bg-primary`) to establish separation. Android elevation via the `elevation` prop is reserved for interactive elements.

### Level 2 — Elevated

**Purpose:** Temporary overlays that appear above primary content but are not blocking — dropdowns, popovers, select menus, tooltips with rich content.

- **Background token:** `cv-bg-surface-elevated`
- **Shadow token:** `cv-shadow-md`
- **Use cases:** Dropdown menus, command palette, date pickers, inline popovers

**Platform notes:**
- **Web:** `cv-shadow-md` box-shadow. `backdrop-filter` may be used sparingly.
- **Mobile:** Platform-native shadow styling. On iOS use `shadowColor`, `shadowOffset`, `shadowOpacity`, `shadowRadius`. On Android use the `elevation` prop.

### Level 3 — Overlay

**Purpose:** Blocking overlays that require user attention or action before returning to the primary content. Accompanied by a backdrop.

- **Background token:** `cv-bg-surface-elevated`
- **Shadow token:** `cv-shadow-lg`
- **Backdrop:** Semi-transparent overlay behind the surface (`bg-black/50` or equivalent token)
- **Use cases:** Modals, confirmation dialogs, side drawers, bottom sheets

**Platform notes:**
- **Web:** Centered modal or side drawer with backdrop. `cv-shadow-lg` box-shadow.
- **iOS:** Full-screen modal or bottom sheet (use `react-native-bottom-sheet` or equivalent). Shadow and blur backdrop optional.
- **Android:** Full-screen modal preferred. Bottom sheets for actions. Avoid center modals on small screens.

### Level 4 — Toast

**Purpose:** Ephemeral system messages that appear above all other content including overlays. Non-blocking — the user does not need to dismiss them to continue.

- **Background token:** `cv-bg-surface-elevated`
- **Shadow token:** `cv-shadow-lg`
- **Use cases:** Toast notifications, floating tooltips, inline error banners that float

**Platform notes:**
- **Web:** Fixed-position, z-index above all other layers.
- **Mobile:** System overlay layer. On iOS/Android, toasts render above the navigation stack. Use a toast library that handles safe area insets correctly.

## Shadow System

Shadow tokens provide consistent depth cues. Each level maps directly to a token.

| Token | Value (Light Mode) | Use |
|---|---|---|
| `cv-shadow-sm` | `0 1px 3px rgba(0,0,0,0.08), 0 1px 2px rgba(0,0,0,0.04)` | Level 1 surfaces |
| `cv-shadow-md` | `0 4px 6px rgba(0,0,0,0.07), 0 2px 4px rgba(0,0,0,0.05)` | Level 2 elevated elements |
| `cv-shadow-lg` | `0 10px 15px rgba(0,0,0,0.10), 0 4px 6px rgba(0,0,0,0.05)` | Level 3 overlays, Level 4 toasts |

**Dark mode:** Use the same shadow color values. Shadows are often invisible on dark backgrounds — compensate by using a slightly higher opacity or relying on background color contrast rather than shadow. Do not invert shadow color to white/light; it looks unnatural. Test dark mode shadows on actual dark backgrounds, not in design tools.

## Translucency

Translucent materials (frosted glass, blur backdrops) are permitted in specific, narrow contexts. They are not a general design tool.

**Permitted uses:**
- Navigation bars (web: sticky header with backdrop blur)
- Tab bars (mobile: iOS tab bar with system blur)
- Floating toolbars that need to feel "above" content without fully obscuring it

**Never use translucency for:**
- Content surfaces (cards, panels, list rows) — the text becomes unreadable
- Modals or dialogs — use opaque backgrounds
- Any surface where the content behind affects readability

### Web

```css
/* Translucent nav bar */
backdrop-filter: blur(12px);
background-color: rgba(var(--cv-bg-primary-rgb), 0.8);
```

Use Tailwind's `backdrop-blur-md` + `bg-cv-bg-primary/80`.

### Mobile

Use `expo-blur` (`BlurView`) from the Expo SDK:

```typescript
import { BlurView } from 'expo-blur';

<BlurView intensity={80} tint="light" style={styles.tabBar}>
  {children}
</BlurView>
```

Do not attempt to replicate blur with CSS-like tricks on mobile — use `BlurView` or the platform's native blur. Fake blur (semi-transparent flat color) is acceptable on Android where `BlurView` performance may be problematic; test on real devices.

## Platform Considerations

Depth rendering differs significantly between platforms. Never assume a web design will translate directly to mobile.

### Web

- **Shadows:** CSS `box-shadow` using the `cv-shadow-*` tokens.
- **Translucency:** `backdrop-filter: blur()` supported in all modern browsers. Include `@supports` fallback for older environments if needed.
- **Z-index management:** Maintain a documented z-index scale to prevent stacking context conflicts. Suggested scale: base(0), surface(10), elevated(20), overlay(30), toast(40).

### iOS

- **Depth via layering and blur:** iOS interfaces read depth through color contrast and blur, not strong shadows. Prefer `cv-bg-surface` vs `cv-bg-primary` contrast over heavy `box-shadow` equivalents.
- **BlurView:** Use for nav bars, tab bars, and contextual menus where platform conventions call for blur.
- **Shadows:** Apply with `shadowColor`, `shadowOffset`, `shadowOpacity`, `shadowRadius` props. Keep shadows subtle — iOS system UI uses very soft shadows.
- **Bottom sheets:** Preferred pattern for Level 3 overlays on iPhone. Use well-maintained libraries (e.g., `@gorhom/bottom-sheet`) rather than building custom.

### Android

- **Elevation prop:** React Native's `elevation` prop maps to Android's Material elevation system. Use it for interactive Level 2+ surfaces.
- **No box-shadow:** The `shadow*` props are iOS-only. Do not apply them to Android-specific components.
- **Test on real devices:** Android shadow rendering varies significantly across manufacturers and Android versions. Emulators are insufficient — test on physical hardware.
- **Background contrast first:** On Android, rely on background color contrast (Level 0 vs Level 1 tokens) as the primary depth signal before reaching for elevation.
