---
name: icon
description: Scalable vector icon from the Lucide icon set, used to reinforce labels and actions
user_invocable: false
---

# Icon

**Gallery reference:** https://component.gallery/components/icon/
**Aliases:** Glyph, Pictogram, Symbol

## When to Use

Use Icon to reinforce a label's meaning, reduce cognitive load in dense UIs, or indicate common patterns (close, search, settings, chevron). Always pair icons with visible text labels unless the icon is universally understood and space is critically constrained. When standalone, provide an accessible label.

## When NOT to Use

Do not use Icon as the sole indicator of an action without an `aria-label`. Do not create custom icons — always use the Lucide library. Do not use icons with inconsistent stroke widths or fill styles — Lucide is stroke-only.

## Anatomy

- **SVG element:** From `lucide-react` or `@lucide/react-native`.
- **Size:** 20px default. 16px compact. 24px large/standalone. Always a multiple of 4px.
- **Stroke:** 1.5px. Do not change stroke width.
- **Color:** Inherits current text color (`currentColor`) by default.

## Variants

| Variant | Size | Use |
|---------|------|-----|
| Inline (16px) | 16px | Inside button labels, badges, tags |
| Default (20px) | 20px | List items, nav labels, form field prefixes |
| Large (24px) | 24px | Standalone icons, empty states, feature highlights |

## Chatverce Styling

- Color: inherits from parent (`currentColor`). Override with `cv-text-*` token on the icon wrapper.
- Stroke width: always 1.5 (Lucide default — do not override)
- Size: `size-4` (16px), `size-5` (20px), `size-6` (24px) in Tailwind
- Never apply background fill to icons — icons are strokes only in this system
- Decorative icons must have `aria-hidden="true"`

## Platform Considerations

Web: Import from `lucide-react`. Mobile: Import from `lucide-react-native`. Both share the same icon names and API. On mobile, explicitly set `width` and `height` props — NativeWind size classes may not apply consistently to SVG.

## Accessibility

- Decorative icons (icon + visible label): `aria-hidden="true"` on the icon.
- Standalone icons (no visible label): wrap in a `<button>` or `<span>` with `aria-label`.
- Mobile: decorative icons inside labeled buttons require no extra treatment. Standalone icons need `accessibilityLabel` on the parent pressable.

## Code Example

```tsx
import { Settings, ChevronRight, Search } from "lucide-react";

{/* Decorative — paired with visible label */}
<button className="flex items-center gap-2 text-[--cv-text-primary]">
  <Settings size={20} aria-hidden="true" />
  Settings
</button>

{/* Standalone — aria-label required */}
<button aria-label="Search conversations" className="text-[--cv-text-secondary]">
  <Search size={20} />
</button>

{/* As a directional cue */}
<ChevronRight size={16} aria-hidden="true" className="text-[--cv-text-tertiary]" />
```

## Anti-Patterns

- Never change the stroke width of Lucide icons — consistency across the system depends on 1.5px.
- Never use icons without labels in primary actions unless the icon is universally understood (e.g., ✕ close).
- Never use emoji as icons — they are not scalable, stylable, or consistently rendered across platforms.
- Never use icons from multiple libraries in the same product.
