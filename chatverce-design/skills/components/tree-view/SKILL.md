---
name: tree-view
description: Hierarchical nested list with expand/collapse nodes for displaying tree-structured data
user_invocable: false
---

# Tree View

**Gallery reference:** https://component.gallery/components/tree-view/
**Aliases:** Tree, Hierarchy list, File tree, Nested list

## When to Use

Use Tree View for presenting parent-child hierarchies where relationships between items are meaningful — folder structures, organizational hierarchies, conversation flow structures in the Chatverce agent builder sidebar. Tree View makes navigating deep nesting easier than flat lists.

## When NOT to Use

Do not use Tree View for flat lists — use a standard List. Do not use Tree View when nesting exceeds 4 levels in a UI context; it becomes impossible to navigate. Do not use Tree View for navigation menus — use Navigation or Accordion.

## Anatomy

- **Root list:** `role="tree"`.
- **Tree items:** `role="treeitem"`. Each has an `aria-expanded` if it has children.
- **Expand/collapse trigger:** Arrow/chevron icon. Rotates on expand.
- **Nested subtree:** `role="group"` containing child `treeitem` elements.
- **Selection indicator (optional):** `cv-bg-surface-elevated` or `cv-bg-accent` highlight on selected item.

## Variants

| Variant | Description |
|---------|-------------|
| Read-only | Expandable nodes, no selection. For browsing structures. |
| Selectable | Single item selection. Selected item highlighted with `cv-bg-surface-elevated` or `cv-text-accent`. |

## Chatverce Styling

- Item row: 32px height, `cv-space-2` horizontal padding, hover `cv-bg-surface-elevated`
- Selected: `cv-bg-surface-elevated`, `cv-text-primary`, weight 500
- Expand arrow: `ChevronRight` 14px, `cv-text-tertiary`, rotates 90° when expanded
- Indent per level: `cv-space-4` (16px) left padding per depth level
- Text: `cv-text-sm`, `cv-text-primary`
- Animation: `cv-duration-normal` on expand/collapse

## Platform Considerations

Web: Build with nested `<ul role="tree">` and `<li role="treeitem">`. Use Radix UI or a headless library for keyboard management. Mobile: Tree Views are uncommon on mobile — consider collapsing the hierarchy or using a drill-down navigation pattern (separate screens per level) instead.

## Accessibility

- `role="tree"` on the root `<ul>`.
- `role="treeitem"` on each `<li>`. `role="group"` on nested `<ul>` within an item.
- `aria-expanded="true|false"` on nodes with children.
- `aria-selected="true|false"` on selectable items.
- Keyboard: Arrow Right — expand / move to first child. Arrow Left — collapse / move to parent. Arrow Up/Down — move between visible items. Home/End — first/last visible item. Enter — select/activate.

## Code Example

```tsx
<ul role="tree" aria-label="Agent flows" className="space-y-0.5">
  <li role="treeitem" aria-expanded={isOpen} className="select-none">
    <div
      className="flex items-center gap-1 px-2 py-1.5 rounded-[--cv-radius-sm] hover:bg-[--cv-bg-surface-elevated] cursor-pointer"
      onClick={() => setIsOpen(!isOpen)}
    >
      <ChevronRight
        size={14}
        aria-hidden="true"
        className={`text-[--cv-text-tertiary] transition-transform duration-[--cv-duration-normal] ${isOpen ? "rotate-90" : ""}`}
      />
      <span className="text-sm text-[--cv-text-primary]">Customer Support</span>
    </div>
    {isOpen && (
      <ul role="group" className="ml-4 mt-0.5 space-y-0.5">
        <li role="treeitem" className="flex items-center gap-1 px-2 py-1.5 rounded-[--cv-radius-sm] hover:bg-[--cv-bg-surface-elevated] cursor-pointer">
          <span className="text-sm text-[--cv-text-primary]">Greeting flow</span>
        </li>
        <li role="treeitem" className="flex items-center gap-1 px-2 py-1.5 rounded-[--cv-radius-sm] hover:bg-[--cv-bg-surface-elevated] cursor-pointer">
          <span className="text-sm text-[--cv-text-primary]">Escalation flow</span>
        </li>
      </ul>
    )}
  </li>
</ul>
```

## Anti-Patterns

- Never use `role="tree"` without full keyboard navigation support — it creates an unusable widget for keyboard users.
- Never nest more than 4 levels without considering a drill-down alternative.
- Never use Tree View for primary navigation — use Navigation instead.
