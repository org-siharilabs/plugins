---
name: design-components
description: "66 Chatverce UI components with specs, code examples, and platform considerations. Use when building or reviewing UI components for web (Next.js/Tailwind/shadcn) or mobile (Expo/NativeWind)."
user_invocable: false
---

# Chatverce Component Library

66 components across three tiers. Each component has a dedicated reference file with anatomy, variants, styling, accessibility, and code examples for both web and mobile.

## How to Use

1. **Find the component** in the catalog below or in [catalog.md](catalog.md) for the full table with aliases and platform notes
2. **Load the reference file** — e.g., for Button, read [button.md](button.md)
3. **Use `cv-*` tokens only** — no hardcoded hex, spacing, or shadow values
4. **Check platform considerations** — web and mobile often differ (e.g., modals → drawers on mobile)

## Component Tiers

### Full Tier (31 components) — Complete specs with web + mobile code examples

| Component | File | Category |
|-----------|------|----------|
| Button | [button.md](button.md) | Interactive |
| Card | [card.md](card.md) | Layout |
| Modal | [modal.md](modal.md) | Overlay |
| Drawer | [drawer.md](drawer.md) | Overlay |
| Toast | [toast.md](toast.md) | Feedback |
| Alert | [alert.md](alert.md) | Feedback |
| Empty State | [empty-state.md](empty-state.md) | Feedback |
| Form | [form.md](form.md) | Input |
| Text Input | [text-input.md](text-input.md) | Input |
| Textarea | [textarea.md](textarea.md) | Input |
| Select | [select.md](select.md) | Input |
| Checkbox | [checkbox.md](checkbox.md) | Input |
| Radio Button | [radio-button.md](radio-button.md) | Input |
| Toggle | [toggle.md](toggle.md) | Input |
| Combobox | [combobox.md](combobox.md) | Input |
| Datepicker | [datepicker.md](datepicker.md) | Input |
| File Upload | [file-upload.md](file-upload.md) | Input |
| Search Input | [search-input.md](search-input.md) | Input |
| Navigation | [navigation.md](navigation.md) | Navigation |
| Tabs | [tabs.md](tabs.md) | Navigation |
| Dropdown Menu | [dropdown-menu.md](dropdown-menu.md) | Navigation |
| Popover | [popover.md](popover.md) | Overlay |
| Tooltip | [tooltip.md](tooltip.md) | Overlay |
| Table | [table.md](table.md) | Data Display |
| Skeleton | [skeleton.md](skeleton.md) | Data Display |
| Avatar | [avatar.md](avatar.md) | Data Display |
| Badge | [badge.md](badge.md) | Data Display |
| Progress Indicator | [progress-indicator.md](progress-indicator.md) | Data Display |
| Agent Builder Canvas | [agent-builder-canvas.md](agent-builder-canvas.md) | Chatverce |
| Agent Builder Node | [agent-builder-node.md](agent-builder-node.md) | Chatverce |
| Conversation Thread | [conversation-thread.md](conversation-thread.md) | Chatverce |

### Standard Tier (24 components) — Detailed specs with key examples

| Component | File |
|-----------|------|
| Accordion | [accordion.md](accordion.md) |
| Breadcrumbs | [breadcrumbs.md](breadcrumbs.md) |
| Button Group | [button-group.md](button-group.md) |
| Fieldset | [fieldset.md](fieldset.md) |
| Header | [header.md](header.md) |
| Footer | [footer.md](footer.md) |
| Heading | [heading.md](heading.md) |
| Hero | [hero.md](hero.md) |
| Icon | [icon.md](icon.md) |
| Image | [image.md](image.md) |
| Label | [label.md](label.md) |
| Link | [link.md](link.md) |
| List | [list.md](list.md) |
| Pagination | [pagination.md](pagination.md) |
| Progress Bar | [progress-bar.md](progress-bar.md) |
| Segmented Control | [segmented-control.md](segmented-control.md) |
| Separator | [separator.md](separator.md) |
| Slider | [slider.md](slider.md) |
| Spinner | [spinner.md](spinner.md) |
| Stepper | [stepper.md](stepper.md) |
| Tree View | [tree-view.md](tree-view.md) |
| AI Working Indicator | [ai-working-indicator.md](ai-working-indicator.md) |
| Channel Indicator | [channel-indicator.md](channel-indicator.md) |
| Dashboard Stat Card | [dashboard-stat-card.md](dashboard-stat-card.md) |

### Minimal Tier (11 components) — Essential styling and accessibility

| Component | File |
|-----------|------|
| Carousel | [carousel.md](carousel.md) |
| Color Picker | [color-picker.md](color-picker.md) |
| Date Input | [date-input.md](date-input.md) |
| File | [file.md](file.md) |
| Quote | [quote.md](quote.md) |
| Rating | [rating.md](rating.md) |
| Rich Text Editor | [rich-text-editor.md](rich-text-editor.md) |
| Skip Link | [skip-link.md](skip-link.md) |
| Stack | [stack.md](stack.md) |
| Video | [video.md](video.md) |
| Visually Hidden | [visually-hidden.md](visually-hidden.md) |

## Universal Rules

- All components use `cv-*` semantic tokens — never hardcoded values
- All interactive elements have 44px minimum touch targets on mobile
- All components support both light and dark themes via semantic tokens
- All text inputs have visible labels above (never floating labels)
- One primary CTA per screen/form
