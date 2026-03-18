---
name: layout
description: Breakpoints, z-index scale, grid system, spacing, safe areas, and breathing room principles for web and mobile
user_invocable: false
---

## Breakpoints

Chatverce uses the standard Tailwind CSS breakpoint set. All breakpoints are `min-width` (mobile-first).

| Token | Value | Usage |
|-------|-------|-------|
| `sm` | 640px | Large phones in landscape, small tablets — begin multi-column where it helps |
| `md` | 768px | Tablets in portrait — sidebar navigation may appear, panels expand |
| `lg` | 1024px | Tablets in landscape, small laptops — full desktop layout begins here |
| `xl` | 1280px | Standard desktop — primary design target for wide layouts |
| `2xl` | 1536px | Large monitors — cap content width, increase breathing room |

**Mobile-first default**: styles without a breakpoint prefix apply at all screen sizes starting from mobile. Add `sm:`, `md:`, `lg:` etc. only to progressively enhance.

**Content width cap**: On `2xl` screens, cap the primary content area with `max-w-7xl mx-auto` (1280px) or `max-w-screen-xl mx-auto`. Do not allow content to stretch to 1536px edge-to-edge — it creates excessively long line lengths and breaks visual hierarchy.

**Line length**: Body text containers should be capped at `max-w-prose` (65ch) on wide screens. Lines longer than ~75 characters degrade reading speed and comprehension.

---

## Z-Index Scale

All z-index values in Chatverce use named custom tokens prefixed `cv-z-`. Never use arbitrary z-index integers in application code — they create ordering conflicts that are impossible to debug.

| Token | Value | Usage |
|-------|-------|-------|
| `cv-z-base` | 0 | Normal document flow |
| `cv-z-raised` | 10 | Slightly elevated content (sticky table headers, cards in hover state) |
| `cv-z-dropdown` | 100 | Dropdowns, select menus, autocomplete popups |
| `cv-z-sticky` | 200 | Sticky navigation bars, sticky section headers |
| `cv-z-overlay` | 300 | Drawer backdrops, non-modal overlay panels |
| `cv-z-modal` | 400 | Modal dialogs and their backdrops |
| `cv-z-notification` | 500 | Toast notifications, snack bars — must appear above modals |
| `cv-z-command` | 600 | Command palette, global search overlay |
| `cv-z-tooltip` | 700 | Tooltips — must be topmost to remain readable in any context |

**Rules:**

1. Layering must be intentional. Before assigning a z-index above `cv-z-raised`, ask whether the element genuinely needs to float above everything else or whether the DOM order can be reorganized instead.
2. Portals (modals, toasts, tooltips) must be rendered into the document root via a React portal — do not rely on z-index stacking within a deeply nested DOM tree.
3. Never use `z-index: 9999` or similar magic numbers. If an element needs to appear above `cv-z-tooltip`, that is a design signal that the z-index scale needs a new named level — not that the component needs a bigger number.

---

## Spacing Scale

All spacing in Chatverce derives from a **4px base unit**. Every margin, padding, gap, and size value must be a multiple of 4.

| Scale Step | Value | Tailwind Class | Usage |
|-----------|-------|---------------|-------|
| 1 | 4px | `p-1` / `m-1` / `gap-1` | Micro gaps — icon-to-label, badge internal padding |
| 2 | 8px | `p-2` / `m-2` / `gap-2` | Tight component internal spacing, input padding |
| 3 | 12px | `p-3` / `m-3` / `gap-3` | Small component padding, list item vertical rhythm |
| 4 | 16px | `p-4` / `m-4` / `gap-4` | Standard component padding, grid gap minimum |
| 5 | 20px | `p-5` / `m-5` / `gap-5` | Medium padding, sidebar item spacing |
| 6 | 24px | `p-6` / `m-6` / `gap-6` | Section internal padding minimum, card padding |
| 8 | 32px | `p-8` / `m-8` / `gap-8` | Section gaps, large card padding |
| 10 | 40px | `p-10` / `m-10` / `gap-10` | Section vertical separation |
| 12 | 48px | `p-12` / `m-12` / `gap-12` | Large section spacing, hero padding |
| 16 | 64px | `p-16` / `m-16` / `gap-16` | Major vertical section breaks |
| 20 | 80px | `p-20` / `m-20` / `gap-20` | Hero sections, large screen vertical rhythm |
| 24 | 96px | `p-24` / `m-24` / `gap-24` | Maximum vertical spacing, landing page sections |

**Named spacing tokens** (semantic aliases used in guidelines):

| Token | Value | Tailwind |
|-------|-------|---------|
| `cv-space-1` | 4px | `*-1` |
| `cv-space-2` | 8px | `*-2` |
| `cv-space-3` | 12px | `*-3` |
| `cv-space-4` | 16px | `*-4` |
| `cv-space-5` | 20px | `*-5` |
| `cv-space-6` | 24px | `*-6` |
| `cv-space-8` | 32px | `*-8` |

---

## Breathing Room

Breathing room is the principle that content needs space to exist — not just to fit. Cramped layouts signal low quality and increase cognitive load.

**Minimum section padding:** Every section or major content region must have at minimum `cv-space-6` (24px) of internal padding on all sides. On mobile this may reduce to `cv-space-4` (16px) to preserve screen real estate, but never below that.

**Minimum card gap:** When cards or panels sit in a grid or list, the gap between them must be at minimum `cv-space-4` (16px). Cards touching each other with no gap merge visually into a single undifferentiated block.

**Content never touches edges:** No text, icon, or interactive element should be placed at the raw screen edge (0px from the viewport boundary). On mobile, maintain at minimum `cv-space-4` (16px) horizontal padding. On desktop, use the layout container's own padding.

