# Primitive Tokens

> These are raw values. Never reference in application code. Only used in semantic token definitions and token output files.

---

## Navy Scale

Anchored at `cv-navy-700 = #1B2A4A` (Midnight Navy — the Chatverce primary brand color). Lighter stops trend toward near-white blue-gray; darker stops toward near-black navy.

| Token | Hex | Notes |
|-------|-----|-------|
| cv-navy-50 | #EEF1F7 | Lightest navy tint — almost white with a hint of blue |
| cv-navy-100 | #D5DCE9 | Very light blue-gray |
| cv-navy-200 | #B3BFCE | Light blue-gray |
| cv-navy-300 | #8E9DB5 | Medium-light blue-gray |
| cv-navy-400 | #617090 | Mid-tone navy-blue |
| cv-navy-500 | #3D5070 | Medium navy |
| cv-navy-600 | #2A3D5E | Deep navy, one step above anchor |
| cv-navy-700 | #1B2A4A | **ANCHOR** — Midnight Navy, Chatverce primary |
| cv-navy-800 | #111C33 | Dark navy |
| cv-navy-900 | #0A1120 | Very dark navy — dark mode surface |
| cv-navy-950 | #060B15 | Near-black navy — darkest surface |

---

## Teal Scale

Anchored at `cv-teal-500 = #2EC4B6` (Chatverce Teal — the accent/CTA color). Lighter stops become near-white aqua; darker stops become deep teal-green.

| Token | Hex | Notes |
|-------|-----|-------|
| cv-teal-50 | #EDFAF8 | Lightest teal tint — subtle AI glow backgrounds |
| cv-teal-100 | #C8F1EC | Very light aqua |
| cv-teal-200 | #99E5DC | Light teal |
| cv-teal-300 | #61D4C9 | Medium-light teal |
| cv-teal-400 | #42CBB9 | Brighter teal, one step above anchor |
| cv-teal-500 | #2EC4B6 | **ANCHOR** — Chatverce Teal |
| cv-teal-600 | #24A99D | Darker teal — hover/pressed states |
| cv-teal-700 | #1A8880 | Deep teal |
| cv-teal-800 | #116661 | Very deep teal |
| cv-teal-900 | #0A4440 | Near-dark teal |
| cv-teal-950 | #052623 | Darkest teal — dark mode subtle backgrounds |

---

## Gray Scale

Multiple anchors from v1: `50=#FAFBFC`, `100=#F1F3F5`, `200=#E2E8F0`, `400=#94A3B8`, `500=#64748B`, `900=#1A1D23`. Closely aligned with Tailwind CSS Slate scale.

| Token | Hex | Notes |
|-------|-----|-------|
| cv-gray-50 | #FAFBFC | **ANCHOR** — Snow, page background |
| cv-gray-100 | #F1F3F5 | **ANCHOR** — Cloud, card/panel background |
| cv-gray-200 | #E2E8F0 | **ANCHOR** — Frost, default border |
| cv-gray-300 | #CBD5E1 | Medium-light border |
| cv-gray-400 | #94A3B8 | **ANCHOR** — Mist, placeholder/disabled text |
| cv-gray-500 | #64748B | **ANCHOR** — Slate, secondary text |
| cv-gray-600 | #475569 | Medium-dark gray |
| cv-gray-700 | #334155 | Dark gray |
| cv-gray-800 | #1E293B | Very dark gray |
| cv-gray-900 | #1A1D23 | **ANCHOR** — Ink, primary text |
| cv-gray-950 | #0F1117 | Near-black — darkest text/surfaces |

---

## Red Scale

Anchored at `cv-red-500 = #EF4444` (Coral — error/destructive color).

| Token | Hex | Notes |
|-------|-----|-------|
| cv-red-50 | #FEF2F2 | Lightest red tint — error background |
| cv-red-100 | #FEE2E2 | Very light red |
| cv-red-200 | #FECACA | Light red |
| cv-red-300 | #FCA5A5 | Medium-light red |
| cv-red-400 | #F87171 | Lighter red — dark mode error |
| cv-red-500 | #EF4444 | **ANCHOR** — Coral, error/destructive |
| cv-red-600 | #DC2626 | Dark red — error text |
| cv-red-700 | #B91C1C | Deep red |
| cv-red-800 | #991B1B | Very deep red |
| cv-red-900 | #7F1D1D | Near-dark red |
| cv-red-950 | #450A0A | Darkest red — dark mode subtle backgrounds |

---

## Amber Scale

Anchored at `cv-amber-500 = #F59E0B` (Amber — warning color).

| Token | Hex | Notes |
|-------|-----|-------|
| cv-amber-50 | #FFFBEB | Lightest amber tint — warning background |
| cv-amber-100 | #FEF3C7 | Very light amber |
| cv-amber-200 | #FDE68A | Light amber |
| cv-amber-300 | #FCD34D | Medium-light amber |
| cv-amber-400 | #FBBF24 | Brighter amber — dark mode warning |
| cv-amber-500 | #F59E0B | **ANCHOR** — Amber, warning |
| cv-amber-600 | #D97706 | Darker amber |
| cv-amber-700 | #B45309 | Deep amber — warning text |
| cv-amber-800 | #92400E | Very deep amber |
| cv-amber-900 | #78350F | Near-dark amber |
| cv-amber-950 | #451A03 | Darkest amber — dark mode subtle backgrounds |

---

## Green Scale

Anchored at `cv-green-500 = #10B981` (Meadow — success color).

| Token | Hex | Notes |
|-------|-----|-------|
| cv-green-50 | #ECFDF5 | Lightest green tint — success background |
| cv-green-100 | #D1FAE5 | Very light green |
| cv-green-200 | #A7F3D0 | Light green |
| cv-green-300 | #6EE7B7 | Medium-light green |
| cv-green-400 | #34D399 | Brighter green — dark mode success |
| cv-green-500 | #10B981 | **ANCHOR** — Meadow, success |
| cv-green-600 | #059669 | Darker green |
| cv-green-700 | #047857 | Deep green — success text |
| cv-green-800 | #065F46 | Very deep green |
| cv-green-900 | #064E3B | Near-dark green |
| cv-green-950 | #022C22 | Darkest green — dark mode subtle backgrounds |

---

## Channel Colors (standalone, not scaled)

Channel colors identify external platform integrations. They are fixed brand colors from those platforms and do not participate in the primitive scale system.

| Token | Hex | Usage |
|-------|-----|-------|
| cv-channel-whatsapp | #25D366 | WhatsApp indicator only |
| cv-channel-telegram | #26A5E4 | Telegram indicator only |
| cv-channel-webchat | (cv-gray-500) | Webchat indicator — resolves to `#64748B` |
| cv-channel-telephone | (cv-navy-700) | Telephone indicator — resolves to `#1B2A4A` |
| cv-channel-email | (cv-navy-700) | Email indicator — resolves to `#1B2A4A` |

> **Rule:** Channel colors are never used as primary/accent. Chatverce identity is navy + teal.
