
# Motion Foundation

## Motion Philosophy

> "Motion serves comprehension, never decoration." — Jony Ive

Motion in Chatverce exists to help users understand what happened, where they are, and what will happen next. It is never used to impress, entertain, or fill silence.

**Principles:**
- **Purposeful:** Every animation must communicate something — a state change, a hierarchy, a relationship.
- **Not gratuitous:** If removing an animation doesn't confuse the user, the animation shouldn't exist.
- **Optional:** All motion must be optional. Users with reduced-motion preferences must get a fully functional, non-disorienting experience.

If you cannot articulate why an animation helps the user understand the interface, remove it.

## Duration Tokens

| Token | Value | Use |
|---|---|---|
| `cv-duration-fast` | 150ms | Micro-interactions, hover states, toggles |
| `cv-duration-normal` | 300ms | Enters, exits, panel transitions |
| `cv-duration-slow` | 500ms | Full-screen transitions, onboarding steps |

Never use values outside these tokens. Do not animate at durations over 500ms — it feels broken.

## Easing Tokens

| Token | Cubic Bezier | Use |
|---|---|---|
| `cv-ease-default` | `cubic-bezier(0.4, 0, 0.2, 1)` | General purpose — smooth in and out |
| `cv-ease-out` | `cubic-bezier(0, 0, 0.2, 1)` | Elements entering the screen |
| `cv-ease-in` | `cubic-bezier(0.4, 0, 1, 1)` | Elements leaving the screen |

Use `cv-ease-out` for enters and `cv-ease-in` for exits. This matches physical intuition — things accelerate as they leave and decelerate as they arrive.

## Animation Patterns

### Enter

Elements entering the viewport or being revealed:

- **Transform:** fade in + translate upward 8px → 0px
- **Duration:** `cv-duration-normal` (300ms)
- **Easing:** `cv-ease-out`

```css
/* Web */
@keyframes cv-enter {
  from { opacity: 0; transform: translateY(8px); }
  to   { opacity: 1; transform: translateY(0); }
}
animation: cv-enter cv-duration-normal cv-ease-out;
```

### Exit

Elements leaving the viewport or being hidden:

- **Transform:** fade out only (no translate on exit — it's disorienting)
- **Duration:** `cv-duration-fast` (150ms)
- **Easing:** `cv-ease-in`

```css
/* Web */
@keyframes cv-exit {
  from { opacity: 1; }
  to   { opacity: 0; }
}
animation: cv-exit cv-duration-fast cv-ease-in;
```

### Micro-interaction

Button presses, toggles, icon state changes:

- **Duration:** `cv-duration-fast` (150ms)
- **Easing:** `cv-ease-default`
- **Example:** scale 1 → 0.97 on press, back to 1 on release

### AI Working

The only permitted "decorative" motion. Used to communicate that the system is processing — not to entertain.

- **Style:** Gentle teal pulse on the AI indicator or generation cursor
- **Color:** Teal brand color (`cv-color-teal-500`)
- **Pattern:** Opacity pulse, not scale or bounce
- **Duration:** `cv-duration-slow` loop
- **Constraint:** Stop immediately when AI output begins. Do not loop indefinitely.

This is the single exception to the "no decorative motion" rule, and only because it communicates system state.

## Never List

These animations are banned in Chatverce. No exceptions.

- **Bouncing** — Feels playful, not professional. Users notice it on every interaction.
- **Spinning loaders** — Spinning that doesn't indicate real progress is noise. Use skeleton screens or the AI pulse.
- **Confetti / particles** — Not a consumer app. Never.
- **Parallax** — Causes motion sickness. Accessibility and usability failure.
- **Auto-playing carousels** — Users cannot process moving content. They will ignore it.
- **Spring/elastic overshoot** — Looks unstable in a business tool.

If a designer or developer proposes any of the above, link to this document.

## Reduced Motion

Users who have enabled reduced-motion settings must receive a fully functional experience with all animations removed or replaced with instant transitions.

### Web

Use the Tailwind `motion-safe:` prefix for all animation utilities:

```html
<!-- Only animate if the user has not requested reduced motion -->
<div class="motion-safe:animate-cv-enter">...</div>
```

At the CSS level, zero all durations when the preference is set:

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

This global rule acts as a safety net. Still use `motion-safe:` at the component level for clarity.

### Mobile (React Native)

Use Reanimated's `useReducedMotion()` hook or the native API:

```typescript
import { useReducedMotion } from 'react-native-reanimated';

const prefersReducedMotion = useReducedMotion();
const duration = prefersReducedMotion ? 0 : 300;
```

Or with the native API for non-Reanimated code:

```typescript
import { AccessibilityInfo } from 'react-native';

const isReduceMotionEnabled = await AccessibilityInfo.isReduceMotionEnabled();
```

Check the preference at the component level — do not assume the preference is false. All animated components must handle the `duration: 0` case gracefully (instant transition, no visual glitch).

## Haptics (Mobile Only)

Haptics are the mobile equivalent of audio feedback — they confirm actions without requiring the user to look at the screen. Used correctly, they make the app feel polished and trustworthy. Used incorrectly, they feel spammy.

**Library:** `expo-haptics`

### Pattern Reference

| Interaction | Haptic Pattern | `expo-haptics` Call |
|---|---|---|
| Button press | Light impact | `Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light)` |
| Toggle switch | Light impact | `Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light)` |
| Destructive confirm (delete, disconnect) | Warning notification | `Haptics.notificationAsync(Haptics.NotificationFeedbackType.Warning)` |
| Success (going live, save complete) | Success notification | `Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success)` |
| Error (validation failure) | Error notification | `Haptics.notificationAsync(Haptics.NotificationFeedbackType.Error)` |
| Pull to refresh | Selection changed | `Haptics.selectionAsync()` |
| Long press menu open | Medium impact | `Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium)` |

### Rules

1. **Never haptics without a visual counterpart.** The haptic reinforces the visual — it does not replace it. A button press must still show a pressed state; the haptic is additive.
2. **Respect the system setting.** Check `expo-haptics` availability; it will no-op on simulators and devices with haptics disabled. Do not attempt to force haptics when the system has them off.
3. **Do not haptic on scroll.** Continuous haptic feedback during scroll is extremely disorienting.
4. **One haptic per action.** Do not stack multiple haptic calls for a single user action.
