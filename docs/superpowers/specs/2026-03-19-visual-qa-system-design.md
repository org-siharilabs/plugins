# Visual QA System — Design Spec

## Goal

A 3-agent pipeline that uses Playwright to screenshot Chatverce web pages across viewports and themes, reviews them against CDG v2.0 with Steve Jobs-level scrutiny, and files structured GitHub issues on the Chatverce project board.

## Architecture

Three agents orchestrated by a `/visual-qa` command:

1. **visual-qa-crawler** — navigates, authenticates, screenshots
2. **steve-jobs-reviewer** — reviews screenshots against CDG, annotates problems
3. **qa-issue-reporter** — deduplicates findings, creates GitHub issues

```
/visual-qa [--url <base>] <route> [<route> ...]
         │
         ▼
┌─────────────────────┐
│  visual-qa command   │  orchestrator
│  resolve base URL    │
│  manage dev server   │
└────────┬────────────┘
         ▼
┌─────────────────────┐
│  visual-qa-crawler   │  agent 1
│  auth → screenshot   │
│  3 viewports × 2     │
│  themes per route    │
└────────┬────────────┘
         ▼
┌─────────────────────┐
│  steve-jobs-reviewer │  agent 2
│  read screenshots    │
│  + CDG guidelines    │
│  → findings.json     │
│  → annotated images  │
└────────┬────────────┘
         ▼
┌─────────────────────┐
│  qa-issue-reporter   │  agent 3
│  deduplicate         │
│  → GitHub epics      │
│  → child issues      │
│  → project board     │
└─────────────────────┘
```

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Browser automation | Playwright MCP (already available in Claude Code) |
| Screenshot storage | `/tmp/visual-qa/` (ephemeral, per-run) |
| Design guidelines | CDG v2.0 skills (design-ethos, design-foundations, design-components, design-patterns, design-enforcement) |
| GitHub integration | `gh` CLI (issues, labels, projects) |
| Target repo | `org-siharilabs/chatverce` |
| Project board | "Chatverce" GitHub Project |

---

## Command: `/visual-qa`

### Invocation

```bash
# Local dev server (default)
/visual-qa /auth /dashboard /inbox

# Remote/staging server
/visual-qa --url https://staging.chatverce.com /auth /dashboard /inbox
```

### Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `routes` | Yes | — | Space-separated list of routes to test (e.g., `/auth`, `/dashboard`) |
| `--url` | No | `http://localhost:3000` | Base URL of the running app |

### Orchestration Flow

1. **Resolve base URL:**
   - If `--url` provided: use it directly, skip dev server management
   - If no `--url`: check `http://localhost:3000`
     - If running: use it
     - If not running: find a free port, start dev server (`npm run dev -- --port <port>`), record PID for cleanup
2. **Dispatch visual-qa-crawler** with base URL + route list
3. **Dispatch steve-jobs-reviewer** with screenshot manifest + CDG skill paths
4. **Dispatch qa-issue-reporter** with findings JSON
5. **Cleanup:** if we started a dev server, kill it by PID
6. **Print terminal summary** — findings grouped by severity with counts

---

## Agent 1: `visual-qa-crawler`

### Purpose

Navigate the Chatverce web app, authenticate, and capture systematic screenshots of every requested route across viewports and themes.

### Tools

- Playwright MCP: `browser_navigate`, `browser_fill_form`, `browser_click`, `browser_snapshot`, `browser_take_screenshot`, `browser_resize`, `browser_evaluate`, `browser_press_key`, `browser_wait_for`
- Bash (for filesystem operations)

### Auth Flow

1. Navigate to `<base-url>/auth` (or login page)
2. Enter email: `admin@siharilabs.com`
3. Submit
4. Enter OTP: `123456`
5. Submit
6. Wait for redirect to authenticated state
7. Verify auth succeeded (check for dashboard or authenticated indicator)

### Screenshot Matrix

For each route, capture:

| Viewport | Width × Height | Name |
|----------|---------------|------|
| Desktop | 1440 × 900 | `desktop` |
| Tablet | 768 × 1024 | `tablet` |
| Mobile | 375 × 812 | `mobile` |

| Theme | Method | Name |
|-------|--------|------|
| Light | `prefers-color-scheme: light` via `browser_evaluate` | `light` |
| Dark | `prefers-color-scheme: dark` via `browser_evaluate` | `dark` |