**Vertical rhythm:** Use consistent vertical spacing increments between content blocks. Do not mix arbitrary values like 13px, 17px, or 23px — they break the rhythm and are visually detectable as inconsistency. Stick to the 4px scale.

**Don't fill space for its own sake:** Breathing room is about balance, not emptiness. A card with too much padding feels unfinished. Calibrate against the content: a dense data table needs tighter cell padding; an onboarding card needs generous padding.

---

## Grid

Chatverce does not use a rigid column grid system (no Bootstrap 12-column layout, no enforced column counts).

**Rationale:** Rigid column grids impose layout constraints that fight against content-first design. Chatverce's product surfaces — agent dashboards, conversation panels, configuration forms — have varied information densities and interaction patterns. A fixed 12-column grid would be applied inconsistently and create more visual debt than it resolves.

**Instead, use:**

**Web:** CSS Grid and Flexbox, composed according to the content's natural structure.
- Use CSS Grid for two-dimensional layouts (rows and columns simultaneously, e.g., a card grid or a dashboard layout).
- Use Flexbox for one-dimensional layouts (a row of buttons, a vertical list, a navigation bar).
- Combine both: a page layout may use CSS Grid for the top-level column split (sidebar + main), with Flexbox inside each column for vertical stacking.

**Mobile:** Flexbox is the primary layout primitive in React Native. CSS Grid is not available.
- `flexDirection: 'column'` for vertical stacks (default).
- `flexDirection: 'row'` for horizontal arrangements.
- `flexWrap: 'wrap'` for responsive wrapping when available screen width varies.

**Content-first layout decisions:**
1. Start with the content — what information must be presented, in what order?
2. Choose the layout primitive that naturally represents that structure.
3. Apply spacing from the spacing scale.
4. Apply breakpoint variants to adapt for larger screens.

Do not start with a grid and fill it with content. Start with content and give it structure.

---

## Safe Areas (Mobile)

Mobile devices have physical features that intrude on the usable screen area. Layouts must account for these without hiding UI or placing interactive elements under device chrome.

### iOS

- **Notch / Dynamic Island:** Top of the screen. The status bar height varies by device. Never position fixed top navigation at 0 — it will be partially obscured on notched devices.
- **Home indicator:** Bottom of the screen on Face ID devices (no physical home button). The home indicator bar occupies approximately 34px at the bottom. Interactive elements placed at the raw screen bottom are difficult to reach and may conflict with system gesture navigation.
- **Rounded corners:** The screen corners are physically rounded. Content placed in the very corner will be clipped.

### Android

- **Navigation bar:** Android devices may use gesture navigation (which overlaps content) or three-button navigation (which takes up fixed space). The navigation bar height varies significantly across manufacturers.
- **Status bar:** Top of the screen. Height varies by device and may overlap content if not accounted for.

### Implementation

**Use `SafeAreaView`** from `react-native-safe-area-context` as the root wrapper for every screen. This automatically insets content away from the notch, home indicator, and navigation bar:

```tsx
import { SafeAreaView } from 'react-native-safe-area-context';

export function MyScreen() {
  return (
    <SafeAreaView style={{ flex: 1 }} edges={['top', 'bottom']}>
      {/* content */}
    </SafeAreaView>
  );
}
```

**Use `useSafeAreaInsets()`** when you need the raw inset values for custom positioning (e.g., a floating action button that must sit above the home indicator):

```tsx
import { useSafeAreaInsets } from 'react-native-safe-area-context';

export function FloatingButton() {
  const insets = useSafeAreaInsets();
  return (
    <Pressable style={{ marginBottom: insets.bottom + 16 }}>
      {/* button content */}
    </Pressable>
  );
}
```

**Testing requirements:**
- Test on a device or simulator **with** a notch/Dynamic Island.
- Test on a device or simulator **without** a notch (older iPhone SE, older Android devices).
- Test on an Android device with the three-button navigation bar visible.
- Verify that no primary interactive element is obscured at the top or bottom of the screen on any configuration.

---

## Platform Considerations

### Web

- Use Tailwind's responsive prefix classes (`sm:`, `md:`, `lg:`, `xl:`, `2xl:`) for breakpoint adaptation. Write the mobile layout first, then add breakpoint variants to enhance.
- Flexbox and CSS Grid compose cleanly in Tailwind — use `flex`, `grid`, `grid-cols-*`, `gap-*` utilities.
- Use `container mx-auto px-4 sm:px-6 lg:px-8` as the standard content container pattern for horizontal padding that scales with breakpoints.
- Avoid `position: fixed` elements that conflict with iOS Safari's dynamic address bar (which changes the viewport height as the user scrolls). Use `dvh` (dynamic viewport height) instead of `vh` for full-height containers on mobile web.

### Mobile (React Native / Expo)

- Use `useWindowDimensions()` to get the current screen width and height reactively. This updates on orientation change — do not cache dimensions at module load time.
- NativeWind supports responsive variants using screen-size-based prefixes — use these consistently with the same breakpoint vocabulary as web where the framework allows.
- **Orientation-aware layout:** When the device rotates, `useWindowDimensions()` triggers a re-render. Ensure layouts that depend on `width` or `height` re-compute gracefully. Test both portrait and landscape for any screen that users may reasonably rotate (e.g., media viewers, keyboards).
- Use `KeyboardAvoidingView` with `behavior="padding"` (iOS) or `behavior="height"` (Android) to prevent the software keyboard from obscuring input fields. Combine with `ScrollView` where the form is longer than the screen.
