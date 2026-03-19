---
description: Audit UI code against the full Chatverce Design Guidelines v2.0 — philosophy, tokens, theme compliance, component patterns, voice & tone, accessibility, motion, and platform appropriateness
disable-model-invocation: true
context: fork
agent: design-reviewer
argument-hint: [file, directory, or component name]
allowed-tools: Read, Grep, Glob, Bash
---

# CDG Design Review

Review the following target for compliance with the Chatverce Design Guidelines v2.0: $ARGUMENTS

## Step 1: Identify Files

Use Glob and Grep to find all relevant files in the target path:
- Component files (`.tsx`, `.jsx`, `.ts`, `.js`)
- Style files (`.css`, `.module.css`, Tailwind class strings)
- Screen or page files
- Any files containing user-facing strings

If `$ARGUMENTS` is a component name rather than a path, search for it across the codebase using Glob.

## Step 2: Read CDG v2.0 References

Before evaluating, confirm your knowledge of the relevant CDG v2.0 foundations. For any dimension where you are uncertain about a rule, read the relevant skill file:

- Philosophy: `skills/philosophy/design-ethos/SKILL.md`
- Token system: `skills/foundations/theme-system/SKILL.md` + `skills/enforcement/token-guard/SKILL.md`
- Theme compliance: `skills/enforcement/theme-guard/SKILL.md`
- Component patterns: `skills/components/catalog.md` + `skills/enforcement/pattern-guard/SKILL.md`
- Voice & tone: `skills/foundations/writing/SKILL.md`
- Accessibility: `skills/foundations/accessibility/SKILL.md` + `skills/enforcement/a11y-guard/SKILL.md`
- Motion: `skills/foundations/motion/SKILL.md`
- Platform patterns: `skills/enforcement/pattern-guard/SKILL.md`

## Step 3: Evaluate 8 Dimensions

For each file identified in Step 1, evaluate against all eight dimensions:

1. **Philosophy** — Ive Test: can anything be removed? Does the UI defer to content? Is the intended emotional register delivered?
2. **Token system** — All `cv-*` tokens used? No hardcoded hex, rgb, z-index, spacing, duration, or easing?
3. **Theme compliance** — Works in both light and dark? No `dark:` prefix in component code? No light-only assumptions?
4. **Component patterns** — One primary CTA? Labels above inputs? Catalog components used? Empty states present? All component states handled?
5. **Voice & tone** — Chatverce terminology throughout? Sentence case? Present tense? Solution-focused error messages?
6. **Accessibility** — Labeled inputs? Alt text on images? 44px touch targets? Keyboard navigation? Screen reader attributes?
7. **Motion** — Approved durations (150/300/500ms)? Reduced-motion handled? No decorative animation? Haptics on key mobile actions?
8. **Platform appropriateness** — Correct patterns for web vs mobile? Safe areas respected? No modals on mobile (use drawers/bottom sheets)?

## Step 4: Produce Report

Output the following structured report:

---

## CDG Design Review: {target}

### Summary

Brief overall assessment (2–4 sentences). State the target, the general quality level, and the most critical issue category.

### Findings

#### Must Fix (violates core)

Violations of foundational CDG v2.0 rules — wrong token usage, technical jargon in user-facing strings, missing accessibility labels, broken theme behavior, prohibited animation patterns. These block shipping.

For each finding:
- **File:** `path/to/file.tsx` (line N if identifiable)
- **Violation:** What is wrong and which rule it breaks
- **Fix:** The specific correction (corrected code or copy where possible)

#### Should Fix (deviates from patterns)

Deviations from CDG v2.0 patterns — non-catalog component, inconsistent spacing, component missing a state, non-standard motion timing. These should be addressed before the next release.

For each finding:
- **File:** `path/to/file.tsx` (line N if identifiable)
- **Deviation:** What deviates and from which pattern
- **Fix:** The recommended correction

#### Consider (philosophy opportunities)

Opportunities to more fully embody the CDG v2.0 philosophy — screens that work correctly but could better express the intended emotional register, breathing room improvements, places where caring could be more visible.

For each suggestion:
- **File:** `path/to/file.tsx`
- **Opportunity:** What could be elevated and why
- **Suggestion:** The specific direction

### Platform Check

State whether this is web, mobile (React Native), or both. For each platform represented, confirm:
- Safe areas handled (mobile)
- Haptics on key actions (mobile)
- Keyboard-avoid scroll behavior (mobile)
- Browser back/forward support (web)
- Responsive behavior at key breakpoints (web)

### Theme Check

Confirm whether the implementation works correctly in both themes. Identify any elements that were only verified in one theme. Call out any `dark:` prefix usage in component code.

---

Each finding must be actionable. If you cannot state the specific fix, the finding is incomplete.
