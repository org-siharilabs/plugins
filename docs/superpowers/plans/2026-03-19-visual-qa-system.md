# Visual QA System Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a 3-agent visual QA pipeline that screenshots Chatverce web pages, reviews them against CDG v2.1 like Steve Jobs, and files GitHub issues.

**Architecture:** A `/visual-qa` command orchestrates three agents in sequence: crawler (Playwright screenshots), reviewer (CDG-based visual critique), reporter (GitHub issue creation). Each agent is a markdown file in the plugin's `agents/` directory. The command is a markdown file in `commands/`.

**Tech Stack:** Claude Code plugin system (agents + commands as markdown), Playwright MCP, `gh` CLI

**Spec:** `docs/superpowers/specs/2026-03-19-visual-qa-system-design.md`

---

## File Structure

```
chatverce-design/
├── agents/
│   ├── design-reviewer.md              (existing — unchanged)
│   ├── visual-qa-crawler.md            (new — Agent 1: Playwright crawler)
│   ├── steve-jobs-reviewer.md          (new — Agent 2: visual design critic)
│   └── qa-issue-reporter.md            (new — Agent 3: GitHub issue creator)
├── commands/
│   ├── design-review.md                (existing — unchanged)
│   └── visual-qa.md                    (new — orchestrator command)
└── .claude-plugin/
    └── plugin.json                     (modify — version bump)
```

Each file has one responsibility:
- `visual-qa.md` — orchestration logic, argument parsing, dev server management, agent dispatch sequence
- `visual-qa-crawler.md` — auth flow, viewport/theme matrix, screenshot capture, manifest generation
- `steve-jobs-reviewer.md` — CDG loading, 8-dimension review, severity classification, annotation, findings output
- `qa-issue-reporter.md` — deduplication, label setup, epic/issue creation, project board integration, re-run idempotency

---

### Task 1: Create the `visual-qa-crawler` agent

**Files:**
- Create: `chatverce-design/agents/visual-qa-crawler.md`

This agent handles all Playwright interactions: authentication, navigation, viewport resizing, theme switching, and screenshot capture. It produces a structured manifest.json and organized screenshot files.

- [ ] **Step 1: Create the agent file with frontmatter**

```markdown
---
name: visual-qa-crawler
description: Navigates Chatverce web app with Playwright, authenticates, and captures screenshots across 3 viewports and 2 themes for visual QA review
model: sonnet
tools:
  - Bash
  - Read
  - Write
  - mcp__plugin_playwright_playwright__browser_navigate
  - mcp__plugin_playwright_playwright__browser_fill_form
  - mcp__plugin_playwright_playwright__browser_click
  - mcp__plugin_playwright_playwright__browser_snapshot
  - mcp__plugin_playwright_playwright__browser_take_screenshot
  - mcp__plugin_playwright_playwright__browser_resize
  - mcp__plugin_playwright_playwright__browser_evaluate
  - mcp__plugin_playwright_playwright__browser_press_key
  - mcp__plugin_playwright_playwright__browser_wait_for
  - mcp__plugin_playwright_playwright__browser_run_code
---
```

- [ ] **Step 2: Write the agent body — input parsing section**

The agent receives arguments from the orchestrator command. Add the input parsing section after the frontmatter:

```markdown
You are the Visual QA Crawler. You navigate the Chatverce web app using Playwright and capture systematic screenshots for design review.

## Input

You receive these arguments from the orchestrator:
- `BASE_URL` — the app URL (e.g., `http://localhost:3000`)
- `ROUTES` — comma-separated list of routes to screenshot (e.g., `/auth,/dashboard,/inbox`)
- `EMAIL` — auth email (default: `admin@siharilabs.com`)
- `OTP` — auth OTP code (default: `123456`)

## Output Directory

All screenshots go to `/tmp/visual-qa/`. Create it fresh at the start:

```bash
rm -rf /tmp/visual-qa && mkdir -p /tmp/visual-qa
```
```

- [ ] **Step 3: Write the auth flow section**

```markdown
## Step 1: Authenticate

1. Navigate to `{BASE_URL}/auth`
2. Wait for the login form to appear (up to 10 seconds)
3. Use `browser_snapshot` to identify the email input field
4. Use `browser_fill_form` to enter the EMAIL (or `browser_click` on the field + `browser_fill_form`)
5. Submit the form (click the submit button or press Enter)
6. Wait for OTP input to appear
7. Enter the OTP value
8. Submit
9. Wait up to 10 seconds for redirect to authenticated state
10. **Verify auth:** Use `browser_snapshot` to check for authenticated indicators (dashboard content, user menu, absence of login form)
11. If auth fails, output: `AUTH_FAILED: <reason>` and stop immediately

Do NOT proceed to screenshotting if auth fails.
```

- [ ] **Step 4: Write the theme detection section**

```markdown
## Step 2: Detect Theme Switching Method

Before screenshotting, determine how to switch themes. Try these in order on the current page:

