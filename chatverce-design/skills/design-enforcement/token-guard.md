
# Token Guard

Token guard is an enforcement layer that runs during implementation and code review. Its job is singular: prevent hardcoded values from bypassing the Chatverce token system. Every color, spacing value, radius, shadow, z-index, font, duration, and easing used in UI code must trace back to a `cv-*` token. If it does not, it is a violation.

This guard is not about preference. Token violations create divergence between the design system and the implementation, make theme switching impossible, and erode the visual consistency the system was built to guarantee.


## Violation Reference

| Violation | Pattern | Correct Fix |
|---|---|---|
| Raw hex color | `bg-[#1B2A4A]`, `color: '#fff'`, `style={{ color: '#64748B' }}` | `bg-cv-*` or `text-cv-*` semantic tokens |
| Raw RGB/HSL | `rgb(27, 42, 74)`, `hsl(215, 46%, 20%)`, `rgba(0,0,0,0.5)` | Semantic token (e.g., `cv-color-surface-overlay`) |
| Arbitrary z-index | `z-[999]`, `z-[9999]`, `zIndex: 50`, `zIndex: 9999` | `z-cv-*` scale token |
| Arbitrary font family | `font-['Helvetica']`, `font-['Arial']`, `fontFamily: 'Georgia'` | `font-cv-sans` or `font-cv-mono` |
| Arbitrary shadow | `shadow-[0_4px_16px_rgba(0,0,0,0.12)]`, `boxShadow: '...'` | `shadow-cv-*` |
| Arbitrary border radius | `rounded-[10px]`, `rounded-[3px]`, `borderRadius: 10` | `rounded-cv-*` |
| Arbitrary spacing | `p-[13px]`, `m-[22px]`, `padding: 13`, `gap: 7` | Nearest scale value (`p-3`, `p-4`, etc.) or `cv-spacing-*` token |
| Arbitrary duration | `duration-[200ms]`, `duration-[400ms]`, `transitionDuration: '250ms'` | `duration-cv-fast` (150ms), `duration-cv-normal` (300ms), `duration-cv-slow` (500ms) |
| Arbitrary easing | `ease-[cubic-bezier(0.25,0.1,0.25,1)]`, `transitionTimingFunction: 'ease-in-out'` | `ease-cv-default`, `ease-cv-out`, `ease-cv-in` |
| RN inline color | `style={{ backgroundColor: '#fff' }}`, `style={{ color: '#1A1D23' }}` | NativeWind class using `bg-cv-*` or `text-cv-*` |
| Raw opacity for theming | `opacity-50` used to darken/lighten a hardcoded color for a theme variant | Dedicated semantic token for that state |


## Violation Detection: What to Look For

When reading or reviewing implementation code, scan for these patterns:

**Tailwind arbitrary values (square bracket syntax):**
```
bg-[#...]     text-[#...]     border-[#...]
bg-[rgb(...)] text-[hsl(...)]
z-[any number]
font-['any font name']
shadow-[...]
rounded-[...px]  rounded-[...rem]
p-[...]  m-[...]  gap-[...]  w-[...px]  h-[...px]   (spacing only — layout dimensions may be intentional)
duration-[...]
ease-[...]
```

**Inline styles (React / React Native):**
```javascript
style={{ backgroundColor: '...' }}   // any literal color value
style={{ color: '...' }}
style={{ zIndex: ... }}               // any numeric z-index
style={{ fontFamily: '...' }}
style={{ boxShadow: '...' }}
style={{ borderRadius: ... }}         // numeric literals
style={{ padding/margin: ... }}       // numeric literals not tied to a token
style={{ transitionDuration: '...' }}
style={{ transitionTimingFunction: '...' }}
```

**CSS-in-JS / plain CSS:**
```css
color: #...;           background: #...;
background-color: rgb(...);
z-index: [any number];
font-family: [anything other than cv-font-*];
box-shadow: [literal values];
border-radius: [px/rem literal];
padding/margin: [px/rem literal not on the spacing scale];
transition-duration: [ms literal];
transition-timing-function: cubic-bezier(...);
```


## Exceptions

The following are legitimate exceptions. Do not flag these as violations:

1. **Primitive definition files** — Files that define the token system itself (e.g., `primitives.md`, `tailwind.config.js` token definitions, CSS custom property definitions). These files must use raw values to create the token layer that all other code references.

2. **Override annotation** — A line containing `/* cv-override */` has been deliberately reviewed and approved. Do not flag it.

3. **Exception annotation** — A line containing `/* cv-exception: [reason] */` has been reviewed with a documented justification. Do not flag it.

4. **SVG and image data** — Color values embedded in SVG paths, `data:` URIs, or image assets are outside the scope of the token system.

5. **Third-party component theming** — When configuring a third-party library that requires specific color formats (e.g., a chart library), use the token value but document it with `/* cv-exception: third-party [library name] */`.


## Self-Correction Protocol

When a violation is detected during implementation, the agent corrects it before saving the file. The correction sequence:

1. **Identify the hardcoded value** — What is the literal value? (e.g., `#1B2A4A`, `13px`, `200ms`)
2. **Identify the intent** — What visual property is this serving? (e.g., primary background, tight gap, fast transition)
3. **Find the correct token** — Look up the appropriate `cv-*` token for that intent. When uncertain, reference `foundations/theme-system/SKILL.md`.
4. **Replace and verify** — Substitute the hardcoded value with the token. Confirm the visual intent is preserved.
5. **Do not flag exceptions** — If the file is a primitive definition or has an override annotation, leave it as-is.

The agent does not save implementation files containing token violations. Correction happens in memory before the file write.


## Rationale

The Chatverce token system exists to make the impossible possible: one codebase that renders correctly in light and dark themes, across web and mobile, with a consistent visual vocabulary. Every hardcoded value is a hole in that system. A hardcoded `#ffffff` will be invisible on a white dark-mode background. A hardcoded `200ms` duration will never match the system's animation rhythm. A hardcoded `10px` radius will be visually inconsistent with every other surface.

Token violations are not minor style issues. They are structural failures.
