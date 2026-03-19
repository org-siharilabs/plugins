# Visual QA System — Design Spec

## Goal

A 3-agent pipeline that uses Playwright to screenshot Chatverce web pages across viewports and themes, reviews them against CDG v2.1 with Steve Jobs-level scrutiny, and files structured GitHub issues on the Chatverce project board.

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
| Design guidelines | CDG v2.1 skills (design-ethos, design-foundations, design-components, design-patterns, design-enforcement) |
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
| `routes` | Yes | — | Space-separated list of routes to test (e.g., `/auth`, `/dashboard`). Nested routes like `/dashboard/analytics` are valid — use the full resolved path. |
| `--url` | No | `http://localhost:3000` | Base URL of the running app |
| `--email` | No | `admin@siharilabs.com` | Auth email for login |
| `--otp` | No | `123456` | OTP code for login |

### Orchestration Flow

1. **Resolve base URL:**
   - If `--url` provided: use it directly, skip dev server management
   - If no `--url`: check `http://localhost:3000`
     - If running: use it
     - If not running: find a free port, start dev server (`npm run dev -- --port <port>`), record PID for cleanup
     - **Health check:** after starting, poll the URL with retries (up to 30 seconds, 1s intervals) until it returns HTTP 200. Abort with clear error if timeout.
2. **Dispatch visual-qa-crawler** with base URL + route list + auth credentials
3. **Dispatch steve-jobs-reviewer** with screenshot manifest + CDG skill paths (one route at a time to manage context window — see Agent 2 review strategy)
4. **Dispatch qa-issue-reporter** with findings JSON
5. **Cleanup:** if we started a dev server, kill it by PID
6. **Print terminal summary** — findings grouped by severity with counts

### Error Handling

- **Auth failure:** if the crawler cannot authenticate, abort the run with a clear message naming the failure (wrong credentials, OTP rejected, login page not found). Do not proceed to screenshotting.
- **Route failure:** if a specific route returns 404 or errors, skip that route, log a warning, and continue with remaining routes. Include the failure in the manifest.
- **Reviewer failure:** if the reviewer fails on a route, log the error and continue with remaining routes. Partial findings are still reported.
- **GitHub failure:** if issue creation fails mid-run (rate limit, auth), pause with exponential backoff (1s, 2s, 4s, max 60s). After 3 retries, save unsubmitted findings to `/tmp/visual-qa/pending-issues.json` and print instructions for manual retry.
- **Zero findings:** if the reviewer finds zero issues, that is a valid success. Print a congratulatory summary ("Steve approves.") — do not create any GitHub issues.

---

## Agent 1: `visual-qa-crawler`

### Purpose

Navigate the Chatverce web app, authenticate, and capture systematic screenshots of every requested route across viewports and themes.

### Tools

- Playwright MCP: `browser_navigate`, `browser_fill_form`, `browser_click`, `browser_snapshot`, `browser_take_screenshot`, `browser_resize`, `browser_evaluate`, `browser_press_key`, `browser_wait_for`
- Bash (for filesystem operations)

### Auth Flow

1. Navigate to `<base-url>/auth` (or login page)
2. Enter email (from `--email` param, default `admin@siharilabs.com`)
3. Submit
4. Enter OTP (from `--otp` param, default `123456`)
5. Submit
6. Wait for redirect to authenticated state (up to 10 seconds)
7. **Verify auth succeeded:** check for an authenticated indicator (e.g., dashboard content, user avatar, absence of login form). If verification fails, abort with error: "Auth failed — check credentials and OTP bypass is enabled on target environment."

### Screenshot Matrix

For each route, capture:

| Viewport | Width × Height | Name |
|----------|---------------|------|
| Desktop | 1440 × 900 | `desktop` |
| Tablet | 768 × 1024 | `tablet` |
| Mobile | 375 × 812 | `mobile` |

| Theme | Method | Name |
|-------|--------|------|
| Light | See theme switching strategy below | `light` |
| Dark | See theme switching strategy below | `dark` |

