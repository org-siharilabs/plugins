# CDG v2.0 — Plan C: Components Layer

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Write all 77 component skills (71 component.gallery + 6 Chatverce-specific) plus the component catalog index.

**Architecture:** Each component follows the component skill template from the spec with platform considerations for Web, iOS, and Android. Components are grouped into batches by depth tier (Full, Standard, Minimal) for efficient authoring. Each batch produces a commit.

**Tech Stack:** Markdown (SKILL.md files)

**Spec:** `docs/specs/2026-03-19-chatverce-design-plugin-v2-design.md` — Part 4: Components (lines 732–808)

**Depends on:** Plan A (foundations must exist for token references and cross-references)

**Working directory:** `/Users/sandeep/Desktop/dev/org-siharilabs/plugins`

---

## Component Skill Template

Every component SKILL.md follows this structure (adapt depth by tier):

```markdown
---
name: {component-name}
description: {one-liner}
user_invocable: false
---

# {Component Name}

**Gallery reference:** https://component.gallery/components/{name}/
**Aliases:** {alt names from gallery}

## When to Use
- {use case}

## When NOT to Use
- {anti-use case — use {alternative} instead}

## Anatomy
{Parts of the component with semantic token references}

## Variants
| Variant | Description | When to Use |
|---------|-------------|-------------|

## Interaction States
| State | Visual | Emotional Intention |
|-------|--------|-------------------|
| Default | ... | ... |
| Hover (web) | ... | ... |
| Pressed/Active | ... | ... |
| Focus | ... | ... |
| Disabled | ... | ... |
| Loading | ... | ... |
| Error | ... | ... |

## Chatverce Styling
{Component tokens (Layer 3) referencing semantic tokens (Layer 2)}

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
{Web-specific behavior, keyboard interaction, hover states}

### iOS (Expo + NativeWind)
{iOS-specific behavior, gestures, haptics, safe areas}

### Android (Expo + NativeWind)
{Android-specific behavior, back button, material elevation}

## Accessibility
{ARIA roles (web), accessibilityRole/Label (mobile), keyboard, focus, screen reader}

## Code Examples

### Web
{shadcn + Tailwind + cv-tokens}

### Mobile
{React Native + NativeWind + cv-tokens}

## Anti-Patterns
- Never: {thing}
```

**Full tier:** All sections above, multiple variants, both code examples, interaction states with emotional intention. ~400-600 words.

**Standard tier:** All sections concise, 1-2 variants, primary platform example. ~200-350 words.

**Minimal tier:** When to Use, Chatverce Styling, Accessibility, single example. ~100-150 words.

---

## File Map

```
chatverce-design/skills/components/
  catalog.md                           # Task 1

  # Full Tier (Tasks 2-8, batched by category)
  button/SKILL.md                      # Task 2: Core interactive
  card/SKILL.md                        #
  modal/SKILL.md                       #
  drawer/SKILL.md                      #
  toast/SKILL.md                       # Task 3: Feedback
  alert/SKILL.md                       #
  empty-state/SKILL.md                 #
  form/SKILL.md                        # Task 4: Data entry
  text-input/SKILL.md                  #
  textarea/SKILL.md                    #
  select/SKILL.md                      #
  checkbox/SKILL.md                    #
  radio-button/SKILL.md                #
  toggle/SKILL.md                      #
  combobox/SKILL.md                    #
  datepicker/SKILL.md                  #
  file-upload/SKILL.md                 #
  search-input/SKILL.md               #
  navigation/SKILL.md                  # Task 5: Navigation
  tabs/SKILL.md                        #
  dropdown-menu/SKILL.md               #
  popover/SKILL.md                     #
  table/SKILL.md                       # Task 6: Data display
  skeleton/SKILL.md                    #
  avatar/SKILL.md                      #
  badge/SKILL.md                       #
  tooltip/SKILL.md                     #
  progress-indicator/SKILL.md          #
  agent-builder-canvas/SKILL.md        # Task 7: Chatverce-specific (Full)
  agent-builder-node/SKILL.md          #
  conversation-thread/SKILL.md         #

  # Standard Tier (Tasks 9-13, batched)
  accordion/SKILL.md                   # Task 8: Navigation/structure (Standard)
  breadcrumbs/SKILL.md                 # Task 8: Navigation/structure
  button-group/SKILL.md                #
  fieldset/SKILL.md                    #
  header/SKILL.md                      #
  footer/SKILL.md                      #
  heading/SKILL.md                     #
  hero/SKILL.md                        # Task 10: Content
  icon/SKILL.md                        #
  image/SKILL.md                       #
  label/SKILL.md                       #
  link/SKILL.md                        #
  list/SKILL.md                        #
  pagination/SKILL.md                  # Task 11: Data/status
  progress-bar/SKILL.md                #
  segmented-control/SKILL.md           #
  separator/SKILL.md                   #
  slider/SKILL.md                      # Task 12: Input/display
  spinner/SKILL.md                     #
  stepper/SKILL.md                     #
  tree-view/SKILL.md                   #
  ai-working-indicator/SKILL.md        # Task 13: Chatverce-specific (Standard)
  channel-indicator/SKILL.md           #
  dashboard-stat-card/SKILL.md         #

  # Minimal Tier (Tasks 14-15, batched)
  carousel/SKILL.md                    # Task 14
  color-picker/SKILL.md                #
  date-input/SKILL.md                  #
  file/SKILL.md                        #
  quote/SKILL.md                       #
  rating/SKILL.md                      # Task 15
  rich-text-editor/SKILL.md            #
  skip-link/SKILL.md                   #
  stack/SKILL.md                       #
  video/SKILL.md                       #
  visually-hidden/SKILL.md             #
```