Total per route: **6 screenshots** (3 viewports × 2 themes).

### State Capture

For each route, if the page has distinct states, attempt to capture:
- **Populated state** (default — page with data)
- **Empty state** (if detectable — e.g., clear filters, empty list)
- **Loading state** (if capturable — throttle network)

State capture is best-effort. The populated state is always captured. Empty and loading states are captured when the crawler can trigger them.

### Output

Directory structure:
```
/tmp/visual-qa/
├── manifest.json
├── auth/
│   ├── desktop-light.png
│   ├── desktop-dark.png
│   ├── tablet-light.png
│   ├── tablet-dark.png
│   ├── mobile-light.png
│   └── mobile-dark.png
├── dashboard/
│   ├── desktop-light.png
│   ├── ...
│   └── mobile-dark.png
└── inbox/
    └── ...
```

`manifest.json`:
```json
{
  "baseUrl": "http://localhost:3000",
  "timestamp": "2026-03-19T12:00:00Z",
  "routes": [
    {
      "path": "/auth",
      "screenshots": [
        {
          "file": "auth/desktop-light.png",
          "viewport": "desktop",
          "width": 1440,
          "height": 900,
          "theme": "light",
          "state": "populated"
        }
      ]
    }
  ]
}
```

---

## Agent 2: `steve-jobs-reviewer`

### Purpose

The ruthless design critic. Reviews every screenshot against CDG v2.0 across all 8 dimensions. Annotates problem areas on screenshots. Outputs structured findings.

### Persona

Steve Jobs reviewing a product demo. Standards:
- "If it doesn't feel inevitable, it's wrong."
- "This is not good enough. I know what good looks like, and this isn't it."
- Zero tolerance for sloppiness, misalignment, inconsistent spacing, or "close enough"
- Every pixel matters. Every interaction matters. Every empty state matters.
- If something is mediocre, say so directly. No softening.

### Tools

- Read (CDG skill files and reference material)
- Playwright MCP: `browser_take_screenshot` (for annotating — navigating to screenshot, overlaying red boxes)
- Bash (for file operations, image manipulation if needed)

### Input

- `manifest.json` from crawler
- Screenshot files
- CDG skill paths:
  - `skills/design-ethos/SKILL.md` — philosophy
  - `skills/design-foundations/SKILL.md` — tokens, color, typography, spacing, motion, a11y
  - `skills/design-components/SKILL.md` — component catalog
  - `skills/design-patterns/SKILL.md` — behavioral patterns
  - `skills/design-enforcement/SKILL.md` — guards (token, theme, a11y, pattern)

### Review Process

For each screenshot:

1. **Look at the screenshot** — form an immediate gut reaction. Does this feel right? Does it feel like a product Steve Jobs would ship?

2. **Evaluate all 8 dimensions:**

   | # | Dimension | What to check |
   |---|-----------|---------------|
   | 1 | Philosophy | Ive Test (5 questions). Can anything be removed? Does it defer to user content? Does it feel inevitable? Emotional register? |
   | 2 | Token compliance | Are colors consistent with CDG palette? Spacing follows 4px grid? Typography uses Inter/JetBrains Mono? Correct radius values? |
   | 3 | Theme compliance | Does dark mode look intentional (not inverted)? Sufficient contrast in both themes? Consistent hierarchy? |
   | 4 | Component patterns | Standard components used correctly? One primary CTA? Labels above inputs? Empty states present? |
   | 5 | Voice & tone | Chatverce terminology? Sentence case? No jargon? Solution-focused error messages? |
   | 6 | Accessibility | Sufficient contrast (visual check)? Touch targets look >= 44px on mobile? Text readable at all viewports? |
   | 7 | Motion | N/A for static screenshots (note: recommend manual motion review in findings if interactive elements detected) |
   | 8 | Platform / viewport | Does mobile feel intentional or just squished desktop? Tablet uses space well? Cross-viewport consistency? |

3. **Cross-viewport comparison** — Compare desktop, tablet, and mobile for the same route+theme. Flag inconsistencies, elements that disappear incorrectly, or responsive breakdowns.

4. **Cross-theme comparison** — Compare light and dark for the same route+viewport. Flag contrast issues, elements that become invisible, or inconsistent hierarchy.