**Theme switching strategy** (try in order):
1. **localStorage key:** Use `browser_evaluate` to set `localStorage.setItem('theme', 'dark')` (or `'light'`) and reload. Check if Chatverce uses a theme key in localStorage.
2. **UI toggle:** If a theme toggle button exists in the UI, click it via Playwright.
3. **CSS media emulation:** Use `browser_run_code` to call Playwright's `page.emulateMedia({ colorScheme: 'dark' })` directly.

The crawler should detect which method works on first route and reuse it for subsequent routes. Log which method was used in the manifest.

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

The ruthless design critic. Reviews every screenshot against CDG v2.1 across all 8 dimensions. Annotates problem areas on screenshots. Outputs structured findings.

### Review Strategy — Context Window Management

The reviewer processes **one route at a time** to avoid context window overflow:

1. **Per-route invocation:** The orchestrator dispatches a separate reviewer invocation for each route. Each invocation receives the 6 screenshots for that route (3 viewports × 2 themes) plus CDG references.
2. **CDG loading:** Enforcement guard sub-files (`token-guard.md`, `theme-guard.md`, `a11y-guard.md`, `pattern-guard.md`) are loaded once per invocation. Other sub-files loaded as needed.
3. **Cross-viewport/theme comparison** happens within the single route invocation (all 6 screenshots are in context together).
4. **Cross-route comparison** is a final lightweight pass by the orchestrator: it reads all per-route findings and flags any systemic patterns (e.g., "same spacing issue on 4 of 5 routes → systemic, not per-route").

Each per-route invocation appends its findings to `findings.json`. The orchestrator merges them.

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
- CDG guidelines — the five `SKILL.md` files are entry points. The reviewer **must read the referenced sub-files** for detailed rules:

  | Skill entry point | Key sub-files to load |
  |---|---|
  | `skills/design-ethos/SKILL.md` | `ive-philosophy.md` |
  | `skills/design-foundations/SKILL.md` | `theme-system.md`, `color.md`, `contrast-matrix.md`, `typography.md`, `layout.md`, `motion.md`, `accessibility.md`, `writing.md` |
  | `skills/design-components/SKILL.md` | `catalog.md` (for component lookup; individual component files loaded as needed) |
  | `skills/design-patterns/SKILL.md` | Loaded per-pattern as needed (e.g., `loading.md`, `onboarding.md`, `feedback.md`) |
  | `skills/design-enforcement/SKILL.md` | `token-guard.md`, `theme-guard.md`, `a11y-guard.md`, `pattern-guard.md` (all four must be loaded for every review) |

  The `SKILL.md` files contain summaries and pointers. The sub-files contain the actual enforcement rules, violation patterns, and detailed specifications. The reviewer must load the enforcement guard sub-files for every review.

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

For P0 and P1 findings, the reviewer produces annotated screenshots. Two-tier approach:

**Tier 1 — DOM-based annotation (preferred):** The crawler keeps the live page state available. The reviewer uses `browser_evaluate` to inject CSS highlight overlays on specific DOM elements (using CSS selectors identified from `browser_snapshot`), then re-screenshots. This produces precise, pixel-accurate annotations because coordinates come from the live DOM.

**Tier 2 — Description-based annotation (fallback):** If DOM-based annotation is not feasible (e.g., the element cannot be targeted by selector), the reviewer describes the problem location in text: quadrant (top-left, center, bottom-right), component name, and approximate position. The finding's `location` field carries this description instead of an annotated screenshot.

**Implementation detail for Tier 1:**
1. The crawler, after taking each screenshot, also captures a `browser_snapshot` (accessibility tree) and saves it alongside the screenshot in the manifest.
2. The reviewer reads the snapshot to identify DOM selectors for problem elements.
3. The reviewer uses `browser_navigate` to the same URL + viewport + theme, then injects a highlight overlay via `browser_evaluate`:
   ```js
   document.querySelector('<selector>').style.outline = '3px solid red';
   document.querySelector('<selector>').style.outlineOffset = '2px';
   ```
4. Re-screenshot and save as `<route>/<viewport>-<theme>-annotated.png`.

Annotation failures are non-blocking. If annotation fails, the finding is still recorded with the original (unannotated) screenshot.

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

### Screenshot Upload