---

## Tasks

### Task 1: Write Component Catalog

**Files:** Create `chatverce-design/skills/components/catalog.md`

- [ ] **Step 1: Create all component directories**

```bash
cd /Users/sandeep/Desktop/dev/org-siharilabs/plugins/chatverce-design/skills/components

# Full tier
mkdir -p accordion alert avatar badge button card checkbox combobox datepicker drawer dropdown-menu empty-state file-upload form modal navigation popover progress-indicator radio-button search-input select skeleton table tabs text-input textarea toast toggle tooltip

# Standard tier
mkdir -p breadcrumbs button-group fieldset footer header heading hero icon image label link list pagination progress-bar segmented-control separator slider spinner stepper tree-view

# Minimal tier
mkdir -p carousel color-picker date-input file quote rating rich-text-editor skip-link stack video visually-hidden

# Chatverce-specific
mkdir -p agent-builder-canvas agent-builder-node ai-working-indicator channel-indicator conversation-thread dashboard-stat-card
```

- [ ] **Step 2: Write catalog.md**

The catalog is the index AI agents read first. Must include:

1. **How to Use This Catalog** — Instructions for agents: identify components needed → load specific skills → follow the spec.

2. **Component Table** — All 77 components:

```markdown
| Component | Path | Tier | Aliases | Platform Notes |
|-----------|------|------|---------|---------------|
| Accordion | components/accordion/ | Full | Collapse, Collapsible | Web: details/summary or custom. Mobile: Animated.View |
| Alert | components/alert/ | Full | Notification, Banner | Web: div[role=alert]. Mobile: View + accessibilityLiveRegion |
```

Include all 71 gallery components + 6 Chatverce-specific. Sorted alphabetically.

3. **Common Compositions** — From spec (Form, Data view, Settings page, Overlay, Feedback, Agent builder, Conversation, Dashboard).

4. **Platform-Specific Component Mapping** — From spec table (Modal → bottom sheet on mobile, Navigation → tab bar on mobile, etc.).

- [ ] **Step 3: Commit**

```bash
git add chatverce-design/skills/components/
git commit -m "feat(cdg): scaffold component directories and write catalog index"
```

---

### Task 2: Full Tier — Core Interactive Components (5 components)

**Files:** Create SKILL.md in: `button/`, `card/`, `modal/`, `drawer/`, `tooltip/`

- [ ] **Step 1: Write button/SKILL.md**

Variants: Primary (navy/teal), Secondary (outline), Ghost (text-only), Destructive (coral). States: default, hover, active, focus, disabled, loading. Min touch target 44px. One primary per screen. Loading: pulse animation, never spinner replacing label.

Platform: Web = `<Button>` from shadcn. iOS = Pressable with haptic (light impact). Android = Pressable.

- [ ] **Step 2: Write card/SKILL.md**

Surface bg, sm shadow, md radius. Content-first. Hover: elevate to md shadow (web only). No colored borders or header stripes. Min padding 20px.

Platform: Web = div with shadow. iOS = View (minimal shadow). Android = View with elevation.

- [ ] **Step 3: Write modal/SKILL.md**

