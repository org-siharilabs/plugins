---
name: combobox
description: Searchable input + dropdown that filters a large list as the user types
user_invocable: false
---

# Combobox

**Gallery reference:** https://component.gallery/components/combobox/
**Aliases:** Autocomplete, Searchable select, Typeahead, Autocomplete input

## When to Use

Use Combobox when the user needs to select from a list that is too long to scan visually (15+ options), or when the user should be able to search/filter while selecting: assigning a contact tag from a long list, selecting a timezone, choosing a language, assigning a team member from a large team. Combobox reduces cognitive load on long lists by letting the user narrow options by typing.

## When NOT to Use

Do not use Combobox for fewer than ~8 options — Select is simpler. Do not use Combobox for free-form input where any value is valid — use Text Input. Do not use Combobox for real-time API search with very high latency — consider a dedicated search pattern instead.

## Anatomy

- **Input area:** `cv-bg-surface-sunken`, `cv-border-default`, `cv-radius-md` — same as Text Input
- **Leading search icon:** Magnifier, `cv-text-secondary`
- **Typed text:** `cv-text-primary`, Body (16px mobile)
- **Dropdown panel:** `cv-bg-surface-elevated`, `cv-shadow-md`, `cv-radius-md`
- **Option items:** `cv-text-primary`, selected = check + `cv-text-accent`
- **Empty results message:** `cv-text-secondary`, "No results for…"
- **Loading indicator:** Spinner inside dropdown while fetching

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Single select | Choose one option | Timezone, language, category |
| Multi select | Choose multiple, shows chips/badges | Tags, assignees, channels |
| Async | Fetches options from API as user types | Large datasets, contact search |
| Grouped | Options organized in sections | Country/region, skill categories |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default (closed) | Input resting | Stable, ready |
| Open (empty query) | Dropdown shows all or recent options | Browsable |
| Typing | Dropdown filters in real-time, 300ms debounce | Responsive |
| Loading (async) | Spinner in dropdown | Processing |
| No results | Empty state message in dropdown | Transparent, helpful |
| Option selected | Value in input, dropdown closes | Done |
| Focus | cv-border-accent ring | Keyboard-accessible |

## Chatverce Styling

- Input: same as Text Input — `cv-bg-surface-sunken`, `cv-border-default` → `cv-border-accent`
- Dropdown: `cv-bg-surface-elevated`, `cv-shadow-md`, `cv-radius-md`
- Selected option check: `cv-text-accent`
- Debounce: 300ms on keystrokes before filtering/fetching
- Max visible options without scroll: 6 items

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Combobox>` (Command-based pattern using `cmdk`). The `<Command>` component provides filtering, keyboard navigation, and ARIA automatically.

### iOS (Expo + NativeWind)
Present the combobox as a full-screen or half-screen modal: a TextInput at the top for search + a FlatList of results below. Use `autoFocus` on the TextInput so the keyboard appears immediately. Dismiss with back gesture or a "Done" button.

### Android (Expo + NativeWind)
Same pattern as iOS — modal search screen. Android users expect search-in-modal for long list selection.

## Accessibility

- Web: `role="combobox"`, `aria-expanded`, `aria-autocomplete="list"`, `aria-controls` pointing to listbox. Each option has `role="option"`, `aria-selected`. Keyboard: arrow keys navigate list, Enter selects, Escape closes.
- Mobile: The search TextInput has `accessibilityLabel`. The FlatList items have `accessibilityRole="menuitem"` or `"button"`. Selected item announces with `accessibilityLiveRegion="polite"`.

## Code Examples

### Web

```tsx
import { useState } from "react";
import { Check, ChevronsUpDown } from "lucide-react";
import { Command, CommandEmpty, CommandGroup, CommandInput, CommandItem, CommandList } from "@/components/ui/command";
import { Popover, PopoverContent, PopoverTrigger } from "@/components/ui/popover";
import { Button } from "@/components/ui/button";

const timezones = [
  { value: "utc", label: "UTC +0:00" },
  { value: "america-new_york", label: "America/New York (UTC -5)" },
  { value: "europe-london", label: "Europe/London (UTC +0/+1)" },
  // …more options
];