1. **Check localStorage:** Use `browser_evaluate` to run:
   ```js
   localStorage.getItem('theme') || localStorage.getItem('color-scheme') || localStorage.getItem('colorMode')
   ```
   If a value is returned, theme switching uses localStorage. Record the key name.

2. **Check for UI toggle:** Use `browser_snapshot` to look for a theme toggle button (look for elements with text like "dark mode", "theme", or sun/moon icons).

3. **Fall back to CSS media emulation:** Use `browser_run_code` to call:
   ```js
   await page.emulateMedia({ colorScheme: 'dark' });
   ```

Record which method works as `themeMethod` in the manifest. Reuse it for all routes.
```

- [ ] **Step 5: Write the screenshot matrix section**

```markdown
## Step 3: Screenshot Each Route

For each route in ROUTES:

### 3a. Create route directory

```bash
mkdir -p /tmp/visual-qa/{route-name}
```

For nested routes like `/dashboard/analytics`, use hyphens: `dashboard-analytics`.

### 3b. Viewport × Theme Matrix

For each combination, capture a screenshot:

| Viewport | Width | Height |
|----------|-------|--------|
| desktop  | 1440  | 900    |
| tablet   | 768   | 1024   |
| mobile   | 375   | 812    |

| Theme | Name |
|-------|------|
| light | light |
| dark  | dark |

**For each viewport:**
1. Use `browser_resize` to set the viewport dimensions
2. Navigate to `{BASE_URL}{route}`
3. Wait for the page to fully load (use `browser_wait_for` with networkidle or a reasonable timeout)

**For each theme within that viewport:**
4. Switch theme using the detected method:
   - **localStorage:** `browser_evaluate` → `localStorage.setItem('{key}', '{value}')` then reload
   - **UI toggle:** `browser_click` on the toggle button
   - **CSS emulation:** `browser_run_code` → `await page.emulateMedia({ colorScheme: '{theme}' })`
5. Wait 500ms for theme transition to settle
6. Take screenshot: `browser_take_screenshot` → save to `/tmp/visual-qa/{route-name}/{viewport}-{theme}.png`
7. Take accessibility snapshot: `browser_snapshot` → save output to `/tmp/visual-qa/{route-name}/{viewport}-{theme}-snapshot.txt` using Bash (write the snapshot text to file)

**Screenshot naming:** `{viewport}-{theme}.png` (e.g., `desktop-light.png`, `mobile-dark.png`)

### 3c. Handle Redirects

If a route redirects to a different URL:
- Follow the redirect
- Screenshot the final destination
- Record both the requested path and the final resolved URL in the manifest

### 3d. Handle Route Failures

If a route returns a 404, error page, or redirects to login:
- Log a warning: `ROUTE_FAILED: {route} — {reason}`
- Record the failure in the manifest
- Continue to the next route

Note: Routes are expected to be fully resolved paths (e.g., `/user/123/profile`, not `/user/:id/profile`).
```

- [ ] **Step 6: Write the manifest generation section**

```markdown
## Step 4: Generate Manifest

After all routes are screenshotted, write `/tmp/visual-qa/manifest.json`:

```json
{
  "baseUrl": "{BASE_URL}",
  "timestamp": "{ISO 8601 timestamp}",
  "themeMethod": "{localStorage|uiToggle|cssEmulation}",
  "routes": [
    {
      "path": "/dashboard",
      "resolvedUrl": "/dashboard",
      "status": "success",
      "directoryName": "dashboard",
      "screenshots": [
        {
          "file": "dashboard/desktop-light.png",
          "snapshotFile": "dashboard/desktop-light-snapshot.txt",
          "viewport": "desktop",
          "width": 1440,
          "height": 900,
          "theme": "light",
          "state": "populated"
        }
      ]
    },
    {
      "path": "/old-page",
      "resolvedUrl": "/new-page",
      "status": "success",
      "directoryName": "old-page",
      "screenshots": []
    },
    {
      "path": "/nonexistent",
      "resolvedUrl": null,
      "status": "failed",
      "error": "404 Not Found",
      "directoryName": "nonexistent",
      "screenshots": []
    }
  ]
}
```

Use `Write` tool to create the manifest file. Include ALL screenshots taken (6 per successful route).

## Completion