### Severity Levels

| Level | Name | Meaning | Example |
|-------|------|---------|---------|
| P0 | Ship-blocker | Cannot go to production | Text invisible on dark mode, button unreachable on mobile, broken layout |
| P1 | Must fix | Serious quality issue | Inconsistent spacing, wrong font, missing empty state, low contrast |
| P2 | Should fix | Noticeable quality gap | Slightly off alignment, could use more breathing room, minor hierarchy issue |
| P3 | Consider | Polish opportunity | Micro-interaction suggestion, subtle spacing tweak, optional enhancement |

### Annotation

For P0 and P1 findings, the reviewer annotates the screenshot:
- Draw a red rectangle around the problem area
- Add a numbered label corresponding to the finding
- Save as `<route>/<viewport>-<theme>-annotated.png`

Annotation method: Use Playwright to open the screenshot in a browser page, overlay red rectangles via JavaScript canvas/CSS, then re-screenshot.

### Output

`findings.json`:
```json
{
  "reviewTimestamp": "2026-03-19T12:05:00Z",
  "summary": {
    "totalFindings": 15,
    "P0": 2,
    "P1": 5,
    "P2": 6,
    "P3": 2
  },
  "findings": [
    {
      "id": "F001",
      "route": "/dashboard",
      "viewport": "mobile",
      "theme": "dark",
      "severity": "P0",
      "dimension": "theme-compliance",
      "title": "Stat card labels invisible on dark mode",
      "description": "The stat card labels use text-gray-400 which has a contrast ratio of approximately 2.1:1 against the dark surface. CDG requires minimum 4.5:1 (WCAG AA). The labels are effectively unreadable.",
      "cdgReference": "design-enforcement/token-guard.md, design-foundations/theme-system.md",
      "screenshot": "dashboard/mobile-dark.png",
      "annotatedScreenshot": "dashboard/mobile-dark-annotated.png",
      "fix": "Replace text-gray-400 with text-cv-content-secondary which resolves correctly in both themes."
    }
  ]
}
```

---

## Agent 3: `qa-issue-reporter`

### Purpose

Transform review findings into well-structured GitHub issues on the Chatverce project board.

### Tools

- Bash (`gh` CLI for GitHub operations)
- Read (findings.json, annotated screenshots)

### Input

- `findings.json` from reviewer
- Annotated screenshot files

### Deduplication

Before creating issues, deduplicate findings that describe the same underlying problem across different viewports or themes:

- Same problem on desktop+tablet+mobile → one issue mentioning all 3 viewports
- Same problem in light+dark → one issue noting both themes
- Group by: route + dimension + title similarity

### Label Setup

On first run, ensure these labels exist on `org-siharilabs/chatverce`:

| Label | Color | Description |
|-------|-------|-------------|
| `visual-qa` | `#6f42c1` (purple) | Created by Visual QA system |
| `P0-ship-blocker` | `#d73a49` (red) | Ship-blocking issue |
| `P1-must-fix` | `#e36209` (orange) | Must fix before release |
| `P2-should-fix` | `#fbca04` (yellow) | Should fix, noticeable quality gap |
| `P3-consider` | `#0e8a16` (green) | Polish opportunity |
| `design-system` | `#1d76db` (blue) | CDG compliance issue |
| `viewport:desktop` | `#bfdadc` | Affects desktop |
| `viewport:tablet` | `#bfdadc` | Affects tablet |
| `viewport:mobile` | `#bfdadc` | Affects mobile |
| `theme:light` | `#fef3cd` | Affects light theme |
| `theme:dark` | `#2b3137` | Affects dark theme |

### Epic Creation

One Epic issue per tested route:

**Title:** `[Visual QA] /<route> — <N> findings (<P0 count> blockers)`

**Body:**
```markdown
## Visual QA Report: /<route>

**Run:** 2026-03-19 12:05 UTC
**Base URL:** http://localhost:3000
**Viewports:** Desktop (1440), Tablet (768), Mobile (375)
**Themes:** Light, Dark

### Summary

| Severity | Count |
|----------|-------|
| P0 Ship-blocker | 2 |
| P1 Must fix | 3 |
| P2 Should fix | 4 |
| P3 Consider | 1 |

### Screenshots

[Desktop Light](screenshot-url) | [Desktop Dark](screenshot-url)
[Tablet Light](screenshot-url) | [Tablet Dark](screenshot-url)
[Mobile Light](screenshot-url) | [Mobile Dark](screenshot-url)

### Findings

- [ ] #123 P0: Stat card labels invisible on dark mode
- [ ] #124 P1: Inconsistent spacing in header
- [ ] #125 P2: Could use more breathing room between sections
...
```

