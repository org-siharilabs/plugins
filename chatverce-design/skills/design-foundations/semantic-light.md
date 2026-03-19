# Semantic Tokens — Light Theme

> Maps semantic token names to their primitive token sources for the **light** color scheme. Reference these tokens in application code — never reference primitive tokens directly.

All hex values resolved from `primitives.md`.

---

## Backgrounds

| Token | Primitive | Resolved Hex | Notes |
|-------|-----------|--------------|-------|
| cv-bg-primary | cv-gray-50 | #FAFBFC | Page/app background |
| cv-bg-surface | white | #FFFFFF | Cards, panels, dialogs |
| cv-bg-surface-elevated | white | #FFFFFF | Modals, dropdowns, popovers |
| cv-bg-surface-sunken | cv-gray-100 | #F1F3F5 | Input fields, inset areas |
| cv-bg-accent | cv-teal-500 | #2EC4B6 | Primary CTA backgrounds, active nav |
| cv-bg-accent-subtle | cv-teal-50 | #EDFAF8 | Teal-tinted backgrounds, AI glow states |
| cv-bg-accent-hover | cv-teal-600 | #24A99D | Accent hover/pressed state |
| cv-bg-destructive | cv-red-500 | #EF4444 | Delete/destructive action backgrounds |
| cv-bg-destructive-subtle | cv-red-50 | #FEF2F2 | Error/warning banners, toast backgrounds |

---

## Text

| Token | Primitive | Resolved Hex | Notes |
|-------|-----------|--------------|-------|
| cv-text-primary | cv-gray-900 | #1A1D23 | Body text, headings — Ink |
| cv-text-secondary | cv-gray-500 | #64748B | Descriptions, labels, timestamps — Slate |
| cv-text-tertiary | cv-gray-400 | #94A3B8 | Placeholders, disabled text — Mist |
| cv-text-on-accent | white | #FFFFFF | Text/icons on teal backgrounds |
| cv-text-on-destructive | white | #FFFFFF | Text/icons on red backgrounds |
| cv-text-link | cv-teal-600 | #24A99D | Inline links, clickable text |
| cv-text-success | cv-green-700 | #047857 | Success messages, confirmations |
| cv-text-warning | cv-amber-700 | #B45309 | Warning messages, attention text |
| cv-text-error | cv-red-600 | #DC2626 | Error messages, validation feedback |

---

## Borders

| Token | Primitive | Resolved Hex | Notes |
|-------|-----------|--------------|-------|
| cv-border-default | cv-gray-200 | #E2E8F0 | Dividers, default input borders — Frost |
| cv-border-strong | cv-gray-300 | #CBD5E1 | Emphasized borders, focused inputs |
| cv-border-accent | cv-teal-500 | #2EC4B6 | Focused state on accent elements |
| cv-border-error | cv-red-500 | #EF4444 | Invalid input borders |

---

## Status

| Token | Primitive | Resolved Hex | Notes |
|-------|-----------|--------------|-------|
| cv-status-success | cv-green-500 | #10B981 | Agent live, task complete — Meadow |
| cv-status-warning | cv-amber-500 | #F59E0B | Attention needed — Amber |
| cv-status-error | cv-red-500 | #EF4444 | Failures, destructive actions — Coral |
| cv-status-info | cv-teal-500 | #2EC4B6 | Informational states, AI activity |

---

## Overlay

| Token | Primitive | Resolved Value | Notes |
|-------|-----------|----------------|-------|
| cv-shadow-color | — | 0,0,0 | Black RGB components for box-shadow use |
| cv-overlay | cv-gray-900 @ 50% | rgba(26,29,35,0.5) | Modal scrim, drawer backdrop |
