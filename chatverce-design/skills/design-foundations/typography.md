
## Type Scale

The complete Chatverce type scale. All sizes are ranges — the lower bound applies to mobile, the upper bound to desktop. Line heights are unitless multipliers.

| Role | Font | Weight | Size Range | Line Height | Usage |
|------|------|--------|-----------|-------------|-------|
| Display | Inter | 600 (Semibold) | 36–48px | 1.1 | Hero headlines, landing sections, onboarding splash |
| H1 | Inter | 600 (Semibold) | 28–32px | 1.2 | Page titles, primary screen headers |
| H2 | Inter | 600 (Semibold) | 20–24px | 1.3 | Section headings, dialog titles |
| H3 | Inter | 500 (Medium) | 16–18px | 1.4 | Card headings, group labels, sub-section titles |
| Body | Inter | 400 (Regular) | 14–16px | 1.5 | All paragraph text, descriptions, list items |
| Caption | Inter | 400 (Regular) | 12–13px | 1.4 | Timestamps, footnotes, helper text, metadata |
| Code | JetBrains Mono | 400 (Regular) | 13–14px | 1.5 | Code blocks, agent output, configuration values, JSON |

**Weight vocabulary:**
- 400 Regular — body copy, captions, code
- 500 Medium — H3 level, slightly elevated importance without shouting
- 600 Semibold — headings and display, assertive but not heavy
- 700 Bold — use sparingly for inline emphasis within body text only; never for entire paragraphs


## Why Inter

Inter is the singular font family for Chatverce (with JetBrains Mono as the monospace companion). The choice is deliberate:

**Geometric and screen-native.** Inter was designed specifically for computer screens — not adapted from a print typeface. Its letterforms are optimized for sub-pixel rendering, which makes it crisper than many alternatives at 14–16px body sizes where Chatverce spends most of its reading load.

**Clean without coldness.** Inter draws from the same humanist-geometric tradition as Apple's San Francisco and Google's Roboto, but with a neutrality that does not carry either company's brand identity. It reads as modern and precise — matching Chatverce's AI-native positioning — without feeling mechanical or clinical.

**Cross-platform compatibility.** Inter is available on Google Fonts, which means zero licensing friction for web deployment. It works natively with NativeWind on React Native, loads cleanly through `expo-font` on mobile, and renders consistently across macOS, Windows, iOS, and Android.

**Legibility at small sizes.** At Caption level (12–13px), Inter remains legible due to its open apertures and large x-height. Many fonts that look good at display sizes become ambiguous at 12px — Inter does not.

**JetBrains Mono** is used exclusively for code and agent output. It provides clear glyph disambiguation (0 vs O, 1 vs l vs I), consistent character width for alignment in tabular output, and a developer-native feel that signals "this is machine output" to the user.


## Hierarchy Rules

Typography creates hierarchy through scale, weight, and spacing — in that order of priority. Color and style (italic, underline) are secondary signals, not primary hierarchy tools.

**Rule 1 — Use the scale for hierarchy.** If you need something to feel "more important," move it up the scale (e.g., Body → H3). Do not invent custom sizes outside the scale.

**Rule 2 — Never skip heading levels.** Heading levels are semantic, not just visual. Screen readers and assistive technology rely on heading order. An H3 must always follow an H2 on the same page; H1 must precede H2. Skipping from H1 to H3 with no H2 is both a visual and accessibility violation.

**Rule 3 — Body minimum: 14px web / 16px mobile.** On web, 14px is the floor for any body-level readable text. On mobile, 16px is the minimum for text in input fields. Inputs rendered at less than 16px on iOS trigger automatic viewport zoom — a disorienting UX regression that must be avoided categorically.

**Rule 4 — Caption is for supporting content only.** Caption (12–13px) is appropriate for timestamps, helper text, footnotes, and metadata. It is not a substitute for body text when space is tight. If content is important enough to read, it is important enough to be at Body size.

**Rule 5 — One H1 per screen/page.** Every page or modal should have exactly one H1. Multiple H1s dilute hierarchy and harm SEO on web.

**Rule 6 — Reserve bold for inline emphasis.** 700 Bold within a Body paragraph draws the eye to a critical term or phrase. Using bold at heading level is redundant (headings already carry weight through scale and 600 Semibold). Do not make entire card titles bold — use H3 instead.


## Dynamic Sizing

Typography must adapt gracefully to different viewport sizes, user accessibility settings, and system font scale preferences.

### Web — Fluid Sizing with `clamp()`

Use CSS `clamp()` to create smooth fluid scaling between mobile and desktop bounds without hard breakpoint jumps.

