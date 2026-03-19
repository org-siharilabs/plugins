---
description: Automated visual QA — screenshots Chatverce pages across viewports and themes, reviews against CDG v2.1, files GitHub issues
disable-model-invocation: true
context: fork
argument-hint: [--url <base>] [--email <email>] [--otp <otp>] <route> [<route> ...]
allowed-tools: Read, Write, Bash, Glob, Agent, mcp__plugin_playwright_playwright__browser_navigate, mcp__plugin_playwright_playwright__browser_fill_form, mcp__plugin_playwright_playwright__browser_click, mcp__plugin_playwright_playwright__browser_snapshot, mcp__plugin_playwright_playwright__browser_take_screenshot, mcp__plugin_playwright_playwright__browser_resize, mcp__plugin_playwright_playwright__browser_evaluate, mcp__plugin_playwright_playwright__browser_press_key, mcp__plugin_playwright_playwright__browser_wait_for, mcp__plugin_playwright_playwright__browser_run_code
---

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
