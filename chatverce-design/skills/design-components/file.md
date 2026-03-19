
# File

**Gallery reference:** https://component.gallery/components/file/

## When to Use

Use the File component to display a file that has been uploaded or attached — in conversation threads (user-sent files, agent-sent documents), in agent knowledge base lists, or in form contexts after upload completes. It is a display component, not an upload trigger (see File Upload for that).

## Chatverce Styling

- Container: `cv-bg-surface-sunken`, `cv-radius-md`, `cv-border-default`, `cv-space-3` padding
- File icon: `File` or type-specific Lucide icon (e.g., `FileText`, `Image`), 20px, `cv-text-secondary`
- File name: `cv-text-sm`, `cv-text-primary`, weight 500, truncated with ellipsis
- File size: `cv-text-xs`, `cv-text-tertiary`
- Action buttons: `Download` and/or `X` (remove) icons, 16px, `cv-text-secondary`

## Accessibility

- The file container should be a `<div>` or `<article>` with an `aria-label` of `"[filename], [size]"`.
- Download button: `aria-label="Download [filename]"`.
- Remove button: `aria-label="Remove [filename]"`.
- For linked downloads, use `<a download>` with a descriptive `aria-label`.

## Code Example

```tsx
import { FileText, Download, X } from "lucide-react";

function FileItem({ name, size, onDownload, onRemove }) {
  return (
    <div
      aria-label={`${name}, ${size}`}
      className="flex items-center gap-3 px-3 py-2.5 bg-[--cv-bg-surface-sunken] border border-[--cv-border-default] rounded-[--cv-radius-md]"
    >
      <FileText size={20} aria-hidden="true" className="text-[--cv-text-secondary] flex-shrink-0" />
      <div className="flex-1 min-w-0">
        <p className="text-sm font-medium text-[--cv-text-primary] truncate">{name}</p>
        <p className="text-xs text-[--cv-text-tertiary]">{size}</p>
      </div>
      <button aria-label={`Download ${name}`} onClick={onDownload} className="text-[--cv-text-secondary] hover:text-[--cv-text-primary]">
        <Download size={16} />
      </button>
      {onRemove && (
        <button aria-label={`Remove ${name}`} onClick={onRemove} className="text-[--cv-text-secondary] hover:text-[--cv-text-destructive]">
          <X size={16} />
        </button>
      )}
    </div>
  );
}
```
