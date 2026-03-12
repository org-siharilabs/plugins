---
description: Audit UI code against the Chatverce design system — brand identity, Ive-inspired principles, component patterns, voice & tone, and accessibility
disable-model-invocation: true
context: fork
agent: design-reviewer
argument-hint: [file, directory, or component name]
allowed-tools: Read, Grep, Glob, Bash(find *)
---

# Chatverce Design Review

Review the following target for compliance with the Chatverce design system: $ARGUMENTS

## Step 1: Identify Files
Use Glob and Grep to find all relevant component files, style files, and UI strings in the target path. If $ARGUMENTS is a component name rather than a path, search for it.

## Step 2: Read the Design System
Read the following reference files from this plugin:
- [Design Soul](../skills/design-soul/SKILL.md) — philosophy and principles
- [Brand Identity](../skills/brand-identity/SKILL.md) — color, type, spacing tokens
- [Component Patterns](../skills/component-patterns/SKILL.md) — UI rules
- [Voice & Tone](../skills/voice-and-tone/SKILL.md) — copy guidelines

## Step 3: Evaluate Against Six Dimensions
For each file, check:
1. **Philosophy** — Does the UI pass the Ive Test? Can anything be removed?
2. **Brand identity** — Correct tokens? No rogue hex values? Typography on-scale?
3. **Component patterns** — Buttons follow hierarchy? Cards content-first? Forms single-column?
4. **Voice & tone** — Approved terminology? No jargon? Error messages are solutions?
5. **Accessibility** — Touch targets >= 44px? WCAG AA contrast? Labels on inputs?
6. **Motion** — Approved durations? No decorative animation?

## Step 4: Produce Report
Output a structured report with findings grouped by severity:
- **Must fix** — Violates core principles (jargon, wrong colors, missing a11y)
- **Should fix** — Deviates from patterns (non-standard spacing, inconsistent radius)
- **Consider** — Opportunities to better embody the philosophy

Each finding must include:
- File path and line number
- What's wrong
- Which principle it violates
- The specific fix (code suggestion when possible)
