
## Color Philosophy

Color in Chatverce is **functional, not decorative**. Every color decision must answer "why does this help the user understand or act?" — not "does this look nice?"

**The identity palette** is derived from the Chatverce "C" logo mark: Midnight Navy (`#1B2A4A`) and Chatverce Teal (`#2EC4B6`). These two colors communicate intelligence, focus, and trustworthy technology. Navy anchors structure; teal signals action, life, and AI activity.

**The identity is navy + teal — intelligence, not messaging.** This distinction is intentional and must be upheld:

- Never use WhatsApp green (`#25D366`) as a primary brand color. Green is reserved exclusively for two purposes: the WhatsApp channel indicator, and success/positive states (agent live, task complete). Elevating green to primary collapses the product's identity into a messaging app clone.
- Teal is distinct from green. Chatverce Teal (`#2EC4B6`) sits in the blue-green spectrum — readable as sophisticated and AI-native, not as a chat bubble color.

**Color roles are semantic, not literal.** Use token names (`cv-text-primary`, `cv-bg-accent`), never raw hex values in application code. Tokens adapt to themes; raw values do not.


## Semantic Color Usage

All tokens are defined in `semantic-light.md` and `semantic-dark.md`. This table summarizes purpose and correct/incorrect usage patterns.

### Background Tokens

| Token | Purpose | Do | Don't |
|-------|---------|-----|-------|
| `cv-bg-primary` | Page/app base background | Use for the root layout background | Use for cards — too flat, no elevation contrast |
| `cv-bg-surface` | Cards, panels, dialogs | Use for any floating or contained content area | Use for the page root — loses depth |
| `cv-bg-surface-elevated` | Modals, dropdowns, popovers | Use for content that floats above the page plane | Overuse — reserve for truly elevated layers |
| `cv-bg-surface-sunken` | Input fields, inset areas | Use for text inputs, code blocks, inset sections | Use for primary content — feels recessed |
| `cv-bg-accent` | Primary CTA backgrounds, active nav | Use for the one primary action per screen | Scatter across multiple elements — dilutes urgency |
| `cv-bg-accent-subtle` | Teal-tinted backgrounds, AI glow states | Use for "AI is working" indicators, selected highlights | Use as a general background — too distinctive |
| `cv-bg-accent-hover` | Accent hover/pressed state | Apply on `:hover` and `:active` over `cv-bg-accent` | Use as a default state |
| `cv-bg-destructive` | Delete/destructive action backgrounds | Use only for confirmed destructive action elements | Use for warnings — that is `cv-status-warning` |
| `cv-bg-destructive-subtle` | Error banners, toast backgrounds | Use for inline error states and error toasts | Combine with red text without checking contrast |

### Text Tokens

| Token | Purpose | Do | Don't |
|-------|---------|-----|-------|
| `cv-text-primary` | Body text, headings | Use for all primary readable content | Use on dark or accent backgrounds directly |
| `cv-text-secondary` | Descriptions, labels, timestamps | Use for supporting information one visual level below primary | Use for critical information — users may skim it |
| `cv-text-tertiary` | Placeholders, disabled text | Use for placeholder copy and truly disabled states | Use for any interactive or informational text |
| `cv-text-on-accent` | Text/icons on teal backgrounds | Use whenever text sits on `cv-bg-accent` | Use on light or surface backgrounds |
| `cv-text-on-destructive` | Text/icons on red backgrounds | Use whenever text sits on `cv-bg-destructive` | Mix with `cv-text-on-accent` — they have different bases |
| `cv-text-link` | Inline links, clickable text | Use for hyperlinks and inline navigation cues | Underline-only differentiation without color |
| `cv-text-success` | Success messages, confirmations | Use for positive outcome copy | Use green as decoration unrelated to success |
| `cv-text-warning` | Warning messages, attention text | Use for non-critical alerts needing attention | Use amber for branding accents |
| `cv-text-error` | Error messages, validation feedback | Use for form validation and critical failure messages | Use red for decorative purposes |

### Border Tokens

| Token | Purpose | Do | Don't |
|-------|---------|-----|-------|
| `cv-border-default` | Dividers, default input borders | Use for resting input borders and structural dividers | Make borders heavier — they should recede |
| `cv-border-strong` | Emphasized borders, focused inputs | Use for `:focus` state on inputs and emphasized separators | Use as a default — too heavy at rest |
| `cv-border-accent` | Focused state on accent elements | Use for focus rings on accent-colored interactive elements | Use on non-interactive elements |
| `cv-border-error` | Invalid input borders | Use when input validation fails | Use red borders for non-error decoration |

### Status Tokens

