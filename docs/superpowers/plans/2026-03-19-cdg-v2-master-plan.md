# Chatverce Design Guidelines v2.0 — Master Plan

> **For agentic workers:** This is the master coordination document. Each sub-plan is independently executable. Start with Plan A, then run B/C/D in parallel.

**Goal:** Rebuild the chatverce-design plugin from v1.0 (12 files, light-only, web-first) to CDG v2.0 (~115 files, dark/light, web+mobile, agent-agnostic)

**Spec:** `docs/specs/2026-03-19-chatverce-design-plugin-v2-design.md`

---

## Execution Order

```
Plan A: Core (sequential — must complete first)
  ↓
Plan B: Patterns ──┐
Plan C: Components ├── (parallel — all three can run simultaneously)
Plan D: Enforcement┘
```

## Sub-Plans

| Plan | File | Status | Estimated Tasks |
|------|------|--------|-----------------|
| **A: Core** | `2026-03-19-cdg-v2-plan-a-core.md` | Ready | 18 tasks |
| **B: Patterns** | `2026-03-19-cdg-v2-plan-b-patterns.md` | Pending Plan A | 16 tasks |
| **C: Components** | `2026-03-19-cdg-v2-plan-c-components.md` | Pending Plan A | 20 tasks (batched) |
| **D: Enforcement & Tooling** | `2026-03-19-cdg-v2-plan-d-enforcement.md` | Pending Plan A | 10 tasks |

## What Each Plan Delivers

**Plan A (Core):** Working plugin with v2 directory structure, philosophy layer, complete token system (primitives, semantic light/dark, Tailwind/CSS/RN outputs), and all 10 foundations. After Plan A, the plugin is usable — an AI agent can load it and get design guidance for any Chatverce work, just without component-level or pattern-level detail.

**Plan B (Patterns):** 14 behavioral pattern skills. After Plan B, agents know HOW users accomplish tasks (loading, onboarding, feedback, AI interactions, etc.), not just what components look like.

**Plan C (Components):** 77 component skills (31 Full, 24 Standard, 11 Minimal) + catalog.md. After Plan C, agents have per-component specs with platform considerations, code examples, and accessibility guidance.

**Plan D (Enforcement & Tooling):** 4 enforcement guards, updated design-reviewer agent, updated design-review command, v1 cleanup, marketplace.json update. After Plan D, the plugin actively prevents violations during implementation and can audit existing code.

## Completion Criteria

All four plans complete → CDG v2.0 is production-ready:
- `plugin.json` at version `2.0.0`
- All v1 files removed or migrated
- `marketplace.json` updated
- Final commit on main branch
