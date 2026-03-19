
# Component Catalog

## How to Use

This catalog is the authoritative reference for any agent building or reviewing UI for the Chatverce product. When a task involves rendering UI, consult this file first to:

1. **Confirm the component exists.** Every named UI element in Chatverce maps to a row in the table below. If a name is not listed, check the Aliases column — agents frequently use colloquial names.
2. **Navigate to the reference file.** The Path column contains the filename within the `design-components` skill directory. Load that file before writing any code or markup. It carries all constraints, token references, and code examples.
3. **Respect the tier.** Full-tier components have complete reference files with web + mobile examples. Minimal-tier components have essential guidance — do not over-engineer them.
4. **Check platform notes.** Many components behave differently across web, iOS, and Android. Read the Platform Notes column before making implementation decisions. Full details are in each component's Platform Considerations section.
5. **Use token names, never raw values.** All code referencing color, spacing, shadow, or radius must use `cv-*` semantic tokens. Never hardcode hex values.


## Component Table

| Component | Path | Tier | Aliases | Platform Notes |
|-----------|------|------|---------|----------------|
| Accordion | design-components/accordion.md | Stub | Collapsible, Expandable, Disclosure | Web: details/summary or Radix. Mobile: Animated View |
| Alert | design-components/alert.md | Full | Banner, Inline notification, System message | Persistent in-page. Not a toast. |
| Avatar | design-components/avatar.md | Full | Profile picture, User image, Initials | Fallback to initials if image fails |
| Badge | design-components/badge.md | Full | Chip, Tag, Label, Lozenge, Pill | No native equivalent — custom View on mobile |
| Breadcrumbs | design-components/breadcrumbs.md | Stub | Breadcrumb trail, Page path | Web only. Mobile uses back navigation pattern. |
| Button | design-components/button.md | Full | CTA, Action button, Submit, Trigger | 44px min touch target. Haptic on mobile. |
| Button Group | design-components/button-group.md | Stub | Action group, Segmented buttons | Horizontal stack. Max 3–4 items. |
| Card | design-components/card.md | Full | Panel, Tile, Surface, Container | Hover shadow web. No hover on mobile. |
| Carousel | design-components/carousel.md | Stub | Slideshow, Image slider, Scroll strip | Web: Embla. Mobile: FlatList horizontal. |
| Checkbox | design-components/checkbox.md | Full | Multi-select, Check | Indeterminate state required. |
| Color Picker | design-components/color-picker.md | Stub | Color swatch, Hue selector | Web only in v2. |
| Combobox | design-components/combobox.md | Full | Autocomplete, Searchable select, Typeahead | Debounced. Mobile: modal search. |
| Date Input | design-components/date-input.md | Stub | Date field, Date text input | Plain text input with date format hint. |
| Datepicker | design-components/datepicker.md | Full | Calendar picker, Date selector | Web: calendar. iOS: wheel. Android: material. |
| Drawer | design-components/drawer.md | Full | Side panel, Slide-over, Sheet, Bottom sheet | Web: side panel. Mobile: @gorhom/bottom-sheet. |
| Dropdown Menu | design-components/dropdown-menu.md | Full | Context menu, Action menu, Menu | role="menu". Escape closes. |
| Empty State | design-components/empty-state.md | Full | Zero state, Blank state, No content | Always include CTA. Never blank. |
| Fieldset | design-components/fieldset.md | Stub | Form group, Input group, Field group | Groups related inputs. Use legend. |
| File | design-components/file.md | Stub | File item, Attachment display | Displays a single attached file with metadata. |
| File Upload | design-components/file-upload.md | Full | Dropzone, Upload area, Attachment | Drag-drop web. Document picker mobile. |
| Footer | design-components/footer.md | Stub | Page footer, Site footer | Web only. Links + legal copy. |
| Form | design-components/form.md | Full | Form container, Field set | Single-column. Labels above. Progressive disclosure. |
| Header | design-components/header.md | Stub | App header, Page header, Navbar | Web top bar. Mobile navigation header. |
| Heading | design-components/heading.md | Stub | Title, Section heading, H1-H6 | Semantic heading levels. One H1 per screen. |
| Hero | design-components/hero.md | Stub | Banner, Splash, Above the fold | Landing/marketing sections. Not in app shell. |
| Icon | design-components/icon.md | Stub | Glyph, Pictogram | Lucide React. 16/20/24px. cv-text-secondary default. |
| Image | design-components/image.md | Stub | Photo, Illustration, Picture | next/image web. expo Image mobile. Alt required. |
| Label | design-components/label.md | Stub | Form label, Input label, Field label | htmlFor pairing. Never placeholder-only. |
| Link | design-components/link.md | Stub | Anchor, Inline link, Text link | cv-text-link. Underline on hover. |
| List | design-components/list.md | Stub | Unordered list, Ordered list, Item list | ul/ol web. FlatList mobile. |
| Modal | design-components/modal.md | Full | Dialog, Overlay, Popup, Alert dialog | Focus trap. Escape closes. iOS pageSheet. Android bottom sheet. |
| Navigation | design-components/navigation.md | Full | Nav bar, Sidebar, Tab bar, Menu | Web sidebar. Mobile bottom tab (max 5). |
| Pagination | design-components/pagination.md | Stub | Page controls, Page numbers | Web only. Mobile uses infinite scroll. |
| Popover | design-components/popover.md | Full | Floating panel, Tooltip-like, Inline card | Click-triggered. Mobile → bottom sheet at small sizes. |
| Progress Bar | design-components/progress-bar.md | Stub | Loading bar, Progress track | Horizontal. Determinate only. |
| Progress Indicator | design-components/progress-indicator.md | Full | Step indicator, Wizard progress, Onboarding steps | Steps/dots. Current/completed/upcoming. |
| Quote | design-components/quote.md | Stub | Blockquote, Pull quote, Citation | Teal left border. cv-bg-surface-sunken bg. |
| Radio Button | design-components/radio-button.md | Full | Radio, Option selector, Single select | Teal fill. role="radio". Always in group. |
| Rating | design-components/rating.md | Stub | Star rating, Score, Review stars | Read-only or interactive. 1–5 stars. |
| Rich Text Editor | design-components/rich-text-editor.md | Stub | WYSIWYG, Text editor, Formatted input | Web: Tiptap. Mobile: plain Textarea. |
| Search Input | design-components/search-input.md | Full | Search bar, Search field, Filter input | Magnifier icon. Clear button. Debounce 300ms. |
| Segmented Control | design-components/segmented-control.md | Stub | Button tabs, Toggle group, Switch tabs | iOS: SegmentedControl. Web: button group. |
| Select | design-components/select.md | Full | Dropdown select, Picker, Single select | Web: custom. iOS: ActionSheet/wheel. Android: Material. |
| Separator | design-components/separator.md | Stub | Divider, Rule, HR | Horizontal or vertical. cv-border-default. |
| Skeleton | design-components/skeleton.md | Full | Loading placeholder, Shimmer, Ghost | Shapes match content. aria-busy="true". |
| Skip Link | design-components/skip-link.md | Stub | Skip to content, Accessibility bypass | Visually hidden until focused. Web only. |
| Slider | design-components/slider.md | Stub | Range input, Scrubber | Teal track fill. 44px touch target. |
| Spinner | design-components/spinner.md | Stub | Loading spinner, Activity indicator | Teal. Web: CSS animation. Mobile: ActivityIndicator. |
| Stack | design-components/stack.md | Stub | Layout stack, VStack, HStack | Layout primitive. Not a visible component. |
| Stepper | design-components/stepper.md | Stub | Numeric input, Increment/decrement | Plus/minus buttons around value. |
| Table | design-components/table.md | Full | Data table, Grid, Data list | Mobile: horizontal scroll or card view. |
| Tabs | design-components/tabs.md | Full | Tab bar, Tab panel, Tab list | Arrow keys. role="tablist". |
| Text Input | design-components/text-input.md | Full | Input, Text field, Input field | 16px mobile min. cv-bg-surface-sunken. |
| Textarea | design-components/textarea.md | Full | Multiline input, Text area, Message box | Resizable web. Optional char count. |
| Toast | design-components/toast.md | Full | Snackbar, Notification, Flash message | Auto-dismiss 4s. Max 2 stacked. aria-live. |
| Toggle | design-components/toggle.md | Full | Switch, On/off switch | role="switch". cv-bg-accent when on. Haptic mobile. |
| Tooltip | design-components/tooltip.md | Full | Hint, Helper text, Info popup | 500ms delay. Mobile: long-press or Popover. |
| Tree View | design-components/tree-view.md | Stub | Tree, Nested list, File tree | Expandable hierarchy. Keyboard nav. |
| Video | design-components/video.md | Stub | Video player, Media player | Web: HTML video. Mobile: expo-video. |
| Visually Hidden | design-components/visually-hidden.md | Stub | Screen reader only, SR-only, Accessible label | CSS clip. Never display:none. |
| Agent Builder Canvas | design-components/agent-builder-canvas.md | Full | Flow canvas, Node canvas, Workflow editor | Web only (v2). Mobile: read-only viz. |
| Agent Builder Node | design-components/agent-builder-node.md | Full | Flow node, Workflow node, Agent step | Color-coded by type. Drag handle. Connection ports. |
| AI Working Indicator | design-components/ai-working-indicator.md | Stub | Thinking indicator, AI loader, Processing state | Animated teal dots. aria-live="polite". |
| Channel Indicator | design-components/channel-indicator.md | Stub | Platform badge, Channel badge, Source indicator | WhatsApp/SMS/email color dots. |
| Conversation Thread | design-components/conversation-thread.md | Full | Chat thread, Message list, Chat view | react-virtual web. FlatList inverted mobile. |
| Dashboard Stat Card | design-components/dashboard-stat-card.md | Stub | KPI card, Metric card, Stat tile | Icon + label + value + trend. |


## Common Compositions

These are frequently built UI patterns composed from multiple components. Each entry lists the components to combine and the reference files to load.

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


## Platform-Specific Mapping

When building for iOS or Android, these web components map to platform-native equivalents. Always read each component's Platform Considerations section for implementation detail.

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
