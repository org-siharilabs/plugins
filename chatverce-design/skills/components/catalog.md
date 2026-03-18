---
name: component-catalog
description: Master index of all Chatverce Design Guide components, tiers, aliases, and platform notes for agent-driven UI construction
user_invocable: false
---

# Component Catalog

## How to Use

This catalog is the authoritative reference for any agent building or reviewing UI for the Chatverce product. When a task involves rendering UI, consult this file first to:

1. **Confirm the component exists.** Every named UI element in Chatverce maps to a row in the table below. If a name is not listed, check the Aliases column — agents frequently use colloquial names.
2. **Navigate to the SKILL.md.** The Path column contains the relative path from `chatverce-design/skills/`. Load that skill before writing any code or markup. The SKILL.md file carries all constraints, token references, and code examples.
3. **Respect the tier.** Full-tier components have complete SKILL.md files with web + mobile examples. Stub-tier components have minimal guidance — do not over-engineer them, and flag if you need a full specification.
4. **Check platform notes.** Many components behave differently across web, iOS, and Android. Read the Platform Notes column before making implementation decisions. Full details are in the SKILL.md Platform Considerations section.
5. **Use token names, never raw values.** All code referencing color, spacing, shadow, or radius must use `cv-*` semantic tokens from `foundations/color/SKILL.md` and `foundations/theme-system/SKILL.md`. Never hardcode hex values.

---

## Component Table