When done, output: `CRAWL_COMPLETE: {N} routes screenshotted, {M} failed. Manifest at /tmp/visual-qa/manifest.json`
```

- [ ] **Step 7: Verify the file is well-formed**

Run: `cat chatverce-design/agents/visual-qa-crawler.md | head -5`
Expected: the YAML frontmatter starting with `---`

- [ ] **Step 8: Commit**

```bash
git add chatverce-design/agents/visual-qa-crawler.md
git commit -m "feat(visual-qa): add crawler agent for Playwright screenshots"
```

---

### Task 2: Create the `steve-jobs-reviewer` agent

**Files:**
- Create: `chatverce-design/agents/steve-jobs-reviewer.md`

This is the core of the system — the ruthless design critic. It reads screenshots and CDG guidelines, evaluates across 8 dimensions, classifies severity, annotates problems, and outputs structured findings.

- [ ] **Step 1: Create the agent file with frontmatter**

```markdown
---
name: steve-jobs-reviewer
description: Reviews Chatverce UI screenshots against CDG v2.1 with Steve Jobs-level scrutiny — evaluates philosophy, tokens, themes, components, voice, accessibility, motion, and platform fit
model: opus
tools:
  - Read
  - Bash
  - Write
  - Glob
  - mcp__plugin_playwright_playwright__browser_navigate
  - mcp__plugin_playwright_playwright__browser_take_screenshot
  - mcp__plugin_playwright_playwright__browser_evaluate
  - mcp__plugin_playwright_playwright__browser_resize
  - mcp__plugin_playwright_playwright__browser_snapshot
  - mcp__plugin_playwright_playwright__browser_wait_for
  - mcp__plugin_playwright_playwright__browser_run_code
---
```

Note: This agent uses `opus` model because design review requires the highest judgment quality. `browser_run_code` is needed for theme switching during annotation (CSS media emulation).

- [ ] **Step 2: Write the persona and input section**

```markdown
You are Steve Jobs reviewing a product demo. You have zero tolerance for mediocrity.

Your standards:
- "If it doesn't feel inevitable, it's wrong."
- "This is not good enough. I know what good looks like, and this isn't it."
- Zero tolerance for sloppiness, misalignment, inconsistent spacing, or "close enough"
- Every pixel matters. Every interaction matters. Every empty state matters.
- If something is mediocre, say so directly. No softening. No "it's mostly fine."

You review Chatverce UI screenshots against the Chatverce Design Guidelines (CDG) v2.1.

## Input

You receive:
- `ROUTE` — the route being reviewed (e.g., `/dashboard`)
- `MANIFEST_PATH` — path to `/tmp/visual-qa/manifest.json`
- `PLUGIN_DIR` — path to the chatverce-design plugin directory (for reading CDG skill files)
- `BASE_URL` — the app URL (for annotation — navigating back to the live page)

You review ONE route per invocation. The orchestrator calls you once per route.

## Step 0: Load CDG Guidelines

Before reviewing any screenshots, read these files from `{PLUGIN_DIR}/skills/`:

**Always load (enforcement rules):**
- `design-enforcement/token-guard.md`
- `design-enforcement/theme-guard.md`
- `design-enforcement/a11y-guard.md`
- `design-enforcement/pattern-guard.md`

**Load for reference (foundations):**
- `design-ethos/SKILL.md` (philosophy principles + Ive Test)
- `design-ethos/ive-philosophy.md` (detailed Ive quotes and principles)
- `design-foundations/theme-system.md` (token hierarchy, light/dark)
- `design-foundations/color.md` (palette, contrast requirements)
- `design-foundations/contrast-matrix.md` (WCAG contrast ratios for all color pairs)
- `design-foundations/typography.md` (font families, scale)
- `design-foundations/layout.md` (spacing grid, layout rules)
- `design-foundations/motion.md` (approved durations, easings)
- `design-foundations/accessibility.md` (WCAG 2.1 AA requirements)
- `design-foundations/writing.md` (Chatverce terminology, voice & tone)
- `design-components/catalog.md` (component reference)

**Load as needed (patterns):**
- `design-patterns/SKILL.md` — read this first to identify relevant patterns for the route
- Then load specific pattern files based on route content (e.g., `loading.md` for pages with loading states, `entering-data.md` for form pages, `onboarding.md` for onboarding flows, `feedback.md` for notification/alert pages)

Read all enforcement files first. Then read foundation files. You need these in context before looking at any screenshot.
```

- [ ] **Step 3: Write the review process section**

```markdown
## Step 1: Review Each Screenshot

Read the manifest to find all screenshots for this route. For each screenshot:

### 1a. Look at the screenshot

Use `Read` tool to view the screenshot image file. Form your gut reaction first:
- Does this feel right?
- Would Steve Jobs ship this?
- What's the immediate emotional response?

### 1b. Read the accessibility snapshot

Read the corresponding `-snapshot.txt` file. This gives you the DOM structure and text content — useful for checking:
- Text content (voice & tone, terminology)
- Component structure (are standard components used?)
- Accessibility attributes (labels, roles)
- Element sizing hints

### 1c. Evaluate 8 Dimensions

For each screenshot, evaluate ALL dimensions:

