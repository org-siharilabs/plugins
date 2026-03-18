# CDG v2.0 — Plan D: Enforcement, Tooling & Cleanup

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Write all 4 enforcement guards, update the design-reviewer agent and design-review command, clean up v1 files, and finalize the plugin for production.

**Architecture:** Enforcement skills are instructional — they teach AI agents what to check during implementation. The design-reviewer agent is enhanced to know all CDG v2 layers. V1 files are removed after all other plans complete.

**Tech Stack:** Markdown (SKILL.md, agent, command files), JSON (plugin.json, marketplace.json)

**Spec:** `docs/specs/2026-03-19-chatverce-design-plugin-v2-design.md` — Part 5: Enforcement (lines 810–900)

**Depends on:** Plan A (foundations for guard references). Plans B and C should ideally be complete before the design-reviewer update, but enforcement guards can be written after Plan A.

**Working directory:** `/Users/sandeep/Desktop/dev/org-siharilabs/plugins`

---

## File Map

```
chatverce-design/
  skills/
    enforcement/
      token-guard/SKILL.md              # Task 1
      theme-guard/SKILL.md              # Task 2
      a11y-guard/SKILL.md               # Task 3
      pattern-guard/SKILL.md            # Task 4

  agents/
    design-reviewer.md                  # Task 5 (rewrite)

  commands/
    design-review.md                    # Task 6 (rewrite)

  # V1 files to remove (Task 7)
  skills/
    design-soul/SKILL.md                # DELETE
    design-soul/ive-philosophy.md       # DELETE (copied to v2 in Plan A)
    brand-identity/SKILL.md             # DELETE
    brand-identity/tokens.md            # DELETE
    component-patterns/SKILL.md         # DELETE
    component-patterns/examples.md      # DELETE
    voice-and-tone/SKILL.md             # DELETE

  README.md                             # Task 8 (rewrite)

# Root marketplace
.claude-plugin/marketplace.json         # Task 9 (update version)
```

---

## Tasks

### Task 1: Write token-guard Enforcement

**Files:** Create `chatverce-design/skills/enforcement/token-guard/SKILL.md`