function TimezoneCombobox({ value, onChange }) {
  const [open, setOpen] = useState(false);
  const selectedLabel = timezones.find(t => t.value === value)?.label;

  return (
    <Popover open={open} onOpenChange={setOpen}>
      <PopoverTrigger asChild>
        <Button
          role="combobox"
          aria-expanded={open}
          className="w-full justify-between bg-[--cv-bg-surface-sunken] border-[--cv-border-default] rounded-[--cv-radius-md] text-left font-normal h-10"
        >
          <span className={selectedLabel ? "text-[--cv-text-primary]" : "text-[--cv-text-tertiary]"}>
            {selectedLabel || "Select timezone…"}
          </span>
          <ChevronsUpDown size={16} className="text-[--cv-text-secondary] shrink-0" />
        </Button>
      </PopoverTrigger>
      <PopoverContent className="w-full p-0 bg-[--cv-bg-surface-elevated] shadow-[--cv-shadow-md] rounded-[--cv-radius-md]">
        <Command>
          <CommandInput placeholder="Search timezones…" className="text-[--cv-text-primary]" />
          <CommandList>
            <CommandEmpty className="text-[--cv-text-secondary] text-sm py-3 text-center">No timezone found.</CommandEmpty>
            <CommandGroup>
              {timezones.map(tz => (
                <CommandItem
                  key={tz.value}
                  value={tz.value}
                  onSelect={(val) => { onChange(val); setOpen(false); }}
                  className="text-[--cv-text-primary] aria-selected:bg-[--cv-bg-surface]"
                >
                  <Check size={16} className={`mr-2 text-[--cv-text-accent] ${value === tz.value ? "opacity-100" : "opacity-0"}`} />
                  {tz.label}
                </CommandItem>
              ))}
            </CommandGroup>
          </CommandList>
        </Command>
      </PopoverContent>
    </Popover>
  );
}
```

### Mobile

```tsx
import { Modal, View, Text, TextInput, FlatList, Pressable } from "react-native";
import { useState, useMemo } from "react";
import { Check, ChevronDown } from "lucide-react-native";

function CvCombobox({ label, options, value, onChange }) {
  const [open, setOpen] = useState(false);
  const [query, setQuery] = useState("");

  const filtered = useMemo(() =>
    options.filter(o => o.label.toLowerCase().includes(query.toLowerCase())),
    [options, query]
  );

  const selectedLabel = options.find(o => o.value === value)?.label;

  return (
    <View className="mb-5">
      <Text className="text-[--cv-text-primary] text-sm font-medium mb-1.5">{label}</Text>
      <Pressable
        className="flex-row items-center justify-between bg-[--cv-bg-surface-sunken] border border-[--cv-border-default] rounded-[--cv-radius-md] px-4 min-h-[44px]"
        onPress={() => setOpen(true)}
        accessibilityRole="combobox"
        accessibilityLabel={`${label}: ${selectedLabel || "not selected"}`}
      >
        <Text style={{ color: selectedLabel ? "var(--cv-text-primary)" : "var(--cv-text-tertiary)", fontSize: 16 }}>
          {selectedLabel || "Select…"}
        </Text>
        <ChevronDown size={16} color="var(--cv-text-secondary)" />
      </Pressable>

      <Modal visible={open} animationType="slide" presentationStyle="pageSheet">
        <View className="flex-1 bg-[--cv-bg-surface]">
          <View className="px-4 pt-4 pb-2 border-b border-[--cv-border-default]">
            <TextInput
              autoFocus
              value={query}
              onChangeText={setQuery}
              placeholder={`Search ${label.toLowerCase()}…`}
              placeholderTextColor="var(--cv-text-tertiary)"
              style={{ fontSize: 16, color: "var(--cv-text-primary)", padding: 12,
                backgroundColor: "var(--cv-bg-surface-sunken)", borderRadius: 8 }}
              accessibilityLabel={`Search ${label}`}
            />
          </View>
          <FlatList
            data={filtered}
            keyExtractor={item => item.value}
            renderItem={({ item }) => (
              <Pressable
                className="flex-row items-center px-4 min-h-[48px] border-b border-[--cv-border-default]"
                onPress={() => { onChange(item.value); setOpen(false); setQuery(""); }}
                accessibilityRole="button"
                accessibilityLabel={item.label}
              >
                <View className="w-6 mr-3">
                  {value === item.value && <Check size={16} color="var(--cv-text-accent)" />}
                </View>
                <Text className="text-[--cv-text-primary] text-base">{item.label}</Text>
              </Pressable>
            )}
            ListEmptyComponent={
              <View className="py-12 items-center">
                <Text className="text-[--cv-text-secondary] text-sm">No results for "{query}"</Text>
              </View>
            }
          />
        </View>
      </Modal>
    </View>
  );
}
```

## Anti-Patterns

- Never skip the debounce on async comboboxes — firing an API call on every keystroke causes server load and jitter.
- Never use Combobox for fewer than ~8 options — Select is simpler and requires less cognitive overhead.
- Never allow the dropdown to open without keyboard or pointer focus on the input.
- Never let an empty combobox dropdown be a blank white box — always show "No results" or "Start typing to search".
- Never omit the selected state indicator (check icon) — users need to know their current selection.
