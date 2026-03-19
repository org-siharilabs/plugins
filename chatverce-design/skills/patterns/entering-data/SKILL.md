---
name: entering-data
description: Form design that respects the user's time through smart defaults, inline validation, and progressive disclosure
user_invocable: false
---

# Entering Data

## What the User Is Doing

The user is providing information the product needs to work — setting up an assistant, connecting a channel, filling out a business profile, inviting a team member. Data entry is a tax: the user is paying with their attention and time. The form's job is to minimize that tax while capturing what the product genuinely requires.

## Emotional Intention

**"This form respects my time."** The user should feel that every field asked for is necessary, that the form knows what it is doing, and that the experience is as fast as possible. Smart defaults reduce typing. Inline validation prevents submission surprises. Progressive disclosure hides complexity until it is needed.

## How It Works

**Layout principles:**

- **Single column** — never multi-column for primary data entry. Multi-column creates ambiguous reading order and increases cognitive load.
- **Labels above inputs** — not placeholder text as labels. Placeholders disappear on typing and provide no persistent context.
- **Required fields** — marked with `*` and an accessible legend: *"* Required"*. Never use color alone to indicate required.
- **Smart defaults** — pre-fill every field that can be reasonably inferred (country from IP, language from browser, name from existing profile). Let the user correct, not create from scratch.

**Validation:**

- Validate on **blur** (when the field loses focus), not on keystroke (too aggressive) and not only on submit (too late)
- Show inline error message directly below the affected field using `cv-text-error` color and an error icon
- Show inline success (green checkmark) for fields where format validation passes (email, phone number) — not for all fields
- On submit with errors, scroll to the first error and focus it

**Progressive disclosure:**

- Show only the fields required for the current step or goal
- Secondary/optional fields are in a collapsible "Advanced" accordion, collapsed by default
- Multi-step forms use a progress indicator showing current step and total steps

**Multi-step forms:**

1. Each step focuses on one conceptual task (not one field)
2. Validate the current step before allowing advancement; do not allow skipping to a later step if earlier required fields are empty
3. Preserve filled data if the user navigates back — never wipe a previous step's input
4. On final submission, show a loading state on the submit button (disabled + spinner inside button) to prevent double-submission

## Components Used

- **Form** — the container; handles field grouping, layout, and submission logic
- **Text input** — single-line text, email, phone
- **Textarea** — multi-line text (assistant prompt, message templates)
- **Select** — bounded choice sets (language, timezone, role)
- **Checkbox** — multi-select options, agreements
- **Radio button** — single-select from a small set of visible options (3–5 max)
- **File upload** — avatar, document attachments
- **Progress indicator** — multi-step form step position
- **Label** — always visible, always above the input
- **Fieldset** — groups related fields (address block, business hours) with a `legend`

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

Standard form layout. Max content width 480px for focused forms; full-width within a panel for settings forms. Input height 40px. `cv-border-default` for resting border; `cv-border-strong` for focused border. `cv-border-error` when validation fails.

Focus ring: `outline: 2px solid var(--cv-border-accent); outline-offset: 2px`. Always visible — never remove the focus ring.

Tab order must follow visual reading order (top to bottom, left to right). Never reorder tab flow.

### iOS (Expo + NativeWind)

Wrap the entire form in a `ScrollView` with `KeyboardAvoidingView` so the focused input is always visible above the keyboard.

Minimum font size for input text: **16px** — below this, iOS zooms in on focus, which is disorienting and indicates a design error.

Use `returnKeyType` to chain fields: `next` advances focus to the next input, `done` on the last field dismisses the keyboard and submits (or moves to the next step).

Date and time fields use the native `DateTimePicker` — do not build custom date inputs.

### Android (Expo + NativeWind)

Same `ScrollView` + `KeyboardAvoidingView` pattern. 16px minimum input font. Android soft keyboard behavior: use `android:windowSoftInputMode="adjustResize"` (or the React Native equivalent) so the layout resizes to accommodate the keyboard.

Material-adjacent input style: `TextInput` with NativeWind classes mapped to Chatverce tokens. Bottom border style is acceptable on Android if preferred by the platform, but consistent with the web outlined style is also correct.

## Do / Don't

| Do | Don't |
|----|-------|
| Use labels above inputs, always visible | Use placeholder text as the only label — it disappears on typing |
| Validate on blur — when the field loses focus | Validate on every keystroke — it interrupts typing |
| Pre-fill smart defaults wherever reasonable | Make the user type information the product already knows |
| Show error messages inline below the failing field | Show a generic error banner at the top of the form without field-level guidance |
| Disable the submit button while a submission is in-flight | Allow double-submission by leaving the button active |
| Scroll to and focus the first error field on submit | Silently fail submission without indicating which fields need correction |
| Use a `16px` minimum font size on mobile inputs | Use small text on mobile inputs (causes zoom-on-focus on iOS) |

## Chatverce-Specific

**Assistant creation form:** Three sections — (1) Identity (name, avatar), (2) Behavior (system prompt, language), (3) Handoff rules (escalation triggers). Sections 2 and 3 use progressive disclosure — the user sees section 1 first, then section 2 is revealed on scroll. Section 3 is in an "Advanced" accordion.

**Channel connection:** The channel connection form is channel-specific (WhatsApp requires a QR scan; Telegram requires an API token). Use conditional rendering to show the correct flow. Do not show WhatsApp fields when the user has selected Telegram.

**Business profile form:** Auto-populate name, email, and country from the account signup data. The user corrects rather than re-enters. Show a "Looks right?" inline prompt on pre-filled fields to set the expectation that these were filled automatically.

**Phone number inputs:** Always include a country code selector. Use a searchable Select (type to filter country names) rather than a scrollable list of 200 options.