**Reference:** Spec enforcement section — token-guard (lines 815–836)

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: token-guard
description: Enforcement guard that catches hardcoded values bypassing the Chatverce token system during implementation
user_invocable: false
---
```

**Required content:**

1. **Purpose:** Prevent hardcoded values that bypass the 3-layer token system. This guard activates when an AI agent is writing or editing UI code in a Chatverce project.

2. **What to Check** — Full violation table from spec:

| Violation | Pattern to Detect | Fix |
|-----------|-------------------|-----|
| Raw hex color | `bg-[#...]`, `color: #...`, `style={{backgroundColor: '#...'}}` | Use `bg-cv-*` or `text-cv-*` semantic tokens |
| Raw RGB/HSL | `rgb(...)`, `hsl(...)` | Use semantic token |
| Arbitrary z-index | `z-[999]`, `zIndex: 50` | Use `z-cv-*` scale |
| Arbitrary font | `font-['Helvetica']` | Use `font-cv-sans` or `font-cv-mono` |
| Arbitrary shadow | `shadow-[0_4px...]` | Use `shadow-cv-*` |
| Arbitrary radius | `rounded-[10px]` | Use `rounded-cv-*` |
| Arbitrary spacing | `p-[13px]`, `padding: 13` | Use nearest scale value |
| Arbitrary duration | `duration-[200ms]` | Use `duration-cv-*` |
| Arbitrary easing | `ease-[cubic-bezier(...)]` | Use `ease-cv-*` |
| RN inline color | `style={{backgroundColor: '#fff'}}` | NativeWind class `bg-cv-surface` |

3. **Exceptions** — When NOT to flag:
   - Primitive definition files (`primitives.md`, `tokens-*.md`)
   - Third-party library overrides with explicit `/* cv-override */` comment
   - One-off values with explicit `/* cv-exception: reason */` comment
   - SVG path data, image URLs, or non-style hex values
   - Tailwind arbitrary values that ARE in the spacing scale (e.g., `p-4` is fine)

4. **Self-correction:** When a violation is detected, the agent should correct it before saving. Include the lookup table: which semantic token replaces which hardcoded value.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/enforcement/token-guard/SKILL.md
git commit -m "feat(cdg): add token-guard enforcement — catches hardcoded values"
```

---

### Task 2: Write theme-guard Enforcement

**Files:** Create `chatverce-design/skills/enforcement/theme-guard/SKILL.md`

**Reference:** Spec enforcement section — theme-guard (lines 838–849)

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: theme-guard
description: Enforcement guard that ensures all UI works correctly in both light and dark themes
user_invocable: false
---
```

**Required content:**

1. **Purpose:** Ensure every piece of UI renders correctly in both light and dark themes. If semantic tokens are used correctly, this guard rarely fires — it's the safety net.

2. **What to Check:**

| Violation | Example | Fix |
|-----------|---------|-----|
| Light-only background | `bg-white` hardcoded | `bg-cv-surface` |
| Dark-only assumption | `text-white` on dynamic surface | `text-cv-text-primary` |
| Missing dark adaptation | `bg-gray-50` without `dark:` | Use semantic token |
| Opacity breaking in dark | `bg-black/5` | `bg-cv-bg-accent-subtle` |
| Shadow invisible in dark | Raw rgba shadow | `shadow-cv-*` |
| Low contrast in one theme | Passes light, fails dark | Reference contrast-matrix |
| StatusBar mismatch (mobile) | Light bar on light bg | Theme-aware StatusBar |
| OLED pure black (mobile) | `#000000` backgrounds | `cv-navy-950` (warm black) |
| Hardcoded `dark:` overrides | `dark:bg-gray-900` | Semantic tokens handle this |

3. **The Rule:** If you're using `dark:` Tailwind prefix, you're probably doing it wrong. Semantic tokens automatically adapt. The `dark:` prefix is only needed for the CSS custom property definitions, never in component code.

4. **Testing instruction:** For every UI element written, mentally swap to the other theme. Does it still work? Can you still read all text? Are borders visible?

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/enforcement/theme-guard/SKILL.md
git commit -m "feat(cdg): add theme-guard enforcement — ensures both themes work"
```

---

### Task 3: Write a11y-guard Enforcement

**Files:** Create `chatverce-design/skills/enforcement/a11y-guard/SKILL.md`

**Reference:** Spec enforcement section — a11y-guard (lines 851–870)

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: a11y-guard
description: Enforcement guard that catches accessibility violations during implementation — covers web (ARIA) and mobile (VoiceOver/TalkBack)
user_invocable: false
---
```

**Required content:**

1. **Purpose:** Enforce accessibility as a baseline. Every interactive element must be keyboard-navigable (web), screen-reader-accessible (both), and touch-target-compliant (mobile).

2. **What to Check — Web:**

| Violation | Detect | Fix |
|-----------|--------|-----|
| Missing input label | `<input>` / `<Input>` without `<label>` or `aria-label` | Add `<Label>` above input |
| Image without alt | `<img>` / `<Image>` without `alt` | Add descriptive `alt` |
| Button without name | `<button>` with only icon child | Add `aria-label` |
| Touch target < 44px | Interactive element smaller than `w-11 h-11` | Add padding or min-size |
| Color-only meaning | Status conveyed by color alone | Add icon + text |
| Missing focus-visible | Custom component without focus ring | Add `focus-visible:ring-2 ring-cv-border-accent` |
| Missing keyboard nav | Custom widget without key handlers | Follow WAI-ARIA authoring practices |
| No skip-link | Page layout without skip navigation | Add `skip-link` component |
| Missing reduced-motion | Animation without `motion-safe:` | Prefix with `motion-safe:` |
| Heading hierarchy skip | `<h1>` followed by `<h3>` | Use sequential levels |

3. **What to Check — Mobile:**

| Violation | Detect | Fix |
|-----------|--------|-----|
| Missing accessible role | `TouchableOpacity` without `accessibilityRole` | Add `accessibilityRole="button"` |
| Missing label | Interactive element without `accessibilityLabel` | Add descriptive label |
| Missing live region | Dynamic content without announcement | Add `accessibilityLiveRegion="polite"` |
| Touch target < 44pt (iOS) / 48dp (Android) | Small touchable | Increase hit area |
| Missing accessibility state | Toggle without `accessibilityState` | Add `{checked: true/false}` |
| Image without label | `<Image>` without `accessible` / `accessibilityLabel` | Add label or mark decorative |

4. **Reference:** Point to `foundations/accessibility/SKILL.md` for full patterns.

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/enforcement/a11y-guard/SKILL.md
git commit -m "feat(cdg): add a11y-guard enforcement — web ARIA + mobile VoiceOver/TalkBack"
```

---

### Task 4: Write pattern-guard Enforcement

**Files:** Create `chatverce-design/skills/enforcement/pattern-guard/SKILL.md`

**Reference:** Spec enforcement section — pattern-guard (lines 872–885)

- [ ] **Step 1: Write SKILL.md**

```markdown
---
name: pattern-guard
description: Enforcement guard that catches Chatverce convention violations — terminology, component patterns, platform-appropriate choices
user_invocable: false
---
```

**Required content:**

1. **Purpose:** Enforce Chatverce-specific conventions: approved terminology, component patterns, and platform-appropriate UI choices.

2. **What to Check:**

| Violation | Detect | Fix |
|-----------|--------|-----|
| Multiple primary buttons | Two `variant="default"` buttons in same view | One primary; others secondary/ghost |
| Floating/inside labels | `placeholder` used as only label | Label above, always visible |
| Technical jargon | "Deploy", "Agent", "Node", "Webhook", "API key", "Workflow", "Endpoint", "Latency", "Uptime" in UI strings | Reference terminology table in `foundations/writing/` |
| Raw error message | `{error.message}` or `{err.toString()}` rendered | Solution-focused copy |
| Decorative animation | `animate-bounce`, `animate-spin` (except loading) | Motion serves comprehension only |
| Non-catalog component | Custom component when catalog equivalent exists | Use catalog component |
| Missing empty state | List/table with no empty handling | Add empty state per component spec |
| Modal on mobile | Centered overlay modal on phone screen | Use Drawer (bottom sheet) instead |
| No haptic on key action | Primary/destructive button without haptic (mobile) | Add `expo-haptics` feedback |
| Ignoring safe areas | Content behind notch/home indicator | Use `SafeAreaView` |
| Ignoring platform conventions | iOS-style back arrow on Android | Use platform-appropriate navigation |

3. **Terminology Quick Reference** — Include the full mapping from `foundations/writing/` so the guard is self-contained:

| Never Say | Always Say |
|-----------|-----------|
| Deploy | Go live |
| Agent | Assistant |
| Node | Step |
| Trigger | "When this happens" |
| Action | "Do this" |
| Conditional | "Check if" |
| Webhook | Connection |
| API key | Secret key |
| Workflow | Flow |
| Endpoint | *(never show)* |

- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/enforcement/pattern-guard/SKILL.md
git commit -m "feat(cdg): add pattern-guard enforcement — terminology, conventions, platform checks"
```

---

### Task 5: Rewrite Design Reviewer Agent

**Files:** Rewrite `chatverce-design/agents/design-reviewer.md`

- [ ] **Step 1: Read current file**

```bash
cat chatverce-design/agents/design-reviewer.md
```

- [ ] **Step 2: Rewrite design-reviewer.md**

Update to know about all CDG v2 layers:

```markdown
---
name: design-reviewer
description: Reviews Chatverce UI code against the full CDG v2.0 design system — philosophy, foundations, patterns, components, and enforcement
model: sonnet
tools:
  - Read
  - Grep
  - Glob
  - Bash
---
```

**Quick reference section** must include:
- Six design ethos principles (from philosophy)
- Token rule (3-layer, semantic only in code)
- All 10 foundation areas with key rules
- Component catalog awareness (knows to check catalog.md)
- Pattern awareness (knows 14 patterns exist)
- All 4 enforcement guard checks
- Platform parity check (web AND mobile considered?)
- Terminology table (full mapping)

**Review dimensions** (updated from v1's 6 to 8):
1. Philosophy (Ive Test pass? Emotional intention considered?)
2. Token system (semantic tokens used? No hardcoded values?)
3. Theme compliance (works in both light/dark?)
4. Component patterns (catalog components used correctly?)
5. Voice & tone (approved terminology? Solution-focused errors?)
6. Accessibility (WCAG AA contrast, labels, touch targets, keyboard, ARIA?)
7. Motion (approved durations? Reduced-motion respected?)
8. Platform appropriateness (correct patterns for web vs mobile?)

- [ ] **Step 3: Commit**

```bash
git add chatverce-design/agents/design-reviewer.md
git commit -m "feat(cdg): rewrite design-reviewer agent for CDG v2.0 — all layers covered"
```

---

### Task 6: Rewrite Design Review Command

**Files:** Rewrite `chatverce-design/commands/design-review.md`

- [ ] **Step 1: Read current file**

```bash
cat chatverce-design/commands/design-review.md
```

- [ ] **Step 2: Rewrite design-review.md**

Update invocation, process, and report format:

**Invocation examples:**
```
/chatverce-design:design-review src/components/inbox
/chatverce-design:design-review mobile/screens/Dashboard
/chatverce-design:design-review ThreadCard
```

**Process:**
1. Identify files with Glob/Grep in target path
2. Read CDG v2 reference files (philosophy, relevant foundations, relevant components, relevant patterns)
3. Evaluate against 8 dimensions (from Task 5)
4. Produce structured report

**Report format:**
```
## CDG Design Review: {target}

### Summary
{1-2 sentence overall assessment}

### Findings

#### Must Fix (violates core principles)
- {file:line} — {what's wrong} — {which principle} — {specific fix}

#### Should Fix (deviates from patterns)
- {file:line} — {what's wrong} — {which pattern} — {specific fix}

#### Consider (opportunities to better embody philosophy)
- {file:line} — {suggestion} — {which ethos principle}

### Platform Check
- Web: {pass/issues}
- Mobile: {pass/issues}

### Theme Check
- Light: {pass/issues}
- Dark: {pass/issues}
```

- [ ] **Step 3: Commit**

```bash
git add chatverce-design/commands/design-review.md
git commit -m "feat(cdg): rewrite design-review command for CDG v2.0 — 8 review dimensions"
```

---

### Task 7: Remove v1 Files

**IMPORTANT:** Only run this task after Plans A, B, and C are confirmed complete.

**Files to delete:**

- [ ] **Step 1: Verify v2 replacements exist**

```bash
# Check that all v2 files exist before deleting v1
test -f chatverce-design/skills/philosophy/design-ethos/SKILL.md && echo "philosophy: OK"
test -f chatverce-design/skills/philosophy/design-ethos/ive-philosophy.md && echo "ive-ref: OK"
test -f chatverce-design/skills/foundations/theme-system/SKILL.md && echo "theme: OK"
test -f chatverce-design/skills/foundations/writing/SKILL.md && echo "writing: OK"
test -f chatverce-design/skills/components/catalog.md && echo "catalog: OK"
test -f chatverce-design/skills/enforcement/token-guard/SKILL.md && echo "enforcement: OK"
```

Expected: All "OK".

- [ ] **Step 2: Remove v1 skill directories**

```bash
rm -rf chatverce-design/skills/design-soul
rm -rf chatverce-design/skills/brand-identity
rm -rf chatverce-design/skills/component-patterns
rm -rf chatverce-design/skills/voice-and-tone
```

- [ ] **Step 3: Verify no v1 remnants**

```bash
ls chatverce-design/skills/
```

Expected: `philosophy/`, `foundations/`, `patterns/`, `components/`, `enforcement/` — no v1 directories.

- [ ] **Step 4: Commit**

```bash
git add -A chatverce-design/skills/
git commit -m "chore(cdg): remove v1 skill files — fully replaced by CDG v2.0"
```

---

### Task 8: Rewrite Plugin README

**Files:** Rewrite `chatverce-design/README.md`

- [ ] **Step 1: Read current README**

```bash
cat chatverce-design/README.md
```

- [ ] **Step 2: Rewrite README.md**

Update to reflect CDG v2.0 structure:

1. **What It Is** — Chatverce Design Guidelines (CDG) — agent-agnostic design system for web + mobile.
2. **Structure** — Philosophy → Foundations → Patterns → Components → Enforcement.
3. **Quick Start** — Installation commands (same as v1).
4. **For AI Agents** — How guidelines load and activate (the activation model from spec).
5. **For Developers** — How to use the design-review command.
6. **Design Philosophy Summary** — Brief of the 6 ethos principles.
7. **Token System** — Overview of 3-layer architecture.
8. **Brand Summary** — Navy + Teal identity, Inter/JetBrains Mono, 4px spacing.

- [ ] **Step 3: Commit**

```bash
git add chatverce-design/README.md
git commit -m "docs(cdg): rewrite plugin README for CDG v2.0"
```

---

### Task 9: Update Marketplace

**Files:** Modify `.claude-plugin/marketplace.json`

- [ ] **Step 1: Read current file**

```bash
cat .claude-plugin/marketplace.json
```

- [ ] **Step 2: Update marketplace.json**

Bump version to `2.0.0`, update description:

```json
{
  "name": "siharilabs",
  "owner": {
    "name": "Sihari Labs",
    "email": "admin@siharilabs.com"
  },
  "metadata": {
    "description": "Claude Code plugins by Sihari Labs — design systems, development tools, and AI workflows",
    "version": "2.0.0"
  },
  "plugins": [
    {
      "name": "chatverce-design",
      "source": "./chatverce-design",
      "description": "Chatverce Design Guidelines (CDG) — agent-agnostic design system for building Chatverce across web and mobile. Philosophy, foundations, patterns, 77 components, and enforcement.",
      "version": "2.0.0",
      "author": {
        "name": "Sihari Labs",
        "email": "admin@siharilabs.com"
      },
      "repository": "https://github.com/org-siharilabs/plugins",
      "license": "Proprietary",
      "keywords": ["design-system", "design-guidelines", "chatverce", "ui", "ux", "dark-mode", "accessibility", "mobile", "web"],
      "category": "design"
    }
  ]
}
```

- [ ] **Step 3: Commit**

```bash
git add .claude-plugin/marketplace.json
git commit -m "chore(cdg): bump marketplace to v2.0.0"
```

---

### Task 10: Final Validation

- [ ] **Step 1: Count all files**

```bash
find chatverce-design/skills -name "*.md" | wc -l
```

Expected: ~115 files (2 philosophy + ~21 foundations + 14 patterns + 78 components + 4 enforcement = ~119)

- [ ] **Step 2: Verify no v1 remnants**

```bash
ls chatverce-design/skills/ | sort
```

Expected: `components  enforcement  foundations  patterns  philosophy`

- [ ] **Step 3: Verify plugin.json version**

```bash
grep version chatverce-design/.claude-plugin/plugin.json
```

Expected: `"version": "2.0.0"`

- [ ] **Step 4: Verify marketplace version**

```bash
grep version .claude-plugin/marketplace.json
```

Expected: `"version": "2.0.0"` (appears twice — metadata and plugin)

- [ ] **Step 5: Run git status**

```bash
git status
```

Expected: clean working tree.

- [ ] **Step 6: Final commit (if needed)**

```bash
git log --oneline -20
```

Review all commits for the CDG v2.0 work.

---

## Plan D Complete Checklist

- [ ] 4 enforcement guard SKILL.md files
- [ ] design-reviewer agent rewritten for v2
- [ ] design-review command rewritten for v2
- [ ] All v1 files removed
- [ ] README rewritten
- [ ] marketplace.json at v2.0.0
- [ ] plugin.json at v2.0.0
- [ ] No v1 remnants in skills directory
- [ ] Total file count ~115
- [ ] Git clean