GitHub issues require hosted image URLs. The reporter uses this approach:

1. **Create the issue first** with a placeholder for the screenshot.
2. **Upload the image** using `gh api` to upload as an issue attachment (GitHub's upload endpoint via the issue comment API — attach images in a comment, extract the hosted URL).
3. **Edit the issue body** to replace the placeholder with the hosted URL.

If image upload fails, the issue is still created with a text note: "Screenshot available locally at `/tmp/visual-qa/<path>`". Image embedding failure is non-blocking.

### Deduplication

Before creating issues, the qa-issue-reporter agent uses LLM judgment to deduplicate findings that describe the same underlying problem:

- Same problem on desktop+tablet+mobile → one issue listing all affected viewports
- Same problem in light+dark → one issue noting both themes
- **Grouping criteria:** route + dimension + semantic similarity of title and description (agent judgment, not string matching)

**Merge rules for deduplicated findings:**
- The merged issue inherits all viewport and theme labels from its constituent findings
- All annotated screenshots from constituent findings are included
- Description combines details from all constituent findings, noting which viewports/themes are affected
- Severity is the highest severity among constituent findings (e.g., P0 + P1 → P0)
- Original finding IDs are preserved in the issue body for traceability

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

## Re-run Behavior

When `/visual-qa` is run again on routes that were previously tested:

1. **Before creating issues**, the reporter searches for existing open issues with the `visual-qa` label matching the route (by searching issue titles for `[Visual QA] /<route>`).
2. **New findings** that don't match any existing open issue → create new issues.
3. **Existing findings** that match an open issue → add a comment with updated status ("Still present as of <date>") rather than creating duplicates.
4. **Resolved findings** — existing open issues whose problem is no longer found → add a comment noting the fix and close the issue: "This issue was not detected in the latest visual QA run. Closing as resolved."
5. **Epic updates** — if an Epic exists for the route, update its summary counts and findings checklist.

This makes re-runs safe and idempotent. Each run improves the issue state rather than polluting it.

---

## Route Handling

- **Nested routes:** `/dashboard/analytics` is valid. Directory name uses path separators replaced with hyphens: `dashboard-analytics/`.
- **Parameterized routes:** The user provides the full resolved path (e.g., `/user/123/profile`, not `/user/:id/profile`).
- **Redirects:** The crawler follows redirects and screenshots the final destination. The manifest records both the requested path and the final resolved URL.

---

## Plugin Discovery

Claude Code auto-discovers agents from the `agents/` directory and commands from the `commands/` directory. No explicit registration in `plugin.json` is needed. The version bump in `plugin.json` ensures users pull the update.

---

## Constraints and Limitations

1. **Screenshots are visual only** — the steve-jobs-reviewer analyzes rendered screenshots, not source code. For code-level violations (hardcoded tokens, missing ARIA attributes), use the existing `design-reviewer` agent separately.

2. **Auth credentials default to test values** — `admin@siharilabs.com` / OTP `123456` by default, overridable via `--email` and `--otp`. These must work on the target environment (OTP bypass must be enabled).

3. **State capture is best-effort** — empty states and loading states are only captured if the crawler can trigger them through UI interaction. Some states may require specific data conditions.

4. **Annotation quality** — red rectangle overlays are approximate. They highlight the general area, not pixel-perfect boundaries.

5. **GitHub rate limits** — for large runs (many routes, many findings), the reporter may hit GitHub API rate limits. The agent should handle this with retries and backoff.

6. **No video/motion review** — static screenshots only. Motion violations (animation duration, easing) cannot be detected. The reviewer should note when interactive elements are visible and recommend manual motion review.

---

## Success Criteria

1. Running `/visual-qa /dashboard` produces 6 screenshots (3 viewports × 2 themes)
2. The steve-jobs-reviewer catches all visually-apparent P0 and P1 issues (text unreadable, broken layouts, missing components, contrast failures)
3. GitHub issues are well-structured with annotated screenshots, clear descriptions, and correct labels
4. Issues appear on the Chatverce project board with correct status and priority
5. The terminal summary gives a clear at-a-glance picture of quality
6. A junior developer can run the command and understand every issue filed without additional context