White/surface elevated bg, lg shadow, lg radius. One clear action. Close: X + overlay click + Escape (web). Focus trap. Enter: fade + scale 95%→100%.

Platform: Web = centered overlay via `<Dialog>`. iOS = `presentationStyle: "pageSheet"` (bottom card). Android = full-screen or bottom sheet.

- [ ] **Step 4: Write drawer/SKILL.md**

Slide from edge. Same card aesthetic as modal. Properties panel for agent builder.

Platform: Web = side panel. iOS/Android = bottom sheet (`@gorhom/bottom-sheet`). Swipe to dismiss on mobile.

- [ ] **Step 5: Write tooltip/SKILL.md**

Contextual info on hover/focus. Dark bg, white text, sm radius. Max width 200px. Delay: 500ms show, immediate hide.

Platform: Web = hover/focus triggered. Mobile = long-press triggered (tooltips don't work with touch). Consider whether Popover is better for mobile contexts.

- [ ] **Step 6: Commit**

```bash
git add chatverce-design/skills/components/{button,card,modal,drawer,tooltip}/SKILL.md
git commit -m "feat(cdg): add core interactive components — button, card, modal, drawer, tooltip"
```

---

### Task 3: Full Tier — Feedback Components (3 components)

**Files:** Create SKILL.md in: `toast/`, `alert/`, `empty-state/`

- [ ] **Step 1: Write toast/SKILL.md**

Transient feedback. Variants: success, error, warning, info, with-action (undo). Auto-dismiss 4s (8s for actionable). Max 2 stacked. Position: web top-right, iOS top, Android bottom. `aria-live="polite"`.

- [ ] **Step 2: Write alert/SKILL.md**

Persistent in-page feedback. Variants: info, warning, error, success. Dismissible or persistent. `role="alert"` + `aria-live="assertive"` for errors.

- [ ] **Step 3: Write empty-state/SKILL.md**

Never blank. Line-art illustration + headline + single CTA. Tone: inviting, never apologetic. "No conversations yet. They'll show up here once your assistant is live."

- [ ] **Step 4: Commit**

```bash
git add chatverce-design/skills/components/{toast,alert,empty-state}/SKILL.md
git commit -m "feat(cdg): add feedback components — toast, alert, empty-state"
```

---

### Task 4: Full Tier — Data Entry Components (11 components)

**Files:** Create SKILL.md in: `form/`, `text-input/`, `textarea/`, `select/`, `checkbox/`, `radio-button/`, `toggle/`, `combobox/`, `datepicker/`, `file-upload/`, `search-input/`

- [ ] **Step 1: Write form/SKILL.md** — Single-column, labels above, progressive disclosure.
- [ ] **Step 2: Write text-input/SKILL.md** — 8px radius, Frost border, 14px text, Teal focus ring.
- [ ] **Step 3: Write textarea/SKILL.md** — Same styling as text-input, resizable, character count optional.
- [ ] **Step 4: Write select/SKILL.md** — Web: custom dropdown or native. iOS: ActionSheet/wheel. Android: Material dropdown.
- [ ] **Step 5: Write checkbox/SKILL.md** — Group selection. Teal check on checked. Indeterminate state.
- [ ] **Step 6: Write radio-button/SKILL.md** — Single selection. Teal fill on selected.
- [ ] **Step 7: Write toggle/SKILL.md** — `role="switch"`. Teal bg when on. Haptic on toggle (mobile).
- [ ] **Step 8: Write combobox/SKILL.md** — Text input + dropdown suggestions. Debounced filtering.
- [ ] **Step 9: Write datepicker/SKILL.md** — Web: calendar dropdown. iOS: native wheel. Android: Material picker.
- [ ] **Step 10: Write file-upload/SKILL.md** — Drag-and-drop (web). Document picker (mobile). Progress indication.
- [ ] **Step 11: Write search-input/SKILL.md** — Magnifying glass icon. Clear button. Debounced query.
- [ ] **Step 12: Commit**

```bash
git add chatverce-design/skills/components/{form,text-input,textarea,select,checkbox,radio-button,toggle,combobox,datepicker,file-upload,search-input}/SKILL.md
git commit -m "feat(cdg): add data entry components — form, inputs, select, toggle, datepicker, file-upload"
```

---

### Task 5: Full Tier — Navigation Components (4 components)

**Files:** Create SKILL.md in: `navigation/`, `tabs/`, `dropdown-menu/`, `popover/`

- [ ] **Step 1: Write navigation/SKILL.md** — Web: sidebar (icon+label, single level). Mobile: bottom tab bar (max 5). Active: teal text + teal 10% bg pill.
- [ ] **Step 2: Write tabs/SKILL.md** — Switch between views. Arrow key navigation. `role="tablist"`. Web: horizontal tabs. Mobile: top tabs or segmented control.
- [ ] **Step 3: Write dropdown-menu/SKILL.md** — Hidden options via button click. `role="menu"`. Arrow keys navigate. Escape closes.
- [ ] **Step 4: Write popover/SKILL.md** — Click-triggered floating content. Web: positioned relative to trigger. Mobile: bottom sheet or modal on small screens.
- [ ] **Step 5: Commit**

```bash
git add chatverce-design/skills/components/{navigation,tabs,dropdown-menu,popover}/SKILL.md
git commit -m "feat(cdg): add navigation components — nav, tabs, dropdown-menu, popover"
```

---

### Task 6: Full Tier — Data Display Components (5 components)

**Files:** Create SKILL.md in: `table/`, `skeleton/`, `avatar/`, `badge/`, `progress-indicator/`

- [ ] **Step 1: Write table/SKILL.md** — Clean, minimal borders. Row hover. Sortable columns. Responsive: horizontal scroll on mobile or card view. Always empty state.
- [ ] **Step 2: Write skeleton/SKILL.md** — Placeholder matching content layout. Shimmer animation. `aria-busy="true"`. Shapes: text lines, circles (avatars), rectangles (cards).
- [ ] **Step 3: Write avatar/SKILL.md** — User/assistant photos. Fallback: initials on colored bg (8 avatar colors). Sizes: sm (32px), md (40px), lg (64px). Online indicator (green dot).
- [ ] **Step 4: Write badge/SKILL.md** — Small label for status/count. Variants: default, success, warning, error, channel. Truncate long text. Max 3 digits for counts ("99+").
- [ ] **Step 5: Write progress-indicator/SKILL.md** — Step-by-step progress (onboarding, forms). Numbered steps or dots. Current/completed/upcoming states.
- [ ] **Step 6: Commit**

```bash
git add chatverce-design/skills/components/{table,skeleton,avatar,badge,progress-indicator}/SKILL.md
git commit -m "feat(cdg): add data display components — table, skeleton, avatar, badge, progress-indicator"
```

---

### Task 7: Full Tier — Chatverce-Specific Components (3 components)

**Files:** Create SKILL.md in: `agent-builder-canvas/`, `agent-builder-node/`, `conversation-thread/`

- [ ] **Step 1: Write agent-builder-canvas/SKILL.md**

Visual, node-based, drag-and-drop. Dot grid background on Snow/Navy-950. Bezier curve connections in Slate, animated flow dots in Teal. Zoom controls bottom-right. Properties panel (Drawer) slides from right.

Platform: Web only for v2 (complex canvas not viable on mobile yet). Mobile: read-only visualization or redirect to web.

- [ ] **Step 2: Write agent-builder-node/SKILL.md**

White card, colored left-border by type: Trigger (Teal), Action (Navy), Condition (Amber), Response (Green). Drag handle. Connection ports.

- [ ] **Step 3: Write conversation-thread/SKILL.md**

Message list with sender avatars. AI vs human message styling. Channel indicator per conversation. Typing indicator. Read receipts. Message grouping by time.

Platform: Web = virtualized list (`react-virtual`). Mobile = `FlatList` with `inverted`.

- [ ] **Step 4: Commit**

```bash
git add chatverce-design/skills/components/{agent-builder-canvas,agent-builder-node,conversation-thread}/SKILL.md
git commit -m "feat(cdg): add Chatverce-specific components — agent builder canvas/node, conversation thread"
```

---

### Task 8-12: Standard Tier Components (24 components in 5 batches)

Each batch follows the same pattern: write SKILL.md at Standard depth (200-350 words, all sections concise, 1-2 variants).

#### Task 8: Standard — Navigation/Structure (7 components)

**Files:** `accordion/`, `breadcrumbs/`, `button-group/`, `fieldset/`, `header/`, `footer/`, `heading/`

- [ ] **Step 1:** Write all 7 SKILL.md files. Brief: each ~250 words, concise variants, primary platform example, accessibility role. Accordion: collapsible sections, `aria-expanded`, arrow icon rotates, animate height with `cv-duration-normal`.
- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/components/{accordion,breadcrumbs,button-group,fieldset,header,footer,heading}/SKILL.md
git commit -m "feat(cdg): add standard components — accordion, breadcrumbs, button-group, fieldset, header, footer, heading"
```

#### Task 9: Standard — Content (6 components)

**Files:** `hero/`, `icon/`, `image/`, `label/`, `link/`, `list/`

- [ ] **Step 1:** Write all 6 SKILL.md files.
- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/components/{hero,icon,image,label,link,list}/SKILL.md
git commit -m "feat(cdg): add standard components — hero, icon, image, label, link, list"
```

#### Task 10: Standard — Data/Status (4 components)

**Files:** `pagination/`, `progress-bar/`, `segmented-control/`, `separator/`

- [ ] **Step 1:** Write all 4 SKILL.md files.
- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/components/{pagination,progress-bar,segmented-control,separator}/SKILL.md
git commit -m "feat(cdg): add standard components — pagination, progress-bar, segmented-control, separator"
```

#### Task 11: Standard — Input/Display (4 components)

**Files:** `slider/`, `spinner/`, `stepper/`, `tree-view/`

- [ ] **Step 1:** Write all 4 SKILL.md files.
- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/components/{slider,spinner,stepper,tree-view}/SKILL.md
git commit -m "feat(cdg): add standard components — slider, spinner, stepper, tree-view"
```

#### Task 12: Standard — Chatverce-Specific (3 components)

**Files:** `ai-working-indicator/`, `channel-indicator/`, `dashboard-stat-card/`

- [ ] **Step 1: Write ai-working-indicator/SKILL.md** — Teal pulse + label. Only allowed decorative motion. "Setting things up...", "Thinking..." variants.
- [ ] **Step 2: Write channel-indicator/SKILL.md** — Small badge with channel icon + channel color. WhatsApp green, Telegram blue, etc. Only place channel colors appear.
- [ ] **Step 3: Write dashboard-stat-card/SKILL.md** — Metric number (large), label (caption), trend indicator (up/down arrow + percentage). "Your assistants handled 3,421 conversations this week."
- [ ] **Step 4: Commit**

```bash
git add chatverce-design/skills/components/{ai-working-indicator,channel-indicator,dashboard-stat-card}/SKILL.md
git commit -m "feat(cdg): add Chatverce-specific standard components — AI indicator, channel indicator, stat card"
```

---

### Task 13-14: Minimal Tier Components (11 components in 2 batches)

Each component gets ~100-150 words: When to Use, Chatverce Styling, Accessibility, one example.

#### Task 13: Minimal Batch 1 (6 components)

**Files:** `carousel/`, `color-picker/`, `date-input/`, `file/`, `quote/`, `rating/`

- [ ] **Step 1:** Write all 6 SKILL.md files at Minimal depth.
- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/components/{carousel,color-picker,date-input,file,quote,rating}/SKILL.md
git commit -m "feat(cdg): add minimal components — carousel, color-picker, date-input, file, quote, rating"
```

#### Task 14: Minimal Batch 2 (5 components)

**Files:** `rich-text-editor/`, `skip-link/`, `stack/`, `video/`, `visually-hidden/`

- [ ] **Step 1:** Write all 5 SKILL.md files at Minimal depth.
- [ ] **Step 2: Commit**

```bash
git add chatverce-design/skills/components/{rich-text-editor,skip-link,stack,video,visually-hidden}/SKILL.md
git commit -m "feat(cdg): add minimal components — rich-text-editor, skip-link, stack, video, visually-hidden"
```

---

## Plan C Complete Checklist

After all 14 tasks, verify:

- [ ] `catalog.md` exists and lists all 77 components
- [ ] 77 component directories exist, each with a SKILL.md
- [ ] All Full-tier components (~31) have complete sections including both code examples and interaction states
- [ ] All Standard-tier components (~24) have all sections at concise depth
- [ ] All Minimal-tier components (~11) have When to Use, Styling, Accessibility, one example
- [ ] Every SKILL.md has YAML frontmatter
- [ ] No hardcoded hex values (reference cv-* tokens)
- [ ] Every component references its gallery URL (except Chatverce-specific)
- [ ] Platform considerations present in every Full and Standard component

```bash
find chatverce-design/skills/components -name "SKILL.md" | wc -l
```

Expected: 77 files

```bash
find chatverce-design/skills/components -name "catalog.md" | wc -l
```

Expected: 1 file
