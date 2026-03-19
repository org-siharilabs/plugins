---
name: privacy
description: Privacy as a design principle — data UI patterns, permission requests, transparency, and user data control for Chatverce
user_invocable: false
---

## Philosophy

Privacy is a design principle, not a legal checkbox. It is not the responsibility of the legal team alone — it is designed into every surface that touches user data.

Adapted from Apple's Human Interface Guidelines: people have a right to know what data is collected, why it is needed, and how it is used. Design for that right proactively, not reactively. If a feature cannot be explained to a user in plain language without embarrassment, it should not ship.

Chatverce handles business conversations, customer data, and AI-processed messages. The stakes are high. Users must be able to trust the product with their customers' information.

---

## Principles

**Explain, in plain language, what is collected and why.**
Avoid legal language in the product UI. If data is being stored, processed, or transmitted, say so clearly in terms of what it means for the user — not what it means for compliance.

**Request only the permissions you need, at the moment you need them.**
Permissions requested before context is established feel intrusive and are more likely to be denied. Ask at the point of value — when the user is about to do the thing that requires the permission.

**Never collect silently.**
No data collection, tracking, or processing should happen without the user's awareness. If an AI model processes a conversation, the user should know that is happening.

**Provide clear deletion paths.**
Users must be able to delete their data. Deletion options must be discoverable — not buried in settings sub-menus. The path to delete data should be as easy as the path to create it.

**Prefer on-device processing.**
Where technically feasible, process data on-device rather than sending it to a server. Surface this to users as a trust signal when it applies.

---

## Permission Requests

When a feature requires a system permission (notifications, contacts, camera, microphone, location), the request must be designed — not thrown at the user by the OS alone.

**Pattern:**
1. Explain the benefit to the user in one sentence before triggering the system prompt.
2. Never request permissions on first launch without any context of what the user is trying to do.
3. Design for graceful degradation if the permission is denied — the app must remain functional.
4. If a permission is denied, provide a path to re-enable it (link to Settings) without pestering the user.

**Example copy:**
> "Enable notifications so you know when customers message — even when Chatverce is in the background."

The system permission prompt follows immediately after. The pre-prompt copy sets context so the user understands what they are agreeing to.

**What not to do:**
- Do not request notifications permission at app launch with no context.
- Do not block core functionality when a non-essential permission is denied.
- Do not show the same permission request repeatedly after the user has denied it.

---

## Data Visibility

Users must be able to understand what data their assistants can access and what the AI is doing with conversations.

**AI processing disclosure:**
Any conversation that is processed by an AI model must carry a visual indicator. This does not need to be alarming — a subtle label or icon in the conversation thread is sufficient. Users should never be surprised to learn their conversations were AI-processed.

**Access scope:**
When setting up an assistant, show users what data the assistant has access to (e.g., conversation history, contact info, order data from integrations). Present this as a list — not buried in documentation.

**Stored vs. transient data:**
Distinguish clearly between:
- **Stored data:** Conversation history, assistant configuration, connected contact records — persisted and queryable.
- **Transient data:** Message content processed by the AI in real time to generate a reply — not retained after the response is sent (where applicable).

Label these distinctions in any data-facing settings screen or privacy summary.

---

## Destructive Data Actions

Any action that permanently deletes user data requires explicit confirmation with clear consequences stated. Do not make deletion feel casual.

**Pattern for destructive confirmation:**

> "This will permanently delete [thing] and [consequences]. This cannot be undone."

Avoid vague language like "Are you sure?" or "This action is irreversible." Be specific about what is being deleted and what is lost.

**Examples:**
- "This will permanently delete the assistant and its entire conversation history. This cannot be undone."
- "Disconnecting WhatsApp will remove all conversation history from this channel."

**Account deletion:**
Account deletion must have a cooling-off period. After a user initiates account deletion, apply a grace period (e.g., 14 days) during which:
- The account is deactivated but not yet purged.
- The user can cancel the deletion.
- The user is informed of the timeline via email.

After the grace period, data is permanently deleted and cannot be recovered. State this timeline explicitly at the point of initiating deletion.

**Button labels for destructive actions:**
Use specific, unambiguous labels. "Delete assistant" not "Remove." "Disconnect channel" not "Unlink." The label should match what will happen.

---

## Platform Considerations

### iOS

- **App Tracking Transparency (ATT):** If any cross-app or cross-site tracking is used, the ATT prompt is required before any identifier is accessed. Follow the same pre-prompt pattern described in Permission Requests — explain the value before the system prompt appears.
- **Privacy nutrition labels:** App Store privacy labels must accurately reflect all data collected. Design teams must be aware of what data the app collects so labels stay current with product changes.
- **On-device processing:** Prefer Core ML and on-device inference for any AI features that can run locally. Surface this as a trust signal in product copy.

### Android

- **Runtime permissions:** Android 6.0+ requires runtime permission requests for sensitive permissions. Follow the contextual permission pattern — never request at launch.
- **Data safety section:** Google Play's data safety section must accurately reflect collection and sharing practices. Keep this in sync with any new data-touching features.
- **Sensitive data:** Avoid writing sensitive data (conversation content, credentials) to external storage. Use encrypted internal storage.

### Web

- **Cookie consent:** Any non-essential cookies require informed consent before being set. The consent UI must be honest — do not design dark patterns that make "accept all" easier than "reject all."
- **Data practices:** Link to a plain-language data practices summary from any settings screen that touches user data. Legal privacy policies are not a substitute for product-level clarity.
- **Session data:** Be explicit in UI about what is stored in the browser (localStorage, sessionStorage, IndexedDB) vs. on the server.
