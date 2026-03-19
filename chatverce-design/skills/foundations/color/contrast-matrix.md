# Contrast Matrix

> Contrast ratios for every approved text/background semantic token pair in Chatverce. Computed against WCAG 2.1 relative luminance using the resolved hex values from `semantic-light.md` and `semantic-dark.md`.
>
> **Legend:**
> - **PASS AA** — ≥ 4.5:1 · satisfies WCAG 2.1 AA for normal text
> - **PASS AA-Large** — ≥ 3.0:1 · satisfies WCAG 2.1 AA for large text (≥ 18px regular / ≥ 14px bold) and UI components; fails for normal body text
> - **PASS AAA** — ≥ 7.0:1 · satisfies WCAG 2.1 AAA (enhanced)
> - **FAIL** — below 3.0:1 · does not satisfy any WCAG text criterion
>
> Any pair marked **FAIL** or **PASS AA-Large** on body-text usage is flagged for adjustment. See notes column.

---

## Light Theme

| Text Token | Background Token | Text Hex | BG Hex | Ratio | WCAG Result | Notes |
|-----------|-----------------|----------|--------|-------|-------------|-------|
| `cv-text-primary` | `cv-bg-primary` | `#1A1D23` | `#FAFBFC` | 16.29:1 | PASS AA + AAA | Optimal — use freely |
| `cv-text-primary` | `cv-bg-surface` | `#1A1D23` | `#FFFFFF` | 16.88:1 | PASS AA + AAA | Optimal — use freely |
| `cv-text-primary` | `cv-bg-surface-sunken` | `#1A1D23` | `#F1F3F5` | 15.18:1 | PASS AA + AAA | Optimal — use freely |
| `cv-text-primary` | `cv-bg-destructive-subtle` | `#1A1D23` | `#FEF2F2` | 15.43:1 | PASS AA + AAA | Body text on error banners — safe |
| `cv-text-secondary` | `cv-bg-primary` | `#64748B` | `#FAFBFC` | 4.59:1 | PASS AA | Passes for normal text — verify at smallest sizes |
| `cv-text-secondary` | `cv-bg-surface` | `#64748B` | `#FFFFFF` | 4.76:1 | PASS AA | Passes for normal text |
| `cv-text-secondary` | `cv-bg-surface-sunken` | `#64748B` | `#F1F3F5` | 4.28:1 | PASS AA-Large | **Use at ≥ 18px regular or ≥ 14px bold only.** Do not use for small labels on input fields. |
| `cv-text-tertiary` | `cv-bg-primary` | `#94A3B8` | `#FAFBFC` | 2.47:1 | **FAIL** | **Intentional — for placeholder/disabled copy only.** Compliant under WCAG 1.4.3 Exception for placeholder text. Never use for readable content. |
| `cv-text-tertiary` | `cv-bg-surface` | `#94A3B8` | `#FFFFFF` | 2.56:1 | **FAIL** | Same exception as above — placeholder/disabled only |
| `cv-text-tertiary` | `cv-bg-surface-sunken` | `#94A3B8` | `#F1F3F5` | 2.31:1 | **FAIL** | Same exception — this is the primary placeholder use case (inputs) |
| `cv-text-on-accent` | `cv-bg-accent` | `#FFFFFF` | `#2EC4B6` | 2.17:1 | **FAIL** | **Needs adjustment.** White on Chatverce Teal fails all WCAG criteria. Use dark navy text (`#1B2A4A` or `cv-navy-900`) on teal instead. See adjustment note below. |
| `cv-text-link` | `cv-bg-primary` | `#24A99D` | `#FAFBFC` | 2.80:1 | **FAIL** | **Needs adjustment.** Teal-600 link text on light background fails. Must be accompanied by underline decoration to satisfy WCAG 1.4.1 (non-color differentiation). Consider darkening to `cv-teal-700` (`#1A7A70` approx). |
| `cv-text-link` | `cv-bg-surface` | `#24A99D` | `#FFFFFF` | 2.90:1 | **FAIL** | Same issue as above |
| `cv-text-success` | `cv-bg-primary` | `#047857` | `#FAFBFC` | 5.29:1 | PASS AA | Passes — use for success messages |
| `cv-text-success` | `cv-bg-surface` | `#047857` | `#FFFFFF` | 5.48:1 | PASS AA | Passes |
| `cv-text-warning` | `cv-bg-primary` | `#B45309` | `#FAFBFC` | 4.85:1 | PASS AA | Passes |
| `cv-text-warning` | `cv-bg-surface` | `#B45309` | `#FFFFFF` | 5.02:1 | PASS AA | Passes |
| `cv-text-error` | `cv-bg-primary` | `#DC2626` | `#FAFBFC` | 4.66:1 | PASS AA | Passes — verify at 14px+ |
| `cv-text-error` | `cv-bg-surface` | `#DC2626` | `#FFFFFF` | 4.83:1 | PASS AA | Passes |
| `cv-text-error` | `cv-bg-destructive-subtle` | `#DC2626` | `#FEF2F2` | 4.41:1 | PASS AA-Large | **Use at ≥ 18px regular or ≥ 14px bold.** Marginal at small text sizes. Combine with error icon. |

---

## Dark Theme

