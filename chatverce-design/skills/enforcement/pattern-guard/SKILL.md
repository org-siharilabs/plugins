---
name: pattern-guard
description: Enforcement guard that catches Chatverce convention violations — terminology, component patterns, platform choices
user_invocable: false
---

# Pattern Guard

Pattern guard enforces Chatverce conventions: the rules that make the product coherent, consistent, and recognizably Chatverce across every surface. These are not aesthetic preferences — they are the product decisions that distinguish Chatverce from a generic SaaS.

---

## Component Pattern Violations

| Violation | Pattern | Fix |
|---|---|---|
| Multiple primary buttons | Two or more `variant="primary"` buttons in the same view or screen section | Keep one primary action; demote others to secondary or ghost |
| Floating labels | `<label>` positioned inside the input field and animating on focus (Material Design style) | Label always above the input, always visible — never floating |
| Non-catalog component | Custom button, input, card, modal, or other component re-implemented from scratch | Use the component from the Chatverce component catalog (`skills/components/`) |
| Missing empty state | List, table, or feed that renders nothing when there is no data | Add an empty state: icon + heading + optional CTA per empty state pattern |
| Raw error message | Error displayed as a raw exception, status code, or technical string | Rewrite as a solution-focused message per writing foundation; no stack traces visible to users |
| Decorative animation | Animation that communicates no state change and exists purely for visual interest | Remove. The only permitted "ambient" animation is the AI working pulse. |
| Form labels below input | Label rendered below its associated input field | Always place labels above inputs |
| Card with action in footer | Card with primary action button in the footer area instead of content | Actions belong in context — not a card footer unless it is a summary/confirm card |
| Gradient on button | Button using a gradient background | Solid background only. No gradients on interactive controls. |
| More than one `h1` per view | Multiple `<h1>` or `accessibilityRole="header"` at the top level | One `h1` (or primary header) per view. Section headers use `h2` and below. |

---

## Voice & Terminology Violations

These violations use the prohibited technical term in a user-facing string. The full table below is the single source of truth — this guard is self-contained.

### Terminology Quick Reference

| Technical term | Chatverce says | Why |
|---|---|---|
| Deploy | Go live | Universal understanding — everyone knows "live" |
| Agent | Assistant | Friendly, approachable — not espionage |
| Node | Step | Sequential, simple, non-technical |
| Trigger | "When this happens" | Natural language — describes function |
| Action | "Do this" | Natural language — describes function |
| Conditional / Branch | "Check if" | Natural language — describes function |
| Webhook | Connection | Simple — what it does, not what it is |
| API key | Secret key | Familiar concept, not a technical term |
| Workflow | Flow | Shorter, friendlier |
| NLP / Intent | "Understands" | What it does for the user — not how |
| Fallback | "If unsure, do this" | Descriptive — explains the behavior |
| Variable | Field | Common across non-technical users |
| Endpoint | *(never show to user)* | Internal concept — always abstract away |
| Latency | Speed / response time | Human language |
| Uptime | Availability / "always on" | Plain language — "always on" where space allows |

### Writing Convention Violations

| Violation | Example | Fix |
|---|---|---|
| Technical jargon in user-facing copy | "Agent deployed to production endpoint" | "Your assistant is live" |
| Passive or hedging language | "This should help automate replies" | "This replies automatically" |
| Title case heading | "Create Your First Assistant" | "Create your first assistant" (sentence case) |
| Exclamation mark outside milestone | "Settings saved!" | "Settings saved" |
| Period on button label | "Save changes." | "Save changes" |
| Ellipsis on button label | "Connect WhatsApp..." | "Connect WhatsApp" |
| Blaming the user in an error | "Invalid input provided" | "That didn't work — try again" |
| Error without a next step | "Connection failed" | "Something went wrong connecting. Check your internet connection." |
| Adjective instead of number | "Replied to many conversations" | "Replied to 247 conversations" |
| "The agent" instead of "your assistant" | "The agent is processing" | "Your assistant is working on it" |

---

## Platform-Specific Violations

### Mobile

| Violation | Pattern | Fix |
|---|---|---|
| Modal on mobile | `Modal` or full-screen dialog for non-critical flows on mobile | Use bottom sheet or drawer instead — modals feel wrong on mobile |
| Missing haptic on key action | Destructive confirm, success (go live), error feedback with no haptic | Add `expo-haptics` call per the haptics pattern in motion foundation |
| Ignoring safe areas | Content underneath iOS notch, Dynamic Island, or Android navigation bar | Wrap in `SafeAreaView` or use `useSafeAreaInsets()` |
| Ignoring platform conventions | Implementing navigation patterns that are foreign to the platform (e.g., breadcrumbs on mobile, swipe-to-delete not available on tap-accessible items) | Follow platform navigation conventions; use native patterns |
| Hardcoded pixel density | `StyleSheet.hairlineWidth` workaround used for layout, or fixed pixel values not using the density-independent unit system | Use density-independent values; rely on NativeWind tokens |
| Missing keyboard-aware scroll | Form in a scroll view that does not push up when the keyboard appears | Wrap in `KeyboardAvoidingView` or use `KeyboardAwareScrollView` |

### Web

| Violation | Pattern | Fix |
|---|---|---|
| Mobile-only patterns on web | Bottom sheet, pull-to-refresh, or swipe gestures as the only interaction method on web | Provide web-appropriate alternatives (modal, manual refresh, button actions) |
| Non-standard navigation pattern | Navigation that doesn't follow web conventions (e.g., no browser back support, broken URLs) | Ensure URL-based routing and browser back/forward work correctly |

---

## Self-Correction Protocol

When a pattern violation is detected:

1. **Terminology violations** — Replace the prohibited term with the Chatverce term from the table above. No exceptions for user-facing strings.
2. **Component violations** — Replace custom re-implementations with catalog components. If a catalog component does not cover the use case, flag it for catalog addition — do not create a one-off.
3. **Platform violations** — Verify the fix is platform-appropriate. A bottom sheet fix on mobile does not need to apply to web.
4. **Writing violations** — Rewrite the copy following the principles in `foundations/writing/SKILL.md`. Lead with outcome, use present tense, sentence case, no drama.

---

## Rationale

Chatverce is a business product used by people who are not technical. Every time the UI uses the word "agent" instead of "assistant," or "deploy" instead of "go live," or shows a raw error code, it alienates the people it is built for. Pattern violations erode trust. Consistent, human language builds it.

Platform conventions are not optional aesthetics — they are the vocabulary users already know. A modal on mobile interrupts the user's mental model. Missing haptics make the app feel unresponsive. These details are the difference between software that feels built and software that feels assembled.