```css
/* Display */
font-size: clamp(36px, 4vw + 1rem, 48px);

/* H1 */
font-size: clamp(28px, 3vw + 0.5rem, 32px);

/* H2 */
font-size: clamp(20px, 2vw + 0.5rem, 24px);

/* Body — use rem, never px, for user font size preference respect */
font-size: clamp(0.875rem, 1vw + 0.5rem, 1rem);
```

Always use `rem` for body-level text so that users who have set a browser-level font size preference (e.g., "Large" in Chrome accessibility settings) receive proportionally scaled text. `px` overrides this preference.

### Mobile — Dynamic Type (iOS) and Font Scaling (Android)

On mobile, the OS exposes a system font scale setting that users rely on for accessibility. Chatverce must honor it.

**`allowFontScaling` policy:** Set `allowFontScaling={true}` on all `<Text>` components that render readable content. Only disable it for UI-structural elements where layout would break at extreme scale (e.g., tab bar icons with labels) — and document the exception with a comment.

**iOS Dynamic Type:** Inter loaded through `expo-font` does not automatically connect to iOS Dynamic Type categories. Use scaled font sizes derived from the user's system font scale via `PixelRatio.getFontScale()` or `useWindowDimensions()` to compute responsive sizes.

**Android font scaling:** Android applies a global `fontScale` multiplier accessible via `PixelRatio.getFontScale()`. Ensure all layout containers that hold text use `flexShrink` or `numberOfLines` + `ellipsizeMode` to handle larger-than-expected text gracefully without clipping or overflow.

**Test at 200% enlargement.** Before shipping any screen, simulate 200% font scale. Every text container must either scroll, truncate gracefully, or reflow without hiding critical information.


## Font Loading

### Web

Load Inter via `@font-face` with `font-display: swap` to prevent invisible text during font load (FOIT — Flash of Invisible Text):

```css
@font-face {
  font-family: 'Inter';
  src: url('/fonts/inter-variable.woff2') format('woff2-variations');
  font-weight: 100 900;
  font-display: swap;
}

@font-face {
  font-family: 'JetBrains Mono';
  src: url('/fonts/jetbrains-mono-variable.woff2') format('woff2-variations');
  font-weight: 100 800;
  font-display: swap;
}
```

Use variable font files (single `.woff2` with weight axis) to reduce total font payload. Subset to Latin + Latin Extended if the product is English-only to further reduce file size.

Alternatively, use the Google Fonts `@import` or `<link>` approach for simpler setups — but prefer self-hosting for privacy (no Google request on page load) and performance (CDN control).

### Mobile

Use `expo-font` to load Inter and JetBrains Mono at app startup. Fonts must be loaded before the first render of text that uses them:

```tsx
import { useFonts } from 'expo-font';
import { Inter_400Regular, Inter_500Medium, Inter_600SemiBold } from '@expo-google-fonts/inter';
import { JetBrainsMono_400Regular } from '@expo-google-fonts/jetbrains-mono';

export function App() {
  const [fontsLoaded] = useFonts({
    'Inter-Regular': Inter_400Regular,
    'Inter-Medium': Inter_500Medium,
    'Inter-SemiBold': Inter_600SemiBold,
    'JetBrainsMono-Regular': JetBrainsMono_400Regular,
  });

  if (!fontsLoaded) return <SplashScreen />;
  return <RootNavigator />;
}
```

Show the splash screen or a neutral loading state until fonts are ready. Never render placeholder text with a system font and then swap — this produces a visible reflow on mobile that degrades perceived quality.


## Platform Considerations

### Web

- Use `rem` units for all font sizes in CSS to respect the browser's base font size preference.
- Set the root `font-size` to `100%` (not a fixed `px` value) so the system default propagates correctly.
- Apply `font-family: 'Inter', system-ui, -apple-system, sans-serif` as the global stack — the system fallbacks ensure readable text even if Inter fails to load.
- Apply `font-synthesis: none` to prevent browsers from generating faux bold or faux italic for weights that are not loaded.
- Apply `-webkit-font-smoothing: antialiased` and `moz-osx-font-smoothing: grayscale` on macOS/iOS for crispness; omit or override on Windows where ClearType rendering is preferred.

### Mobile (React Native / Expo)

- Font family names are exact strings matching the keys registered in `useFonts`. Use constants — not inline strings — to prevent typo-driven silent fallbacks to the system font.
- `fontWeight` in React Native StyleSheet requires the font file for that weight to be loaded. Specifying `fontWeight: '700'` without loading a Bold variant will silently fall back to the Regular weight on iOS and attempt a bold simulation on Android.
- **iOS 16px input minimum:** All `<TextInput>` components must use `fontSize: 16` (or larger) to prevent iOS from zooming the viewport on focus. This is non-negotiable UX behavior, not an optional guideline.
- Test on both iPhone SE (small screen, high pixel density) and iPad (large screen, different scale factor) to confirm the type scale reads correctly across the device range.
