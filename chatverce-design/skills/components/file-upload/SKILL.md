---
name: file-upload
description: Drag-and-drop (web) or document picker (mobile) control for attaching files to agent configuration or conversations
user_invocable: false
---

# File Upload

**Gallery reference:** https://component.gallery/components/file-upload/
**Aliases:** Dropzone, Upload area, Attachment, File picker, Document upload

## When to Use

Use File Upload for any flow that requires the user to attach a document: uploading a knowledge base file to an agent, attaching a media file to a message, importing a contact list CSV. It provides visual affordance for drag-and-drop on web and a native document picker on mobile.

## When NOT to Use

Do not use File Upload when a URL input would serve just as well (video embeds, external links). Do not use a bare `<input type="file">` without the styled dropzone — the default browser input is inconsistent and unstylable. Do not use File Upload for profile photos — use a dedicated avatar upload pattern with crop.

## Anatomy

- **Drop zone:** Dashed `cv-border-default` border, `cv-bg-surface-sunken`, `cv-radius-md`, centered content
- **Icon:** Upload cloud or paperclip icon, `cv-text-secondary`, 32px
- **Primary text:** "Drag files here or click to browse", `cv-text-primary`, Body
- **Secondary text:** Accepted types + size limit, `cv-text-secondary`, Caption
- **Active drag state:** `cv-border-accent` dashed border, `cv-bg-accent-subtle` fill
- **File list:** Below zone, each file shows name + size + remove button + progress bar
- **Progress bar:** Thin teal bar per file showing upload progress

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Default (dropzone) | Full drag-drop zone with click-to-browse | Knowledge base upload, media upload |
| Compact | Small button-like trigger "Attach file" | Inline within a form or chat input |
| Multi-file | Accepts multiple files, shows file list | Batch document upload |
| Single file | Replaces on reselect | Profile doc, single import file |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | Dashed border, light bg | Open, ready |
| Hover (web) | cv-border-accent dashed, cv-bg-accent-subtle | Inviting |
| Drag-over | cv-border-accent dashed, cv-bg-accent-subtle, scale 101% | Active drag target |
| Uploading | Progress bar per file, cancel button | Transparent, in progress |
| Upload complete | Green check per file, remove button | Done, successful |
| Error | cv-border-error, error message per file | Issue, actionable |
| Disabled | Muted zone, cursor not-allowed | Not available |

## Chatverce Styling

- Drop zone border: dashed, `cv-border-default` rest, `cv-border-accent` drag-over
- Drop zone bg: `cv-bg-surface-sunken` rest, `cv-bg-accent-subtle` drag-over
- Border radius: `cv-radius-md`
- Progress bar: `cv-bg-accent`, 4px height
- File list item: `cv-bg-surface`, `cv-radius-sm`, padding 8px 12px
- Error: `cv-text-error`, `cv-border-error`

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use `react-dropzone` (`useDropzone` hook). It handles drag events, click-to-browse, and file type / size validation. Pair with a presigned URL upload to S3 or Supabase Storage for actual file handling. Show individual file progress via XHR or fetch with progress events.

### iOS (Expo + NativeWind)
Use `expo-document-picker` (`DocumentPicker.getDocumentAsync`). For images/video, use `expo-image-picker`. Display selected files in a FlatList below the trigger button. Show upload progress via the Chatverce API upload endpoint.

### Android (Expo + NativeWind)
Same as iOS. `expo-document-picker` and `expo-image-picker` handle the Android file/media selection dialogs. Ensure `READ_EXTERNAL_STORAGE` permission is handled via `expo-permissions` or `expo-media-library`.

## Accessibility

- Web: The drop zone must be keyboard-accessible — a `<button>` inside the zone that opens the file dialog on Enter/Space. `aria-label="Upload files"` on the trigger. File status updates use `aria-live="polite"` to announce upload completion or errors.
- Mobile: The trigger Pressable must have `accessibilityRole="button"` and `accessibilityLabel="Attach file"`. Upload progress is announced via `accessibilityLiveRegion="polite"`.

## Code Examples

### Web

