
# Rich Text Editor

**Gallery reference:** https://component.gallery/components/rich-text-editor/

## When to Use

Use Rich Text Editor when users need to compose formatted content — agent message templates with bold/italic emphasis, knowledge base articles, and email body content. Use only where formatting genuinely adds value; plain textarea is preferred for simple single-paragraph inputs.

## Chatverce Styling

- Toolbar: `cv-bg-surface`, `cv-border-default` bottom border, `cv-space-2` padding
- Toolbar buttons: Icon-only, 28px, `cv-radius-sm`, active state `cv-bg-surface-elevated cv-text-accent`
- Editor area: `cv-bg-surface`, `cv-border-default` border, `cv-radius-md`, `cv-space-3` padding, min-height 120px
- Typography inside editor: matches `cv-text-body`, `cv-text-primary`
- Do not build from scratch — use Tiptap (web) or Slate.js as the foundation

## Accessibility

- The editor content area must have `role="textbox" aria-multiline="true"` and `aria-label` or `aria-labelledby`.
- Toolbar buttons: each requires `aria-label` and `aria-pressed` for toggle states (Bold, Italic, etc.).
- Keyboard: full keyboard editing is provided by the underlying library; verify Tab behavior does not trap focus.

## Code Example

```tsx
import { useEditor, EditorContent } from "@tiptap/react";
import StarterKit from "@tiptap/starter-kit";
import { Bold, Italic, List } from "lucide-react";

function RichTextEditor({ value, onChange }) {
  const editor = useEditor({
    extensions: [StarterKit],
    content: value,
    onUpdate: ({ editor }) => onChange(editor.getHTML()),
  });

  return (
    <div className="border border-[--cv-border-default] rounded-[--cv-radius-md] overflow-hidden">
      <div className="flex gap-1 p-2 border-b border-[--cv-border-default] bg-[--cv-bg-surface]">
        <button
          aria-label="Bold" aria-pressed={editor?.isActive("bold")}
          onClick={() => editor?.chain().focus().toggleBold().run()}
          className={`p-1.5 rounded-[--cv-radius-sm] ${editor?.isActive("bold") ? "bg-[--cv-bg-surface-elevated] text-[--cv-text-accent]" : "text-[--cv-text-secondary]"}`}
        >
          <Bold size={16} aria-hidden="true" />
        </button>
        <button
          aria-label="Italic" aria-pressed={editor?.isActive("italic")}
          onClick={() => editor?.chain().focus().toggleItalic().run()}
          className={`p-1.5 rounded-[--cv-radius-sm] ${editor?.isActive("italic") ? "bg-[--cv-bg-surface-elevated] text-[--cv-text-accent]" : "text-[--cv-text-secondary]"}`}
        >
          <Italic size={16} aria-hidden="true" />
        </button>
      </div>
      <EditorContent
        editor={editor}
        className="p-3 min-h-[120px] text-[--cv-text-primary] focus-within:outline-none"
      />
    </div>
  );
}
```