### Child Issue Creation

One issue per deduplicated finding:

**Title:** `[P<n>] <finding title>`

**Body:**
```markdown
## <finding title>

**Route:** /<route>
**Severity:** P<n> — <severity name>
**Dimension:** <dimension>
**Viewports:** Desktop, Mobile
**Theme:** Dark

### Problem

<description from findings.json>

### CDG Reference

<cdgReference from findings.json>

### Annotated Screenshot

![annotated](<uploaded screenshot URL>)

### Fix

<fix from findings.json>

---
*Filed by [Chatverce Visual QA](https://github.com/org-siharilabs/plugins) — CDG v2.1.0*
```

**Labels:** `visual-qa`, `P<n>-<severity>`, `design-system`, applicable viewport labels, applicable theme labels

### Project Board Integration

After creating issues:
1. Add all issues to the "Chatverce" GitHub Project
2. Set status field to "To Do"
3. Set priority field if the project has one (map P0→Urgent, P1→High, P2→Medium, P3→Low)

### Terminal Summary

After all issues are created, print a summary:

```
Visual QA Complete — /dashboard /inbox /auth

  P0 Ship-blockers:  2  (must fix before ship)
  P1 Must fix:       5
  P2 Should fix:     6
  P3 Consider:       2
  ─────────────────────
  Total findings:   15
  GitHub issues:    12  (3 deduplicated)
  Epics created:     3

  Epic: [Visual QA] /dashboard — 7 findings (1 blocker)
        https://github.com/org-siharilabs/chatverce/issues/100

  Epic: [Visual QA] /inbox — 5 findings (1 blocker)
        https://github.com/org-siharilabs/chatverce/issues/101

  Epic: [Visual QA] /auth — 3 findings (0 blockers)
        https://github.com/org-siharilabs/chatverce/issues/102
```

---

## File Placement in Plugin

All new files live inside the `chatverce-design` plugin:

```
chatverce-design/
├── agents/
│   ├── design-reviewer.md          (existing — code review)
│   ├── visual-qa-crawler.md        (new)
│   ├── steve-jobs-reviewer.md      (new)
│   └── qa-issue-reporter.md        (new)
├── commands/
│   ├── design-review.md            (existing)
│   └── visual-qa.md                (new — orchestrator command)
├── skills/
│   └── (existing CDG skills, unchanged)
└── .claude-plugin/
    └── plugin.json                 (bump version)
```

---

## Constraints and Limitations

1. **Screenshots are visual only** — the steve-jobs-reviewer analyzes rendered screenshots, not source code. For code-level violations (hardcoded tokens, missing ARIA attributes), use the existing `design-reviewer` agent separately.

2. **Auth credentials are hardcoded** — `admin@siharilabs.com` / OTP `123456`. These must work on both localhost and staging.

3. **State capture is best-effort** — empty states and loading states are only captured if the crawler can trigger them through UI interaction. Some states may require specific data conditions.

4. **Annotation quality** — red rectangle overlays are approximate. They highlight the general area, not pixel-perfect boundaries.

5. **GitHub rate limits** — for large runs (many routes, many findings), the reporter may hit GitHub API rate limits. The agent should handle this with retries and backoff.

6. **No video/motion review** — static screenshots only. Motion violations (animation duration, easing) cannot be detected. The reviewer should note when interactive elements are visible and recommend manual motion review.

---

## Success Criteria

1. Running `/visual-qa /dashboard` produces 6 screenshots (3 viewports × 2 themes)
2. The steve-jobs-reviewer identifies at least the same class of issues as the existing design-reviewer code audit
3. GitHub issues are well-structured with annotated screenshots, clear descriptions, and correct labels
4. Issues appear on the Chatverce project board with correct status and priority
5. The terminal summary gives a clear at-a-glance picture of quality
6. A junior developer can run the command and understand every issue filed without additional context