```tsx
import { useCallback } from "react";
import { useDropzone } from "react-dropzone";
import { UploadCloud, X, CheckCircle2 } from "lucide-react";

function FileUploadZone({ onFilesSelected, accept, maxSize = 10 * 1024 * 1024 }) {
  const onDrop = useCallback((acceptedFiles) => onFilesSelected(acceptedFiles), [onFilesSelected]);

  const { getRootProps, getInputProps, isDragActive, isDragReject } = useDropzone({
    onDrop, accept, maxSize, multiple: true,
  });

  return (
    <div
      {...getRootProps()}
      className={`
        rounded-[--cv-radius-md] border-2 border-dashed p-8 text-center cursor-pointer transition-all
        ${isDragActive
          ? "border-[--cv-border-accent] bg-[--cv-bg-accent-subtle]"
          : isDragReject
          ? "border-[--cv-border-error] bg-[--cv-bg-destructive-subtle]"
          : "border-[--cv-border-default] bg-[--cv-bg-surface-sunken] hover:border-[--cv-border-accent] hover:bg-[--cv-bg-accent-subtle]"
        }
      `}
    >
      <input {...getInputProps()} aria-label="Upload files" />
      <UploadCloud size={32} className="mx-auto mb-3 text-[--cv-text-secondary]" />
      <p className="text-[--cv-text-primary] text-sm font-medium">
        {isDragActive ? "Drop files here…" : "Drag files here or click to browse"}
      </p>
      <p className="text-[--cv-text-secondary] text-xs mt-1">PDF, DOCX, TXT up to 10MB</p>
    </div>
  );
}
```

### Mobile

```tsx
import { View, Text, Pressable, FlatList } from "react-native";
import * as DocumentPicker from "expo-document-picker";
import { Paperclip, X, CheckCircle } from "lucide-react-native";

function FileUploadMobile({ onFilesSelected, allowedTypes = ["application/pdf", "text/plain"] }) {
  const [files, setFiles] = useState([]);

  const handlePick = async () => {
    const result = await DocumentPicker.getDocumentAsync({
      type: allowedTypes,
      copyToCacheDirectory: true,
      multiple: true,
    });
    if (!result.canceled && result.assets) {
      const newFiles = result.assets;
      setFiles(prev => [...prev, ...newFiles]);
      onFilesSelected(newFiles);
    }
  };

  return (
    <View className="mb-5">
      <Pressable
        className="flex-row items-center justify-center gap-2 border-2 border-dashed border-[--cv-border-default] rounded-[--cv-radius-md] bg-[--cv-bg-surface-sunken] py-6"
        onPress={handlePick}
        accessibilityRole="button"
        accessibilityLabel="Attach files — tap to browse"
      >
        <Paperclip size={20} color="var(--cv-text-secondary)" />
        <Text className="text-[--cv-text-primary] text-sm font-medium">Attach files</Text>
      </Pressable>
      <Text className="text-[--cv-text-secondary] text-xs mt-1 text-center">PDF, DOCX, TXT up to 10MB</Text>

      {files.length > 0 && (
        <FlatList
          data={files}
          className="mt-3"
          keyExtractor={(item, i) => `${item.name}-${i}`}
          renderItem={({ item, index }) => (
            <View className="flex-row items-center bg-[--cv-bg-surface] rounded-[--cv-radius-sm] px-3 py-2 mb-2 gap-3">
              <CheckCircle size={16} color="var(--cv-status-success)" />
              <View className="flex-1">
                <Text className="text-[--cv-text-primary] text-sm" numberOfLines={1}>{item.name}</Text>
                <Text className="text-[--cv-text-secondary] text-xs">{(item.size / 1024).toFixed(0)} KB</Text>
              </View>
              <Pressable
                onPress={() => setFiles(prev => prev.filter((_, i) => i !== index))}
                accessibilityRole="button"
                accessibilityLabel={`Remove ${item.name}`}
              >
                <X size={16} color="var(--cv-text-tertiary)" />
              </Pressable>
            </View>
          )}
        />
      )}
    </View>
  );
}
```

## Anti-Patterns

- Never use a plain `<input type="file">` without a styled wrapper — the default browser control is unstyled and inaccessible.
- Never upload immediately on file drop without showing progress — the user has no feedback that anything is happening.
- Never accept all file types without restriction — always define `accept` constraints and explain them in the UI.
- Never omit the remove button — users make mistakes and need to replace files.
- Never show a single failure status for a multi-file upload — show per-file status so the user knows which files need attention.