| Component | Path | Tier | Aliases | Platform Notes |
|-----------|------|------|---------|----------------|
| Accordion | components/accordion/ | Stub | Collapsible, Expandable, Disclosure | Web: details/summary or Radix. Mobile: Animated View |
| Alert | components/alert/ | Full | Banner, Inline notification, System message | Persistent in-page. Not a toast. |
| Avatar | components/avatar/ | Full | Profile picture, User image, Initials | Fallback to initials if image fails |
| Badge | components/badge/ | Full | Chip, Tag, Label, Lozenge, Pill | No native equivalent — custom View on mobile |
| Breadcrumbs | components/breadcrumbs/ | Stub | Breadcrumb trail, Page path | Web only. Mobile uses back navigation pattern. |
| Button | components/button/ | Full | CTA, Action button, Submit, Trigger | 44px min touch target. Haptic on mobile. |
| Button Group | components/button-group/ | Stub | Action group, Segmented buttons | Horizontal stack. Max 3–4 items. |
| Card | components/card/ | Full | Panel, Tile, Surface, Container | Hover shadow web. No hover on mobile. |
| Carousel | components/carousel/ | Stub | Slideshow, Image slider, Scroll strip | Web: Embla. Mobile: FlatList horizontal. |
| Checkbox | components/checkbox/ | Full | Multi-select, Check | Indeterminate state required. |
| Color Picker | components/color-picker/ | Stub | Color swatch, Hue selector | Web only in v2. |
| Combobox | components/combobox/ | Full | Autocomplete, Searchable select, Typeahead | Debounced. Mobile: modal search. |
| Date Input | components/date-input/ | Stub | Date field, Date text input | Plain text input with date format hint. |
| Datepicker | components/datepicker/ | Full | Calendar picker, Date selector | Web: calendar. iOS: wheel. Android: material. |
| Drawer | components/drawer/ | Full | Side panel, Slide-over, Sheet, Bottom sheet | Web: side panel. Mobile: @gorhom/bottom-sheet. |
| Dropdown Menu | components/dropdown-menu/ | Full | Context menu, Action menu, Menu | role="menu". Escape closes. |
| Empty State | components/empty-state/ | Full | Zero state, Blank state, No content | Always include CTA. Never blank. |
| Fieldset | components/fieldset/ | Stub | Form group, Input group, Field group | Groups related inputs. Use legend. |
| File | components/file/ | Stub | File item, Attachment display | Displays a single attached file with metadata. |
| File Upload | components/file-upload/ | Full | Dropzone, Upload area, Attachment | Drag-drop web. Document picker mobile. |
| Footer | components/footer/ | Stub | Page footer, Site footer | Web only. Links + legal copy. |
| Form | components/form/ | Full | Form container, Field set | Single-column. Labels above. Progressive disclosure. |
| Header | components/header/ | Stub | App header, Page header, Navbar | Web top bar. Mobile navigation header. |
| Heading | components/heading/ | Stub | Title, Section heading, H1-H6 | Semantic heading levels. One H1 per screen. |
| Hero | components/hero/ | Stub | Banner, Splash, Above the fold | Landing/marketing sections. Not in app shell. |
| Icon | components/icon/ | Stub | Glyph, Pictogram | Lucide React. 16/20/24px. cv-text-secondary default. |
| Image | components/image/ | Stub | Photo, Illustration, Picture | next/image web. expo Image mobile. Alt required. |
| Label | components/label/ | Stub | Form label, Input label, Field label | htmlFor pairing. Never placeholder-only. |
| Link | components/link/ | Stub | Anchor, Inline link, Text link | cv-text-link. Underline on hover. |
| List | components/list/ | Stub | Unordered list, Ordered list, Item list | ul/ol web. FlatList mobile. |
| Modal | components/modal/ | Full | Dialog, Overlay, Popup, Alert dialog | Focus trap. Escape closes. iOS pageSheet. Android bottom sheet. |
| Navigation | components/navigation/ | Full | Nav bar, Sidebar, Tab bar, Menu | Web sidebar. Mobile bottom tab (max 5). |
| Pagination | components/pagination/ | Stub | Page controls, Page numbers | Web only. Mobile uses infinite scroll. |
| Popover | components/popover/ | Full | Floating panel, Tooltip-like, Inline card | Click-triggered. Mobile → bottom sheet at small sizes. |
| Progress Bar | components/progress-bar/ | Stub | Loading bar, Progress track | Horizontal. Determinate only. |
| Progress Indicator | components/progress-indicator/ | Full | Step indicator, Wizard progress, Onboarding steps | Steps/dots. Current/completed/upcoming. |
| Quote | components/quote/ | Stub | Blockquote, Pull quote, Citation | Teal left border. cv-bg-surface-sunken bg. |
| Radio Button | components/radio-button/ | Full | Radio, Option selector, Single select | Teal fill. role="radio". Always in group. |
| Rating | components/rating/ | Stub | Star rating, Score, Review stars | Read-only or interactive. 1–5 stars. |
| Rich Text Editor | components/rich-text-editor/ | Stub | WYSIWYG, Text editor, Formatted input | Web: Tiptap. Mobile: plain Textarea. |
| Search Input | components/search-input/ | Full | Search bar, Search field, Filter input | Magnifier icon. Clear button. Debounce 300ms. |
| Segmented Control | components/segmented-control/ | Stub | Button tabs, Toggle group, Switch tabs | iOS: SegmentedControl. Web: button group. |
| Select | components/select/ | Full | Dropdown select, Picker, Single select | Web: custom. iOS: ActionSheet/wheel. Android: Material. |
| Separator | components/separator/ | Stub | Divider, Rule, HR | Horizontal or vertical. cv-border-default. |
| Skeleton | components/skeleton/ | Full | Loading placeholder, Shimmer, Ghost | Shapes match content. aria-busy="true". |
| Skip Link | components/skip-link/ | Stub | Skip to content, Accessibility bypass | Visually hidden until focused. Web only. |
| Slider | components/slider/ | Stub | Range input, Scrubber | Teal track fill. 44px touch target. |
| Spinner | components/spinner/ | Stub | Loading spinner, Activity indicator | Teal. Web: CSS animation. Mobile: ActivityIndicator. |
| Stack | components/stack/ | Stub | Layout stack, VStack, HStack | Layout primitive. Not a visible component. |
| Stepper | components/stepper/ | Stub | Numeric input, Increment/decrement | Plus/minus buttons around value. |
| Table | components/table/ | Full | Data table, Grid, Data list | Mobile: horizontal scroll or card view. |
| Tabs | components/tabs/ | Full | Tab bar, Tab panel, Tab list | Arrow keys. role="tablist". |
| Text Input | components/text-input/ | Full | Input, Text field, Input field | 16px mobile min. cv-bg-surface-sunken. |
| Textarea | components/textarea/ | Full | Multiline input, Text area, Message box | Resizable web. Optional char count. |
| Toast | components/toast/ | Full | Snackbar, Notification, Flash message | Auto-dismiss 4s. Max 2 stacked. aria-live. |
| Toggle | components/toggle/ | Full | Switch, On/off switch | role="switch". cv-bg-accent when on. Haptic mobile. |
| Tooltip | components/tooltip/ | Full | Hint, Helper text, Info popup | 500ms delay. Mobile: long-press or Popover. |
| Tree View | components/tree-view/ | Stub | Tree, Nested list, File tree | Expandable hierarchy. Keyboard nav. |
| Video | components/video/ | Stub | Video player, Media player | Web: HTML video. Mobile: expo-video. |
| Visually Hidden | components/visually-hidden/ | Stub | Screen reader only, SR-only, Accessible label | CSS clip. Never display:none. |
| Agent Builder Canvas | components/agent-builder-canvas/ | Full | Flow canvas, Node canvas, Workflow editor | Web only (v2). Mobile: read-only viz. |
| Agent Builder Node | components/agent-builder-node/ | Full | Flow node, Workflow node, Agent step | Color-coded by type. Drag handle. Connection ports. |
| AI Working Indicator | components/ai-working-indicator/ | Stub | Thinking indicator, AI loader, Processing state | Animated teal dots. aria-live="polite". |
| Channel Indicator | components/channel-indicator/ | Stub | Platform badge, Channel badge, Source indicator | WhatsApp/SMS/email color dots. |
| Conversation Thread | components/conversation-thread/ | Full | Chat thread, Message list, Chat view | react-virtual web. FlatList inverted mobile. |
| Dashboard Stat Card | components/dashboard-stat-card/ | Stub | KPI card, Metric card, Stat tile | Icon + label + value + trend. |