| Token | Purpose | Do | Don't |
|-------|---------|-----|-------|
| `cv-status-success` | Agent live, task complete | Use for positive status indicators, often as a dot/icon fill | Use green text on green background — check contrast |
| `cv-status-warning` | Attention needed | Use for caution states, degraded conditions | Mistake for error — warning is cautionary, not critical |
| `cv-status-error` | Failures, destructive actions | Use for system errors, failed tasks, outages | Use as a substitute for `cv-bg-destructive` |
| `cv-status-info` | Informational states, AI activity | Use for "thinking", "processing", "online" AI indicators | Overuse — reserve for genuine AI/system status |


## Contrast Requirements

All text rendered in the product must meet WCAG 2.1 contrast minimums. The preferred standard is Enhanced (AAA) wherever feasible.

| Standard | Ratio | Applies to |
|----------|-------|-----------|
| WCAG AA (minimum) | 4.5:1 | Normal body text (< 18px regular / < 14px bold) |
| WCAG AA Large Text | 3:1 | Large text (≥ 18px regular / ≥ 14px bold), UI components, graphical elements |
| WCAG AAA (preferred) | 7:1 | Body text on critical or long-read surfaces |

**Rules:**

1. Always verify contrast when combining a text token with a background token. The `contrast-matrix.md` file documents approved combinations.
2. Never introduce a new text/background pairing without computing the ratio and adding it to the matrix.
3. Decorative elements (dividers, icon backgrounds when the icon is also labeled) are exempt from the 4.5:1 rule but must still meet 3:1.
4. Placeholder text (`cv-text-tertiary`) intentionally targets ~3.5:1 — acceptable for placeholder copy per WCAG's non-content text guidance, but should trend higher when feasible.


## Color Accessibility

**Never rely on color alone to convey meaning.** Color is a visual shortcut; it is not a universal communication channel. Users with color vision deficiencies, low-contrast displays, or high-contrast OS modes must receive identical meaning through non-color channels.

| Meaning | Color signal | Required secondary signal |
|---------|-------------|--------------------------|
| Error state | Red border / red text | Error icon (e.g., `AlertCircle`) + error message text |
| Success state | Green indicator | Check icon or "Complete" label |
| Warning state | Amber indicator | Warning icon (e.g., `AlertTriangle`) + descriptive text |
| Active/selected | Teal accent | Selected indicator, bold label, or checked state |
| Required field | — | Asterisk (`*`) or "Required" label — never color alone |
| Disabled state | Muted color | `disabled` attribute / `aria-disabled` + cursor style |

**Dark mode and high-contrast modes**: All semantic tokens have dark theme counterparts in `semantic-dark.md`. Never hard-code light-theme hex values in components — always use tokens so the theming layer handles mode switching transparently.

**Colorblind simulation**: Before shipping a new color-reliant pattern, verify it under Deuteranopia (red-green) and Protanopia simulations. Both Chrome DevTools and Figma provide these simulations natively.


## Platform Considerations

### Web (CSS Custom Properties)

Tokens are exposed as CSS custom properties defined in `tokens-css.md`. Light/dark switching is handled at the `:root` level via a `data-theme` attribute or the `prefers-color-scheme` media query.

```css
/* Light theme default */
:root {
  --cv-text-primary: #1A1D23;
  --cv-bg-primary: #FAFBFC;
}

/* Dark theme */
[data-theme="dark"],
@media (prefers-color-scheme: dark) {
  --cv-text-primary: #FAFBFC;
  --cv-bg-primary: #060B15;
}
```

Use `var(--cv-text-primary)` in all CSS. Never reference hex directly.

### Mobile (NativeWind + React Native)

On mobile, NativeWind's dark mode variant is driven by the device's color scheme. Tokens are exposed via a `useThemeColors()` hook (or equivalent) that reads from the active scheme and returns resolved values.

```tsx
// Always reference tokens, never hard-code
const colors = useThemeColors();
<Text style={{ color: colors['cv-text-primary'] }} />

// With NativeWind, use semantic class names
<Text className="text-cv-text-primary" />
```

Dark mode switching on mobile must respect the system setting. Do not build a manual light/dark toggle that overrides the OS preference without an explicit user action.


## Adding New Colors

New colors should be rare. The palette is intentionally constrained to maintain visual coherence.

**Process for adding a new color:**

1. **Justify the gap**: Document which existing token fails to serve the need and why. If an existing token can serve the purpose, use it.
2. **Add a primitive first**: New hues go into `primitives.md` as a named scale (e.g., `cv-purple-500`). Do not add one-off hex values.
3. **Create a semantic token**: Map the new primitive to a semantic purpose in both `semantic-light.md` and `semantic-dark.md`. Both themes are required simultaneously.
4. **Check contrast**: Add the new token combinations to `contrast-matrix.md` with verified contrast ratios.
5. **Expose via platform tokens**: Update `tokens-css.md`, `tokens-tailwind.md`, and `tokens-rn.md`.
6. **Update this file**: Add the new token to the Semantic Color Usage table above.

**Do not** add colors to serve a one-time illustration or marketing graphic. Those belong in static assets, not the token system.