| Text Token | Background Token | Text Hex | BG Hex | Ratio | WCAG Result | Notes |
|-----------|-----------------|----------|--------|-------|-------------|-------|
| `cv-text-primary` | `cv-bg-primary` | `#FAFBFC` | `#060B15` | 19.01:1 | PASS AA + AAA | Optimal |
| `cv-text-primary` | `cv-bg-surface` | `#FAFBFC` | `#0A1120` | 18.20:1 | PASS AA + AAA | Optimal |
| `cv-text-primary` | `cv-bg-surface-elevated` | `#FAFBFC` | `#111C33` | 16.38:1 | PASS AA + AAA | Optimal — modals/dropdowns |
| `cv-text-primary` | `cv-bg-surface-sunken` | `#FAFBFC` | `#060B15` | 19.01:1 | PASS AA + AAA | Optimal — inputs |
| `cv-text-primary` | `cv-bg-destructive-subtle` | `#FAFBFC` | `#450A0A` | 15.58:1 | PASS AA + AAA | Body text on dark error banners — safe |
| `cv-text-secondary` | `cv-bg-primary` | `#94A3B8` | `#060B15` | 7.68:1 | PASS AA + AAA | Excellent for supporting text |
| `cv-text-secondary` | `cv-bg-surface` | `#94A3B8` | `#0A1120` | 7.35:1 | PASS AA + AAA | Excellent |
| `cv-text-secondary` | `cv-bg-surface-elevated` | `#94A3B8` | `#111C33` | 6.62:1 | PASS AA | Good — just below AAA; acceptable |
| `cv-text-tertiary` | `cv-bg-primary` | `#64748B` | `#060B15` | 4.14:1 | PASS AA-Large | **Use at ≥ 18px regular or ≥ 14px bold for body text.** Placeholder exception applies at smaller sizes. |
| `cv-text-tertiary` | `cv-bg-surface` | `#64748B` | `#0A1120` | 3.96:1 | PASS AA-Large | Same as above — placeholder exception applies |
| `cv-text-on-accent` | `cv-bg-accent` | `#060B15` | `#42CBB9` | 9.82:1 | PASS AA + AAA | Dark navy on brighter teal — excellent contrast in dark mode |
| `cv-text-link` | `cv-bg-primary` | `#42CBB9` | `#060B15` | 9.82:1 | PASS AA + AAA | Teal-400 links on dark background — excellent |
| `cv-text-link` | `cv-bg-surface` | `#42CBB9` | `#0A1120` | 9.40:1 | PASS AA + AAA | Excellent |
| `cv-text-success` | `cv-bg-primary` | `#34D399` | `#060B15` | 10.24:1 | PASS AA + AAA | Excellent |
| `cv-text-success` | `cv-bg-surface` | `#34D399` | `#0A1120` | 9.81:1 | PASS AA + AAA | Excellent |
| `cv-text-warning` | `cv-bg-primary` | `#FBBF24` | `#060B15` | 11.80:1 | PASS AA + AAA | Excellent |
| `cv-text-warning` | `cv-bg-surface` | `#FBBF24` | `#0A1120` | 11.29:1 | PASS AA + AAA | Excellent |
| `cv-text-error` | `cv-bg-primary` | `#F87171` | `#060B15` | 7.12:1 | PASS AA + AAA | Excellent |
| `cv-text-error` | `cv-bg-surface` | `#F87171` | `#0A1120` | 6.82:1 | PASS AA | Good — just below AAA |
| `cv-text-error` | `cv-bg-destructive-subtle` | `#F87171` | `#450A0A` | 5.84:1 | PASS AA | Passes for error text on dark error banner |

---

## Adjustment Notes

### 1. `cv-text-on-accent` / `cv-bg-accent` — Light Theme (2.17:1 — FAIL)

White text on Chatverce Teal (`#2EC4B6`) fails WCAG at all sizes (2.17:1). **Recommended fix:** Change `cv-text-on-accent` in the light semantic token to use `cv-navy-900` (`#0A1120`) or `cv-navy-950` (`#060B15`) instead of white. Dark navy on teal achieves ~9.8:1 (AAA), which is the dark theme's already-correct approach.

Until the token is updated, do not render small text directly on `cv-bg-accent` in light mode. Use icons + labels at large sizes only, or apply an accessible dark text override.

### 2. `cv-text-link` / `cv-bg-primary` and `cv-bg-surface` — Light Theme (2.80–2.90:1 — FAIL)

Teal-600 (`#24A99D`) as link text on white/near-white backgrounds does not meet WCAG 1.4.3. Two remedies:

**Option A (preferred):** Darken `cv-text-link` to `cv-teal-800` (approximately `#0F5C56`) to achieve ≥ 4.5:1.

**Option B (acceptable with caveats):** Keep the teal hue but add a permanent `text-decoration: underline` to all link instances. Under WCAG 1.4.1, links that differ from surrounding text by both color and non-color visual cue (underline) do not require the 4.5:1 body text contrast ratio — they require only 3:1 against adjacent text. This is only valid when links appear inline with body text, not as standalone navigation items.

### 3. `cv-text-secondary` / `cv-bg-surface-sunken` — Light Theme (4.28:1 — PASS AA-Large only)

Secondary text (Slate `#64748B`) on sunken input backgrounds (`#F1F3F5`) fails WCAG AA for body-sized text. This pairing occurs when rendering character count labels or helper text inside or adjacent to input fields at small sizes. **Recommended fix:** Restrict this combination to text ≥ 18px regular or ≥ 14px bold, or elevate secondary text to use `cv-text-primary` in input context.

---

## How to Use This Matrix

1. Before rendering text on any background, find the token pair in this table.
2. If the pair is not listed, compute the contrast ratio and add a new row before shipping.
3. If your pair shows FAIL: do not use it. Select an alternative or apply the adjustment note.
4. If your pair shows PASS AA-Large: only use it for text that is definitively large (heading level, ≥ 18px, or bold ≥ 14px).
5. Status indicator dots and decorative icon fills that carry no text are exempt from 4.5:1 but must meet 3:1 against their immediate background per WCAG 1.4.11 (Non-text Contrast).
