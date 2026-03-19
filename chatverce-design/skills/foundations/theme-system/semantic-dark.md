# Semantic Tokens — Dark Theme

> Maps semantic token names to their primitive token sources for the **dark** color scheme. Reference these tokens in application code — never reference primitive tokens directly.

All hex values resolved from `primitives.md`. Every token present in `semantic-light.md` has a matching entry here.

---

## Backgrounds

| Token | Primitive | Resolved Hex | Notes |
|-------|-----------|--------------|-------|
| cv-bg-primary | cv-navy-950 | #060B15 | Page/app background — near-black navy |
| cv-bg-surface | cv-navy-900 | #0A1120 | Cards, panels, dialogs |
| cv-bg-surface-elevated | cv-navy-800 | #111C33 | Modals, dropdowns, popovers |
| cv-bg-surface-sunken | cv-navy-950 | #060B15 | Input fields, inset areas |
| cv-bg-accent | cv-teal-400 | #42CBB9 | Primary CTA backgrounds — brighter for dark contrast |
| cv-bg-accent-subtle | cv-teal-950 | #052623 | Teal-tinted backgrounds, AI glow states |
| cv-bg-accent-hover | cv-teal-300 | #61D4C9 | Accent hover/pressed state |
| cv-bg-destructive | cv-red-400 | #F87171 | Delete/destructive action backgrounds |
| cv-bg-destructive-subtle | cv-red-950 | #450A0A | Error/warning banners, toast backgrounds |

---

## Text

| Token | Primitive | Resolved Hex | Notes |
|-------|-----------|--------------|-------|
| cv-text-primary | cv-gray-50 | #FAFBFC | Body text, headings on dark |
| cv-text-secondary | cv-gray-400 | #94A3B8 | Descriptions, labels, timestamps |
| cv-text-tertiary | cv-gray-500 | #64748B | Placeholders, disabled text |
| cv-text-on-accent | cv-navy-950 | #060B15 | Text/icons on teal backgrounds |
| cv-text-on-destructive | white | #FFFFFF | Text/icons on red backgrounds |
| cv-text-link | cv-teal-400 | #42CBB9 | Inline links, clickable text |
| cv-text-success | cv-green-400 | #34D399 | Success messages, confirmations |
| cv-text-warning | cv-amber-400 | #FBBF24 | Warning messages, attention text |
| cv-text-error | cv-red-400 | #F87171 | Error messages, validation feedback |

---

## Borders

| Token | Primitive | Resolved Hex | Notes |
|-------|-----------|--------------|-------|
| cv-border-default | cv-navy-700 | #1B2A4A | Dividers, default input borders |
| cv-border-strong | cv-navy-600 | #2A3D5E | Emphasized borders, focused inputs |
| cv-border-accent | cv-teal-400 | #42CBB9 | Focused state on accent elements |
| cv-border-error | cv-red-400 | #F87171 | Invalid input borders |

---

## Status

| Token | Primitive | Resolved Hex | Notes |
|-------|-----------|--------------|-------|
| cv-status-success | cv-green-400 | #34D399 | Agent live, task complete |
| cv-status-warning | cv-amber-400 | #FBBF24 | Attention needed |
| cv-status-error | cv-red-400 | #F87171 | Failures, destructive actions |
| cv-status-info | cv-teal-400 | #42CBB9 | Informational states, AI activity |

---

## Overlay

| Token | Primitive | Resolved Value | Notes |
|-------|-----------|----------------|-------|
| cv-shadow-color | — | 0,0,0 | Black RGB components for box-shadow use |
| cv-overlay | black @ 60% | rgba(0,0,0,0.6) | Modal scrim, drawer backdrop — deeper than light |