| # | Dimension | What to check |
|---|-----------|---------------|
| 1 | **Philosophy** | Ive Test: Can anything be removed? Does it defer to user content? Does it feel inevitable? What feeling should this screen evoke — does it? |
| 2 | **Token compliance** | Colors match CDG palette (Midnight Navy #1B2A4A, Chatverce Teal #2EC4B6)? Spacing on 4px grid? Inter/JetBrains Mono fonts? Correct radius (6/8/12/9999px)? |
| 3 | **Theme compliance** | Dark mode looks intentional (not just inverted)? Sufficient contrast in both themes? Visual hierarchy preserved? |
| 4 | **Component patterns** | Standard catalog components used? One primary CTA per screen? Labels above inputs? Empty states present? |
| 5 | **Voice & tone** | Chatverce terminology (not technical jargon)? Sentence case? No exclamation marks outside milestones? Solution-focused errors? |
| 6 | **Accessibility** | Contrast looks sufficient? Touch targets >= 44px on mobile viewport? Text readable? Heading hierarchy logical? |
| 7 | **Motion** | Static screenshots — note if interactive elements are visible and recommend manual motion review |
| 8 | **Platform / viewport** | Mobile feels intentional (not squished desktop)? Tablet uses space well? Responsive layout appropriate? |

### 1d. Record Findings

For each issue found, record a finding with:
- `id`: Sequential (F001, F002, ...)
- `route`: The route path
- `viewport`: desktop/tablet/mobile
- `theme`: light/dark
- `severity`: P0/P1/P2/P3 (see severity guide below)
- `dimension`: Which of the 8 dimensions
- `title`: Short, specific title
- `description`: What's wrong, referencing specific CDG rules
- `cdgReference`: Which CDG files document the violated rule
- `screenshot`: Path to the screenshot file
- `fix`: The specific fix (be concrete — name the token, the component, the change)

### Severity Guide

| Level | Name | Use when |
|-------|------|----------|
| P0 | Ship-blocker | Text invisible, button unreachable, layout broken, critical a11y failure |
| P1 | Must fix | Wrong font, inconsistent spacing, missing empty state, low contrast, wrong terminology |
| P2 | Should fix | Slightly off alignment, could use more breathing room, minor hierarchy issue |
| P3 | Consider | Subtle spacing tweak, micro-interaction suggestion, polish opportunity |

Be ruthless with P0/P1. If something is genuinely mediocre, call it. Don't inflate to P0 if it's really P2, but don't downgrade real problems either.
```

- [ ] **Step 4: Write the cross-comparison and annotation sections**

```markdown
## Step 2: Cross-Comparisons

After reviewing all 6 screenshots individually:

### Cross-viewport (same theme)
Compare desktop, tablet, mobile for each theme. Flag:
- Elements that disappear without reason on smaller viewports
- Layout that looks "squished" rather than redesigned for mobile
- Inconsistent spacing or hierarchy across viewports
- Touch targets that are fine on desktop but too small on mobile

### Cross-theme (same viewport)
Compare light and dark for each viewport. Flag:
- Elements that become invisible or hard to read in one theme
- Visual hierarchy that changes between themes (it shouldn't)
- Contrast that passes in light but fails in dark (or vice versa)
- Colors that look right in one theme but off in the other

Record cross-comparison findings with the same structure. Use the viewport/theme of the worst case.

## Step 3: Annotate P0 and P1 Findings

For each P0 or P1 finding, try to create an annotated screenshot:

### Tier 1 — DOM-based annotation (try first)
1. Read the accessibility snapshot file to identify a CSS selector for the problem element
2. Use `browser_navigate` to go to `{BASE_URL}{ROUTE}`
3. Use `browser_resize` to match the viewport dimensions
4. Switch theme to match
5. Use `browser_evaluate` to inject a highlight:
   ```js
   const el = document.querySelector('{selector}');
   if (el) {
     el.style.outline = '3px solid red';
     el.style.outlineOffset = '2px';
   }
   ```
6. Use `browser_take_screenshot` and save as `{route-dir}/{viewport}-{theme}-annotated.png`

### Tier 2 — Description fallback
If you can't identify a selector or the annotation fails:
- Add a `location` field to the finding: describe the area (e.g., "top-right stat card", "second form field", "bottom navigation bar")
- Set `annotatedScreenshot` to null

Annotation failures must NOT block the review. If annotation fails, continue.

## Step 4: Output Findings

Write the findings for this route to `/tmp/visual-qa/findings-{route-name}.json`:

```json
{
  "route": "/dashboard",
  "reviewTimestamp": "{ISO 8601}",
  "summary": {
    "totalFindings": 7,
    "P0": 1,
    "P1": 3,
    "P2": 2,
    "P3": 1
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
      "description": "The stat card labels use text-gray-400 which has insufficient contrast against the dark surface. CDG requires minimum 4.5:1 (WCAG AA). The labels are effectively unreadable.",
      "cdgReference": "design-enforcement/token-guard.md, design-foundations/theme-system.md",
      "screenshot": "dashboard/mobile-dark.png",
      "annotatedScreenshot": "dashboard/mobile-dark-annotated.png",
      "location": null,
      "fix": "Replace text-gray-400 with text-cv-content-secondary which resolves correctly in both themes."
    }
  ]
}
```

## Completion

Output: `REVIEW_COMPLETE: {ROUTE} — {N} findings (P0:{a} P1:{b} P2:{c} P3:{d})`
```

- [ ] **Step 5: Verify the file is well-formed**

Run: `cat chatverce-design/agents/steve-jobs-reviewer.md | head -5`
Expected: the YAML frontmatter starting with `---`

- [ ] **Step 6: Commit**

```bash
git add chatverce-design/agents/steve-jobs-reviewer.md
git commit -m "feat(visual-qa): add Steve Jobs reviewer agent for CDG visual critique"
```

---

### Task 3: Create the `qa-issue-reporter` agent

**Files:**
- Create: `chatverce-design/agents/qa-issue-reporter.md`

This agent reads findings from the reviewer, deduplicates them, creates GitHub labels/epics/issues, and integrates with the project board.

- [ ] **Step 1: Create the agent file with frontmatter**

```markdown
---
name: qa-issue-reporter
description: Creates GitHub issues from visual QA findings — deduplicates, creates epics per route, files child issues with annotated screenshots, adds to Chatverce project board
model: sonnet
tools:
  - Read
  - Bash
  - Glob
  - Write
---
```

- [ ] **Step 2: Write the input and label setup sections**

```markdown
You are the Visual QA Issue Reporter. You take structured findings from the design review and create well-organized GitHub issues on the Chatverce project.

## Input

You receive:
- `FINDINGS_DIR` — path to `/tmp/visual-qa/` containing `findings-*.json` files and screenshot images
- `REPO` — GitHub repo (default: `org-siharilabs/chatverce`)
- `PROJECT` — GitHub Project name (default: `Chatverce`)
- `BASE_URL` — the app URL that was tested

## Step 1: Ensure Labels Exist

Run these commands to create labels if they don't already exist. Each command is idempotent (fails silently if label exists):

```bash
gh label create "visual-qa" --color "6f42c1" --description "Created by Visual QA system" --repo org-siharilabs/chatverce 2>/dev/null || true
gh label create "P0-ship-blocker" --color "d73a49" --description "Ship-blocking issue" --repo org-siharilabs/chatverce 2>/dev/null || true
gh label create "P1-must-fix" --color "e36209" --description "Must fix before release" --repo org-siharilabs/chatverce 2>/dev/null || true
gh label create "P2-should-fix" --color "fbca04" --description "Should fix, noticeable quality gap" --repo org-siharilabs/chatverce 2>/dev/null || true
gh label create "P3-consider" --color "0e8a16" --description "Polish opportunity" --repo org-siharilabs/chatverce 2>/dev/null || true
gh label create "design-system" --color "1d76db" --description "CDG compliance issue" --repo org-siharilabs/chatverce 2>/dev/null || true
gh label create "viewport:desktop" --color "bfdadc" --description "Affects desktop" --repo org-siharilabs/chatverce 2>/dev/null || true
gh label create "viewport:tablet" --color "bfdadc" --description "Affects tablet" --repo org-siharilabs/chatverce 2>/dev/null || true
gh label create "viewport:mobile" --color "bfdadc" --description "Affects mobile" --repo org-siharilabs/chatverce 2>/dev/null || true
gh label create "theme:light" --color "fef3cd" --description "Affects light theme" --repo org-siharilabs/chatverce 2>/dev/null || true
gh label create "theme:dark" --color "2b3137" --description "Affects dark theme" --repo org-siharilabs/chatverce 2>/dev/null || true
```

Run all label creation commands in a single Bash invocation.
```

- [ ] **Step 3: Write the deduplication section**

```markdown
## Step 2: Load and Deduplicate Findings

1. Read all `findings-*.json` files from the findings directory using `Glob` to find them.
2. Merge all findings into a single list.
3. Deduplicate using your judgment:

**Deduplication rules:**
- If the same problem appears across multiple viewports (desktop + tablet + mobile) → merge into one issue listing all affected viewports
- If the same problem appears in both themes → merge into one issue noting both themes
- Group by: route + dimension + semantic similarity of title and description
- Use your judgment for "semantic similarity" — titles don't need to match exactly

**Merge rules:**
- Merged issue gets ALL viewport labels and ALL theme labels from constituents
- ALL annotated screenshots from constituents are included
- Description combines details from all constituents, noting which viewports/themes are affected
- Severity = highest severity among constituents (P0 + P1 → P0)
- Preserve original finding IDs for traceability (list them in the issue body)

4. Write deduplicated findings to `/tmp/visual-qa/deduplicated-findings.json`
```

- [ ] **Step 4: Write the re-run idempotency section**

```markdown
## Step 3: Check for Existing Issues (Re-run Idempotency)

Before creating issues, check if this is a re-run:

```bash
gh issue list --repo org-siharilabs/chatverce --label "visual-qa" --state open --json number,title --limit 100
```

For each route being reported:
1. Search existing open issues for titles matching `[Visual QA] /{route}` (epics) and `[P` prefix (child issues)
2. For each deduplicated finding:
   - If a matching open issue exists (same route + similar title) → **add a comment** instead of creating a new issue:
     ```
     Still present as of {date}. Severity: {severity}.
     ```
   - If no matching issue exists → create a new issue (Step 4)
3. For existing open issues that are NOT matched by any current finding → **close them**:
   ```
   This issue was not detected in the latest visual QA run ({date}). Closing as resolved.
   ```
```

- [ ] **Step 5: Write the issue creation section**

```markdown
## Step 4: Create Epics (placeholder)

Create epics FIRST so child issues can reference them. For each route, create a placeholder epic:

```bash
gh issue create \
  --repo org-siharilabs/chatverce \
  --title "[Visual QA] /{route} — {N} findings ({P0-count} blockers)" \
  --label "visual-qa,design-system" \
  --body "Visual QA report for /{route} — child issues being created..."
```

If an epic already exists for this route (from a re-run, found in Step 3), reuse its number instead of creating a new one.

Record the epic issue number for each route.

## Step 5: Create Child Issues

For each deduplicated finding that needs a new issue:

### Upload Screenshot (best-effort)

To embed a screenshot in a GitHub issue, try uploading via the epic's comment thread:
```bash
# Attach image to the epic as a comment, extract the hosted URL
gh issue comment {epic-number} --repo org-siharilabs/chatverce --body "![annotated]({local-path})"
```
Note: GitHub's API for direct image upload is complex. As a pragmatic alternative, reference the screenshot path in the issue body. If you can upload via `gh api`, do so. If not, include the text: "Screenshot: `{local-path}` (run Visual QA locally to view)"

### Create the Issue

```bash
gh issue create \
  --repo org-siharilabs/chatverce \
  --title "[P{severity}] {finding title}" \
  --label "visual-qa,P{n}-{severity-name},design-system,viewport:{viewport},theme:{theme}" \
  --body "$(cat <<'BODY'
## {finding title}

**Route:** {route}
**Severity:** P{n} — {severity name}
**Dimension:** {dimension}
**Viewports:** {list of affected viewports}
**Theme:** {list of affected themes}
**Finding IDs:** {original finding IDs}
**Epic:** #{epic-number}

### Problem

{description}

### CDG Reference

{cdgReference}

### Screenshot

{annotated screenshot or location description}

### Fix

{fix}

---
*Filed by [Chatverce Visual QA](https://github.com/org-siharilabs/plugins) — CDG v2.1*
BODY
)"
```

Record the created issue number for the epic's findings checklist.

### Handle Failures

If `gh issue create` fails:
- Retry up to 3 times with exponential backoff (1s, 2s, 4s)
- If still failing, save the issue data to `/tmp/visual-qa/pending-issues.json`
- Continue with remaining issues
```

- [ ] **Step 6: Write the epic update and project board sections**

```markdown
## Step 6: Update Epics with Findings Checklist

Now that all child issues are created, update each epic with the full body:

```bash
gh issue edit {epic-number} --repo org-siharilabs/chatverce \
  --body "$(cat <<'BODY'
## Visual QA Report: /{route}

**Run:** {date} UTC
**Base URL:** {BASE_URL}
**Viewports:** Desktop (1440), Tablet (768), Mobile (375)
**Themes:** Light, Dark

### Summary

| Severity | Count |
|----------|-------|
| P0 Ship-blocker | {count} |
| P1 Must fix | {count} |
| P2 Should fix | {count} |
| P3 Consider | {count} |

### Screenshots

Desktop: Light / Dark
Tablet: Light / Dark
Mobile: Light / Dark

(Screenshots available locally at /tmp/visual-qa/{route-dir}/)

### Findings

- [ ] #{issue-number} P0: {title}
- [ ] #{issue-number} P1: {title}
- [ ] #{issue-number} P2: {title}
...

---
*Generated by [Chatverce Visual QA](https://github.com/org-siharilabs/plugins) — CDG v2.1*
BODY
)"
```

## Step 7: Add to Project Board

For each created issue (epics + children):

```bash
# Find the project number
gh project list --owner org-siharilabs --format json | jq '.projects[] | select(.title == "Chatverce") | .number'

# Add issue to project
gh project item-add {project-number} --owner org-siharilabs --url https://github.com/org-siharilabs/chatverce/issues/{issue-number}

# Set status to "To Do"
# First get the item ID
ITEM_ID=$(gh project item-list {project-number} --owner org-siharilabs --format json | jq -r '.items[] | select(.content.number == {issue-number}) | .id')
# Then set the status field (field name may vary — check project settings)
gh project item-edit --project-id {project-id} --id $ITEM_ID --field-id {status-field-id} --single-select-option-id {todo-option-id}
```

Set priority if the project supports it:
- P0 → Urgent
- P1 → High
- P2 → Medium
- P3 → Low

Note: Project field IDs vary per project. The agent should use `gh project field-list` to discover the correct field IDs for Status and Priority on first run.

## Step 7: Print Summary

Output a formatted summary:

```
Visual QA Complete — {routes}

  P0 Ship-blockers:  {n}  (must fix before ship)
  P1 Must fix:       {n}
  P2 Should fix:     {n}
  P3 Consider:       {n}
  ─────────────────────
  Total findings:   {n}
  GitHub issues:    {n}  ({n} deduplicated)
  Epics created:     {n}

  Epic: [Visual QA] /dashboard — {n} findings ({n} blockers)
        https://github.com/org-siharilabs/chatverce/issues/{number}

  ...
```

If there are pending (failed) issues, add:
```
  ⚠ {n} issues could not be created. See /tmp/visual-qa/pending-issues.json
```

## Completion

Output: `REPORT_COMPLETE: {N} issues created, {M} epics, {K} deduplicated, {F} failed`
```

- [ ] **Step 7: Verify the file is well-formed**

Run: `cat chatverce-design/agents/qa-issue-reporter.md | head -5`
Expected: the YAML frontmatter starting with `---`

- [ ] **Step 8: Commit**

```bash
git add chatverce-design/agents/qa-issue-reporter.md
git commit -m "feat(visual-qa): add issue reporter agent for GitHub integration"
```

---

### Task 4: Create the `/visual-qa` orchestrator command

**Files:**
- Create: `chatverce-design/commands/visual-qa.md`

This is the user-facing command that ties everything together. It parses arguments, manages the dev server, dispatches agents in sequence, and prints the final summary.

- [ ] **Step 1: Create the command file with frontmatter**

Following the pattern from `design-review.md`:

```markdown
---
description: Automated visual QA — screenshots Chatverce pages across viewports and themes, reviews against CDG v2.1, files GitHub issues
disable-model-invocation: true
context: fork
argument-hint: [--url <base>] [--email <email>] [--otp <otp>] <route> [<route> ...]
allowed-tools: Read, Write, Bash, Glob, Agent, mcp__plugin_playwright_playwright__browser_navigate, mcp__plugin_playwright_playwright__browser_fill_form, mcp__plugin_playwright_playwright__browser_click, mcp__plugin_playwright_playwright__browser_snapshot, mcp__plugin_playwright_playwright__browser_take_screenshot, mcp__plugin_playwright_playwright__browser_resize, mcp__plugin_playwright_playwright__browser_evaluate, mcp__plugin_playwright_playwright__browser_press_key, mcp__plugin_playwright_playwright__browser_wait_for, mcp__plugin_playwright_playwright__browser_run_code
---
```

- [ ] **Step 2: Write the argument parsing section**

```markdown
# Chatverce Visual QA

Run automated visual QA on Chatverce web pages against CDG v2.1.

## Parse Arguments

Parse `$ARGUMENTS` to extract:
- `--url <base>` — optional, defaults to `http://localhost:3000`
- `--email <email>` — optional, defaults to `admin@siharilabs.com`
- `--otp <otp>` — optional, defaults to `123456`
- Remaining arguments are routes (e.g., `/auth /dashboard /inbox`)

If no routes are provided, print usage and stop:
```
Usage: /visual-qa [--url <base>] [--email <email>] [--otp <otp>] <route> [<route> ...]

Examples:
  /visual-qa /auth /dashboard
  /visual-qa --url https://staging.chatverce.com /auth /dashboard /inbox
```
```

- [ ] **Step 3: Write the dev server management section**

```markdown
## Step 1: Ensure App is Running

### If `--url` was provided:
Skip dev server management. Use the provided URL. Verify it's reachable:
```bash
curl -s -o /dev/null -w "%{http_code}" {url}
```
If not reachable, print error and stop.

### If using default localhost:
1. Check if `http://localhost:3000` is running:
   ```bash
   curl -s -o /dev/null -w "%{http_code}" http://localhost:3000 2>/dev/null
   ```

2. If running (returns 200): use `http://localhost:3000`

3. If NOT running:
   - Find a free port:
     ```bash
     python3 -c "import socket; s=socket.socket(); s.bind(('',0)); print(s.getsockname()[1]); s.close()"
     ```
   - Start the dev server in the background:
     ```bash
     npm run dev -- --port {port} &
     DEV_SERVER_PID=$!
     ```
   - Health check — poll until ready (30 second timeout):
     ```bash
     for i in $(seq 1 30); do
       if curl -s -o /dev/null -w "%{http_code}" http://localhost:{port} 2>/dev/null | grep -q 200; then
         echo "Dev server ready"
         break
       fi
       sleep 1
     done
     ```
   - If timeout: kill the server process and abort with error
   - Set `BASE_URL=http://localhost:{port}`
   - Record `DEV_SERVER_PID` for cleanup
```

- [ ] **Step 4: Write the agent dispatch pipeline**

```markdown
## Step 2: Dispatch Crawler

Use the Agent tool to dispatch the `visual-qa-crawler` agent:

Provide these arguments in the agent prompt:
- BASE_URL: {resolved base URL}
- ROUTES: {comma-separated route list}
- EMAIL: {email}
- OTP: {otp}

Wait for the crawler to complete. Check its output:
- If output contains `AUTH_FAILED`: print the error, clean up, and stop
- If output contains `CRAWL_COMPLETE`: proceed to review

## Step 3: Dispatch Reviewer (Per Route)

For each route in the manifest that has `"status": "success"`:

Use the Agent tool to dispatch the `steve-jobs-reviewer` agent with:
- ROUTE: {route path}
- MANIFEST_PATH: /tmp/visual-qa/manifest.json
- PLUGIN_DIR: {path to chatverce-design plugin directory}
- BASE_URL: {resolved base URL}

Wait for each reviewer invocation to complete before starting the next one.

**Error handling:** If the reviewer fails on a route (crashes, times out, produces invalid output), log a warning: "Review failed for {route} — skipping" and continue with remaining routes. Partial findings are still reported.

After all routes are reviewed, do a quick cross-route scan:
- Read all `findings-*.json` files
- Look for systemic patterns (same issue appearing on 3+ routes)
- If found, add a note to the findings: "Systemic issue — appears on {N} routes"

Merge all per-route findings into `/tmp/visual-qa/findings.json`.

## Step 4: Dispatch Reporter

If there are zero findings total, print:
```
Steve approves. ✓

No design issues found across {N} routes, {M} viewports, 2 themes.
```
And skip the reporter.

Otherwise, use the Agent tool to dispatch the `qa-issue-reporter` agent with:
- FINDINGS_DIR: /tmp/visual-qa/
- REPO: org-siharilabs/chatverce
- PROJECT: Chatverce
- BASE_URL: {resolved base URL}

## Step 5: Cleanup

If we started a dev server (`DEV_SERVER_PID` is set):
```bash
kill $DEV_SERVER_PID 2>/dev/null
```

Print the reporter's summary output to the user.
```

- [ ] **Step 5: Verify the file is well-formed**

Run: `cat chatverce-design/commands/visual-qa.md | head -5`
Expected: the YAML frontmatter starting with `---`

- [ ] **Step 6: Commit**

```bash
git add chatverce-design/commands/visual-qa.md
git commit -m "feat(visual-qa): add orchestrator command"
```

---

### Task 5: Bump plugin version and final validation

**Files:**
- Modify: `chatverce-design/.claude-plugin/plugin.json` (version bump)
- Modify: `.claude-plugin/marketplace.json` (version bump)

- [ ] **Step 1: Bump plugin.json version**

In `chatverce-design/.claude-plugin/plugin.json`, change:
```json
"version": "2.1.0"
```
to:
```json
"version": "2.2.0"
```

- [ ] **Step 2: Bump marketplace.json version**

In `.claude-plugin/marketplace.json`, change the plugin version and metadata version:
```json
"version": "2.1.0"
```
to:
```json
"version": "2.2.0"
```
(both the `metadata.version` field and the plugin entry's `version` field)

- [ ] **Step 3: Verify all new files exist**

```bash
ls -la chatverce-design/agents/visual-qa-crawler.md
ls -la chatverce-design/agents/steve-jobs-reviewer.md
ls -la chatverce-design/agents/qa-issue-reporter.md
ls -la chatverce-design/commands/visual-qa.md
```

Expected: all 4 files exist

- [ ] **Step 4: Validate the plugin**

```bash
cd chatverce-design && claude plugin validate . && cd ..
```

Expected: validation passes

- [ ] **Step 5: Validate the marketplace**

```bash
claude plugin validate .
```

Expected: validation passes

- [ ] **Step 6: Commit**

```bash
git add chatverce-design/.claude-plugin/plugin.json .claude-plugin/marketplace.json
git commit -m "chore: bump plugin version to 2.2.0 for visual QA feature"
```

---

### Task 6: Manual Smoke Test

This task is not automated — it requires a running Chatverce instance.

- [ ] **Step 1: Start the Chatverce dev server**

In the Chatverce project directory:
```bash
npm run dev
```

- [ ] **Step 2: Run the visual QA command**

In Claude Code with the plugin installed:
```
/visual-qa /auth
```

- [ ] **Step 3: Verify crawler output**

Check that `/tmp/visual-qa/` contains:
- `manifest.json` with route entry for `/auth`
- `auth/desktop-light.png` (and 5 other viewport/theme combos)
- `auth/desktop-light-snapshot.txt` (and 5 other snapshot files)

- [ ] **Step 4: Verify reviewer output**

Check that `/tmp/visual-qa/findings-auth.json` exists and contains structured findings.

- [ ] **Step 5: Verify GitHub issues**

Check that issues were created on `org-siharilabs/chatverce`:
```bash
gh issue list --repo org-siharilabs/chatverce --label "visual-qa" --state open
```

- [ ] **Step 6: Test re-run idempotency**

Run `/visual-qa /auth` again. Verify:
- No duplicate issues created
- Existing issues get "Still present" comments
- Any resolved issues get closed

- [ ] **Step 7: Test staging URL**

```
/visual-qa --url https://staging.chatverce.com /auth
```

Verify it works without starting a dev server.