---

## Common Compositions

These are frequently built UI patterns composed from multiple components. Each entry lists the components to combine and the SKILL.md files to load.

### Form Pattern
Components: `form/`, `fieldset/`, `text-input/`, `textarea/`, `select/`, `checkbox/`, `radio-button/`, `toggle/`, `label/`, `button/`
Use: Any data collection screen — agent setup, account settings, billing, onboarding.
Rule: Single-column only. Labels above fields. One primary action (Button/accent) per form. Secondary action (Button/ghost) for cancel.

### Data View Pattern
Components: `table/`, `skeleton/`, `badge/`, `avatar/`, `empty-state/`, `pagination/`, `search-input/`
Use: Listing screens — agent list, conversation history, contact list, billing history.
Rule: Always pair with skeleton (loading) and empty-state (zero data). Skeleton shapes must match table row height.

### Settings Pattern
Components: `form/`, `toggle/`, `select/`, `text-input/`, `separator/`, `heading/`, `button/`
Use: Configuration screens — agent settings, notification preferences, account settings.
Rule: Group related settings with Separator + Heading. Each group is a Card. Destructive actions (delete account) go at the bottom, separated.

### Overlay Pattern
Components: `modal/` or `drawer/`, `heading/`, `form/`, `button/`
Use: Confirmations, quick-add forms, detail views triggered inline.
Rule: One modal action at a time. Modal for confirmation dialogs. Drawer for content-heavy side panels. Never nest modals.

### Feedback Pattern
Components: `toast/`, `alert/`, `empty-state/`, `spinner/`, `skeleton/`
Use: Loading, success, error, and zero-data states throughout the product.
Rule: Toast for transient feedback. Alert for persistent in-page conditions. Empty state for zero data. Spinner for short indeterminate loads (<3s). Skeleton for content loading.

### Agent Builder Pattern
Components: `agent-builder-canvas/`, `agent-builder-node/`, `drawer/`, `popover/`, `button/`, `tooltip/`
Use: The visual workflow editor for building Chatverce agents.
Rule: Canvas is web-only in v2. Node selection opens a Drawer for properties. Popover for port-level hints. Tooltip on nodes for quick labels.

### Conversation Pattern
Components: `conversation-thread/`, `avatar/`, `badge/`, `channel-indicator/`, `ai-working-indicator/`, `text-input/`, `button/`
Use: The live conversation view, inbox, and agent testing panel.
Rule: AI messages left-aligned, human messages right-aligned. Timestamp grouped by day. Channel indicator on thread header. AI Working Indicator between last message and next AI response.

### Dashboard Pattern
Components: `dashboard-stat-card/`, `table/`, `badge/`, `avatar/`, `skeleton/`, `empty-state/`, `chart (external)/`
Use: The analytics and overview screens.
Rule: Stat cards in a responsive grid (2-col mobile, 4-col desktop). Always show skeleton on initial load. Chart libraries are external — wrap in a Card component for consistent elevation.

---

## Platform-Specific Mapping

When building for iOS or Android, these web components map to platform-native equivalents. Always read the SKILL.md Platform Considerations section for implementation detail.

| Web Component | iOS Pattern | Android Pattern |
|---------------|------------|-----------------|
| Modal (centered overlay) | `.sheet(.pageSheet)` / `UIModalPresentationPageSheet` | Bottom sheet (ModalBottomSheet) |
| Drawer (side panel) | Bottom sheet via `@gorhom/bottom-sheet` | Bottom sheet via `@gorhom/bottom-sheet` |
| Navigation (sidebar) | Tab bar (`<Tabs>` in Expo Router) — max 5 items | Bottom navigation bar (same) |
| Select (custom dropdown) | ActionSheet or UIPickerView (wheel) | Material spinner / modal picker |
| Datepicker (calendar) | Native wheel picker (`DateTimePicker`) | Material date picker (`DateTimePicker`) |
| Popover (positioned float) | Bottom sheet or action sheet for small screens | Bottom sheet for small screens |
| Pagination (page numbers) | Infinite scroll / FlatList `onEndReached` | Same |
| Tooltip (hover hint) | Long-press action or remove entirely | Long-press action or remove entirely |
| Breadcrumbs | Back button (header) | Back button (header) |
| Carousel | FlatList horizontal with `pagingEnabled` | Same |
| Rich Text Editor | Plain Textarea with formatting stripped | Same |
| Skip Link | Not applicable (no keyboard-only navigation) | Not applicable |
| Footer | Not applicable (screen-based navigation) | Not applicable |
