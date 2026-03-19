---
name: pagination
description: Navigation controls for moving between pages of a dataset, optimized for non-technical users
user_invocable: false
---

# Pagination

**Gallery reference:** https://component.gallery/components/pagination/
**Aliases:** Pager, Page navigation, Next/Prev

## When to Use

Use Pagination when a dataset is too large to display on a single screen and splitting into discrete pages makes navigation manageable. Ideal for conversation lists, agent audit logs, and search results in the Chatverce web app where users can orient by "page X of Y".

## When NOT to Use

Do not use Pagination on mobile — prefer infinite scroll or "Load more" for touch interfaces where discrete page navigation is awkward. Do not use Pagination for fewer than 2 pages. Do not add Pagination to real-time feeds where content changes while the user reads.

## Anatomy

- **Previous button:** `<button>` or `<a>`. `ChevronLeft` icon + "Previous" label (or icon-only with `aria-label`).
- **Page indicators:** Current page (bold, `cv-bg-accent` or `cv-text-accent`) and adjacent page numbers as links.
- **Next button:** `ChevronRight` icon + "Next" label.
- **Count text (optional):** "Page 3 of 12" as an `aria-live` region.
- **Nav wrapper:** `<nav aria-label="Pagination">`.

## Variants

| Variant | Description |
|---------|-------------|
| Simple | Previous / Page X of Y / Next. Best for non-technical users. Clear and unambiguous. |
| Numbered | Previous + page number links + ellipsis + Next. For power users and large datasets. |

## Chatverce Styling

- Current page: `cv-bg-accent`, `cv-text-on-accent`, `cv-radius-md`
- Adjacent page links: `cv-text-secondary`, hover `cv-bg-surface-elevated`
- Previous/Next: `cv-text-primary`, disabled state `cv-text-tertiary`
- Disabled buttons: `pointer-events-none`, `cv-text-tertiary`
- Container: `flex items-center gap-1`, centered or right-aligned

## Platform Considerations

Web: Use `<nav aria-label="Pagination">` wrapping `<a>` or `<button>` elements. Mobile: Replace with infinite scroll or a "Load more" button — do not port pagination controls to mobile. If page count must be shown, use a simple text indicator only.

## Accessibility

- Wrap in `<nav aria-label="Pagination">`.
- Current page: `aria-current="page"` on the active page element.
- Previous/Next when disabled: `aria-disabled="true"` (not HTML `disabled`, to keep focusable for screen readers).
- Announce page changes: wrap the count text in `aria-live="polite"`.

## Code Example

```tsx
<nav aria-label="Pagination" className="flex items-center gap-1">
  <button
    aria-label="Previous page"
    aria-disabled={currentPage === 1}
    className="flex items-center gap-1 px-3 py-2 text-sm text-[--cv-text-primary] rounded-[--cv-radius-md] hover:bg-[--cv-bg-surface-elevated] disabled:text-[--cv-text-tertiary] disabled:pointer-events-none"
    onClick={() => setPage(p => p - 1)}
  >
    <ChevronLeft size={16} aria-hidden="true" />
    Previous
  </button>

  <span aria-live="polite" className="text-sm text-[--cv-text-secondary] px-3">
    Page {currentPage} of {totalPages}
  </span>

  <button
    aria-label="Next page"
    aria-disabled={currentPage === totalPages}
    className="flex items-center gap-1 px-3 py-2 text-sm text-[--cv-text-primary] rounded-[--cv-radius-md] hover:bg-[--cv-bg-surface-elevated] disabled:text-[--cv-text-tertiary] disabled:pointer-events-none"
    onClick={() => setPage(p => p + 1)}
  >
    Next
    <ChevronRight size={16} aria-hidden="true" />
  </button>
</nav>
```

## Anti-Patterns

- Never use pagination on mobile — use infinite scroll or "Load more".
- Never disable Previous/Next without providing a visual cue explaining why (e.g., greyed text).
- Never show only icon arrows without text labels for non-technical users — "Previous" / "Next" text is required.
