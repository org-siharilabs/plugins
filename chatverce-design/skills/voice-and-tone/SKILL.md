---
name: voice-and-tone
description: Chatverce voice, tone, and terminology guidelines. Use when writing user-facing text, button labels, error messages, empty states, onboarding copy, tooltips, notifications, or any UI strings for Chatverce. Also use when reviewing existing copy.
user-invocable: false
---

### The Voice Principle

Chatverce speaks like a calm, competent friend who happens to be brilliant at AI — but never makes you feel stupid. Think: the best Apple Store employee. They don't explain the chip architecture. They say "This one's faster."

### Voice Attributes

| Attribute | What it means | Do | Don't |
|-----------|---------------|-----|-------|
| Clear | No jargon, no ambiguity | "Your assistant is live" | "Agent deployed to production endpoint" |
| Confident | Declarative, not hedging | "This will reply automatically" | "This should help with automating replies" |
| Warm | Human, encouraging | "Nice — your first assistant is ready" | "Agent creation successful" |
| Brief | Fewer words, then cut again | "Replies in 2 seconds" | "Average response latency is approximately 2 seconds" |

### Writing Rules

1. **Lead with the outcome, not the mechanism.** "Customers get instant replies" not "The AI processes incoming messages and generates contextual responses"
2. **Use "you" and "your."** "Your assistant" not "The agent." It's theirs.
3. **Present tense.** "Your assistant replies to customers" not "Your assistant will reply to customers"
4. **No exclamation marks in UI.** Exception: first-time milestones ("Your first assistant is live!")
5. **Error messages are solutions.** "Check your WhatsApp connection in Settings" not "Error: Channel disconnected (ERR_WA_401)"
6. **Numbers over adjectives.** "Handled 1,247 conversations" not "Handled many conversations"
7. **Never blame the user.** "That didn't work — try again" not "Invalid input"

### Tone by Context

| Context | Tone | Example |
|---------|------|---------|
| Onboarding | Encouraging, guiding | "Let's set up your first assistant — takes about 3 minutes" |
| Success | Warm, understated | "All set. Your assistant is handling conversations now." |
| Error | Calm, solution-focused | "Something went wrong connecting to WhatsApp. Reconnect in Settings." |
| Empty state | Inviting, forward-looking | "No conversations yet. They'll show up here once your assistant is live." |
| Destructive action | Clear, no drama | "This will permanently delete the assistant and its conversation history." |
| Loading/waiting | Transparent | "Setting things up..." not "Please wait while we process your request" |
| Dashboard stats | Factual, proud | "Your assistants handled 3,421 conversations this week" |

### Button & Action Labels

| Pattern | Do | Don't |
|---------|-----|-------|
| Primary CTA | "Create assistant" | "Submit" / "Create new AI agent" |
| Save | "Save changes" | "Update" / "Apply configuration" |
| Cancel | "Cancel" | "Discard" / "Abort" |
| Delete | "Delete assistant" | "Remove" / "Destroy" |
| Connect | "Connect WhatsApp" | "Configure WhatsApp channel integration" |
| Go live | "Go live" | "Deploy to production" |
| Navigation | "Assistants" | "Agent management" |

### Terminology Dictionary

This is the canonical location for Chatverce terminology. Use these terms in all user-facing strings.

| Technical | Chatverce Says | Why |
|-----------|---------------|-----|
| Deploy | Go live | Universal understanding |
| Agent | Assistant | Friendly, not espionage |
| Node | Step | Sequential, simple |
| Trigger | "When this happens" | Natural language |
| Action | "Do this" | Natural language |
| Conditional/Branch | "Check if" | Natural language |
| Webhook | Connection | Simple |
| API key | Secret key | Familiar concept |
| Workflow | Flow | Shorter |
| NLP/Intent | "Understands" | What, not how |
| Fallback | "If unsure, do this" | Descriptive |
| Variable | Field | Common |
| Endpoint | — | Never show to user |
| Latency | Speed / response time | Human language |
| Uptime | Availability | Or just "always on" |
