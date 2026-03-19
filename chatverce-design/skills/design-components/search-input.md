
# Search Input

**Gallery reference:** https://component.gallery/components/search-input/
**Aliases:** Search bar, Search field, Filter input, Query input

## When to Use

Use Search Input at the top of any list or table that the user needs to filter: agent list, conversation inbox, contact list, knowledge base documents. Search Input communicates immediately that the content below is searchable. Pair it with the Empty State component for zero-results scenarios.

## When NOT to Use

Do not use Search Input for a global site search (that is a separate global search pattern). Do not use Search Input inline inside a form alongside other form fields — it reads as a form input, not a filter control. Do not use Search Input if the list has fewer than ~8 items — filtering adds no value.

## Anatomy

- **Container:** Same as Text Input — `cv-bg-surface-sunken`, `cv-border-default`, `cv-radius-md`
- **Leading icon:** Magnifier (Search), 16px, `cv-text-secondary`, always present
- **Input text:** `cv-text-primary`, 14–16px
- **Placeholder:** "Search…" or "Search {thing type}…", `cv-text-tertiary`
- **Clear button:** X icon, 14px, `cv-text-secondary`, appears only when input has value
- **Loading indicator (optional):** Spinner replacing search icon while async results load

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Local filter | Filters an already-loaded list in-memory | Agent list, settings search, short lists |
| Async search | Debounced, fires API call | Conversation search, contact search, large datasets |
| With type filter | Search + filter dropdown combined | Multi-type content (search across agents + contacts) |
| Compact | Smaller height (32px web, 40px mobile) | Toolbar-embedded search, table header |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default (empty) | Magnifier, placeholder | Ready, inviting |
| Focus (empty) | cv-border-accent ring | Active, ready to type |
| Typing | Real-time filter after 300ms debounce | Responsive, alive |
| Has value | Clear (X) button appears | Input acknowledged, clearable |
| Loading (async) | Spinner replaces magnifier | Processing query |
| No results | List shows Empty State | Transparent, helpful |
| Cleared | Returns to default, list resets | Clean slate |

## Chatverce Styling

- Background: `cv-bg-surface-sunken`
- Border: `cv-border-default` → `cv-border-accent` focus
- Leading icon: magnifier, `cv-text-secondary`
- Clear icon: X, `cv-text-secondary`, only when value present
- Border radius: `cv-radius-md`
- Height: 40px web, 44px mobile
- Debounce: 300ms for async; local filter can be 0ms

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Build on shadcn/ui `<Input>` with a wrapper `<div>` for icon positioning. Use `useMemo` + `Array.filter` for local filtering. Use `useCallback` + `debounce` (from `lodash` or custom) for async search. Consider `useTransition` (React 18) to keep the input responsive while the list updates.

### iOS (Expo + NativeWind)
Use a `TextInput` wrapped in a styled View with the magnifier icon positioned absolutely. On iOS, the native `UISearchBar` is not available in React Native — use the custom View pattern. Consider using `expo-search-bar` or a custom styled TextInput.

### Android (Expo + NativeWind)
Same as iOS custom approach. Set `returnKeyType="search"` so the Android keyboard shows a search icon on the action button.

## Accessibility

- Web: `<input type="search">` — this gives browsers and screen readers the semantic "search" context. Use `role="searchbox"` if not using a native search input. The clear button must have `aria-label="Clear search"`. Associate a visually hidden label or use `aria-label="Search"` on the input itself.
- Mobile: `accessibilityRole="search"` on the TextInput. Clear button must have `accessibilityRole="button"` and `accessibilityLabel="Clear search"`. When search results update, use `accessibilityLiveRegion="polite"` on the result count.

## Code Examples

### Web

```tsx
import { useState, useCallback } from "react";
import { Search, X } from "lucide-react";
import { useDebouncedCallback } from "use-debounce";

function SearchInput({ onSearch, placeholder = "Search…", className = "" }) {
  const [value, setValue] = useState("");

  const debouncedSearch = useDebouncedCallback((query) => {
    onSearch(query);
  }, 300);

  const handleChange = (e) => {
    setValue(e.target.value);
    debouncedSearch(e.target.value);
  };

  const handleClear = () => {
    setValue("");
    onSearch("");
  };

  return (
    <div className={`relative flex items-center ${className}`}>
      <Search size={16} className="absolute left-3 text-[--cv-text-secondary] pointer-events-none" />
      <input
        type="search"
        value={value}
        onChange={handleChange}
        placeholder={placeholder}
        aria-label={placeholder}
        className="w-full bg-[--cv-bg-surface-sunken] border border-[--cv-border-default] rounded-[--cv-radius-md]
                   pl-9 pr-9 py-2 h-10 text-sm text-[--cv-text-primary] placeholder:text-[--cv-text-tertiary]
                   focus:outline-none focus:border-[--cv-border-accent] focus:ring-2 focus:ring-[--cv-border-accent]/20"
      />
      {value && (
        <button
          onClick={handleClear}
          className="absolute right-3 text-[--cv-text-secondary] hover:text-[--cv-text-primary]"
          aria-label="Clear search"
        >
          <X size={14} />
        </button>
      )}
    </div>
  );
}
```

### Mobile

```tsx
import { View, TextInput, Pressable } from "react-native";
import { useState } from "react";
import { Search, X } from "lucide-react-native";
import { useDebouncedCallback } from "use-debounce";

function SearchInputMobile({ onSearch, placeholder = "Search…" }) {
  const [value, setValue] = useState("");

  const debouncedSearch = useDebouncedCallback((query) => onSearch(query), 300);

  const handleChange = (text) => {
    setValue(text);
    debouncedSearch(text);
  };

  const handleClear = () => {
    setValue("");
    onSearch("");
  };

  return (
    <View className="flex-row items-center bg-[--cv-bg-surface-sunken] border border-[--cv-border-default] rounded-[--cv-radius-md] px-3 min-h-[44px] gap-2">
      <Search size={16} color="var(--cv-text-secondary)" />
      <TextInput
        value={value}
        onChangeText={handleChange}
        placeholder={placeholder}
        placeholderTextColor="var(--cv-text-tertiary)"
        returnKeyType="search"
        clearButtonMode="never"
        style={{ flex: 1, fontSize: 16, color: "var(--cv-text-primary)", paddingVertical: 10 }}
        accessibilityRole="search"
        accessibilityLabel={placeholder}
      />
      {value.length > 0 && (
        <Pressable
          onPress={handleClear}
          hitSlop={8}
          accessibilityRole="button"
          accessibilityLabel="Clear search"
        >
          <X size={14} color="var(--cv-text-secondary)" />
        </Pressable>
      )}
    </View>
  );
}
```

## Anti-Patterns

- Never fire a search API call on every keystroke — always debounce (300ms minimum).
- Never hide the clear button when the input has a value — users expect to be able to clear quickly.
- Never use a magnifier icon as the only affordance without a visible input field — the field itself must be clearly an input.
- Never fail silently on zero results — always show the Empty State component with the "No results for X" message.
- Never auto-focus the search input on page load unless the primary purpose of the entire screen is searching.
