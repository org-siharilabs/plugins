
## Voice and Tone

### The Voice Principle

Chatverce speaks like a calm, competent friend who happens to be brilliant at AI — but never makes you feel stupid. Think: the best Apple Store employee. They don't explain the chip architecture. They say "This one's faster."

The product is powerful. The language is not. Complexity lives in the system — not in the words we use to describe it.

### Voice Attributes

| Attribute | What it means | Do | Don't |
|---|---|---|---|
| Clear | No jargon, no ambiguity | "Your assistant is live" | "Agent deployed to production endpoint" |
| Confident | Declarative, not hedging | "This will reply automatically" | "This should help with automating replies" |
| Warm | Human, encouraging | "Nice — your first assistant is ready" | "Agent creation successful" |
| Brief | Fewer words, then cut again | "Replies in 2 seconds" | "Average response latency is approximately 2 seconds" |

### Tone by Context

Tone shifts with context — the voice stays constant, the register adjusts.

| Context | Tone | Example |
|---|---|---|
| Onboarding | Encouraging, guiding | "Let's set up your first assistant — takes about 3 minutes" |
| Success | Warm, understated | "All set. Your assistant is handling conversations now." |
| Error | Calm, solution-focused | "Something went wrong connecting to WhatsApp. Reconnect in Settings." |
| Empty state | Inviting, forward-looking | "No conversations yet. They'll show up here once your assistant is live." |
| Destructive action | Clear, no drama | "This will permanently delete the assistant and its conversation history." |
| Loading / waiting | Transparent | "Setting things up..." |
| Dashboard stats | Factual, proud | "Your assistants handled 3,421 conversations this week" |

### Button and Action Labels

| Pattern | Do | Don't |
|---|---|---|
| Primary CTA | "Create assistant" | "Submit" / "Create new AI agent" |
| Save | "Save changes" | "Update" / "Apply configuration" |
| Cancel | "Cancel" | "Discard" / "Abort" |
| Delete | "Delete assistant" | "Remove" / "Destroy" |
| Connect | "Connect WhatsApp" | "Configure WhatsApp channel integration" |
| Go live | "Go live" | "Deploy to production" |
| Navigation | "Assistants" | "Agent management" |


## Terminology

This is the single source of truth for Chatverce terminology. These mappings apply to all user-facing strings across web, mobile, and notifications. When in doubt, default to the Chatverce term. Never use the technical term in any user-visible context.

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

**Using this table:**
- If a term is listed here, it is the only acceptable term in UI copy.
- If an engineer or PM uses the technical term in a design review, redirect to the Chatverce term.
- New technical concepts introduced by engineering must be mapped here before UI copy is written.


## Copy Mechanics

These are the writing rules that apply to all Chatverce UI text. They are not style preferences — they are product standards.

**Lead with outcome, not mechanism.**
Write what the feature does for the user — not how it works internally.
- "Customers get instant replies" — not "The AI processes incoming messages and generates contextual responses"

**Use "you" and "your."**
The product belongs to the user. Make language possessive.
- "Your assistant" not "The agent." "Your conversations" not "The conversation history."

**Present tense.**
The product exists now. Write in the present.
- "Your assistant replies to customers" — not "Your assistant will reply to customers"

**No exclamation marks in UI.**
Exception: first-time milestones only — moments of genuine celebration the user has earned.
- Acceptable: "Your first assistant is live!"
- Not acceptable: "Settings saved!" / "Welcome back!"

**Error messages are solutions.**
An error message that does not tell the user what to do next has failed.
- "Check your WhatsApp connection in Settings" — not "Error: Channel disconnected (ERR_WA_401)"

**Numbers over adjectives.**
Concrete numbers are more trustworthy and more useful than vague qualifiers.
- "Handled 1,247 conversations" — not "Handled many conversations"
- "Replies in under 2 seconds" — not "Replies quickly"

**Never blame the user.**
When something fails, the system failed — not the user. Language must reflect this.
- "That didn't work — try again" — not "Invalid input"
- "We couldn't connect — check your internet connection" — not "Connection failed due to network error"

**Sentence case for all headings and labels.**
Capitalize the first word only (plus proper nouns). Never title case for UI strings.
- "Create your first assistant" — not "Create Your First Assistant"

**No period at the end of button labels.**
Buttons are actions, not sentences.
- "Save changes" — not "Save changes."

**No ellipsis in button labels.**
An ellipsis suggests the action is incomplete or uncertain. Buttons must be decisive.
- "Connect WhatsApp" — not "Connect WhatsApp..."
