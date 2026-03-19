---
name: empty-state
description: Purposeful placeholder shown when a list or content area has no data — always inviting, never blank
user_invocable: false
---

# Empty State

**Gallery reference:** https://component.gallery/components/empty-state/
**Aliases:** Zero state, Blank state, No content, No results, Null state

## When to Use

Use Empty State every time a list, table, inbox, or content area would otherwise show nothing. A blank area without explanation communicates brokenness — users cannot tell whether data is loading, missing, or whether they need to take action. Empty State makes the absence of data intentional and inviting.

## When NOT to Use

Do not use Empty State during loading — use Skeleton instead (content is coming). Do not use Empty State for error conditions — use Alert or an error state with a retry action. Do not use a generic "No data" message without context — every empty state in Chatverce must be screen-specific.

## Anatomy

- **Illustration (optional):** Simple, friendly SVG or icon set. 80–120px. Not generic clipart.
- **Headline:** H3, `cv-text-primary`. Short and inviting — names the absent thing and sets expectation.
- **Body copy (optional):** Body, `cv-text-secondary`. 1–2 sentences. Explains what goes here and why it's empty.
- **CTA button (optional):** Primary or Secondary button. The one action that creates or finds the first item.

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Default | Illustration + headline + body + CTA | First-run empty screens — agent list, conversation inbox |
| Search empty | Magnifier illustration + "no results" + clear search | Search/filter returned no results |
| Error empty | Alert icon + "something went wrong" + retry | Data failed to load |
| Compact | Icon + headline only, no illustration | Small panels, table cells, sidebar sections |
| Instructional | Illustration + step-by-step hint + CTA | Complex setup (agent builder canvas when empty) |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | Static, centered | Welcoming, clear |
| CTA hover (web) | Button hover state | Ready to begin |
| CTA pressed | Button pressed state | Action taken |

## Chatverce Styling

- Container: centered vertically and horizontally within the parent area, `cv-bg-primary` or `cv-bg-surface` parent
- Illustration: `cv-text-secondary` tint or full-color spot illustration
- Headline: `cv-text-primary`, H3 (16–18px, Medium)
- Body: `cv-text-secondary`, Body (14–16px, Regular), max-width 320px
- CTA: Primary button (accent) for first-run, Secondary for search-empty
- Spacing: 16px between illustration and headline, 8px between headline and body, 24px between body and CTA

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Center the empty state with flexbox. Use `min-h-[300px]` or fill the parent's height with `h-full`. SVG illustrations are preferred — they scale cleanly and can be themed via `currentColor`.

### iOS (Expo + NativeWind)
Use a `View` with `flex: 1, alignItems: 'center', justifyContent: 'center'`. Ensure the parent FlatList or ScrollView passes the empty state through `ListEmptyComponent`. This renders correctly inside both FlatList and SectionList.

### Android (Expo + NativeWind)
Same as iOS. Use `ListEmptyComponent` prop on FlatList. Ensure the empty state container has enough vertical padding to avoid being clipped at the top or bottom by system bars.

## Accessibility

- Web: The headline should be a proper heading element (h2 or h3) so screen readers can navigate to it. The CTA button must have a clear `aria-label` if the button text alone lacks context.
- Mobile: `accessibilityRole="header"` on the headline Text. CTA must have `accessibilityRole="button"` and `accessibilityLabel`.

## Copy Guidelines

Empty state copy follows Chatverce's voice: **warm, direct, and action-oriented**. Never say "No data found" or "Nothing to show" — these sound like error messages.

**Formulas that work:**
- "[Things] will show up here once [condition]."
- "You haven't [created/added/started] any [things] yet. [CTA action] to get started."
- "No [things] match your search. [Try different keywords / Clear filters]."

**Screen-specific examples:**
| Screen | Headline | Body | CTA |
|--------|---------|------|-----|
| Agent list | "No agents yet" | "Build your first AI agent and put it to work." | "Create Agent" |
| Conversation inbox | "No conversations yet" | "They'll show up here once your assistant is live." | "Publish Agent" |
| Contact list | "No contacts yet" | "Import contacts or let your agent collect them automatically." | "Import Contacts" |
| Search results | "No results for "{query}"" | "Try different keywords or clear the filters." | "Clear search" |

## Code Examples

### Web

```tsx
import { Button } from "@/components/ui/button";
import { PlusCircle } from "lucide-react";

// First-run empty state (agent list)
function EmptyAgentList({ onCreate }) {
  return (
    <div className="flex flex-col items-center justify-center min-h-[360px] text-center px-4">
      {/* Illustration placeholder — replace with actual SVG */}
      <div className="w-24 h-24 rounded-full bg-[--cv-bg-accent-subtle] flex items-center justify-center mb-4">
        <PlusCircle size={40} className="text-[--cv-bg-accent]" />
      </div>
      <h3 className="text-[--cv-text-primary] font-medium text-lg mb-2">No agents yet</h3>
      <p className="text-[--cv-text-secondary] text-sm max-w-xs mb-6">
        Build your first AI agent and put it to work across your customer channels.
      </p>
      <Button
        onClick={onCreate}
        className="bg-[--cv-bg-accent] text-[--cv-text-on-accent] rounded-[--cv-radius-md]"
      >
        Create Agent
      </Button>
    </div>
  );
}

// Search empty state
function EmptySearchResults({ query, onClear }) {
  return (
    <div className="flex flex-col items-center justify-center min-h-[200px] text-center px-4">
      <Search size={40} className="text-[--cv-text-tertiary] mb-3" />
      <h3 className="text-[--cv-text-primary] font-medium text-base mb-1">
        No results for "{query}"
      </h3>
      <p className="text-[--cv-text-secondary] text-sm mb-4">
        Try different keywords or clear the filters.
      </p>
      <Button variant="outline" onClick={onClear} className="border-[--cv-border-default]">
        Clear search
      </Button>
    </div>
  );
}
```

### Mobile

```tsx
import { View, Text, Pressable, FlatList } from "react-native";
import { PlusCircle } from "lucide-react-native";

// Used as ListEmptyComponent inside FlatList
function EmptyAgentListMobile({ onCreate }) {
  return (
    <View className="flex-1 items-center justify-center py-16 px-8">
      <View className="w-24 h-24 rounded-full bg-[--cv-bg-accent-subtle] items-center justify-center mb-4">
        <PlusCircle size={40} color="var(--cv-bg-accent)" />
      </View>
      <Text
        className="text-[--cv-text-primary] font-medium text-lg mb-2 text-center"
        accessibilityRole="header"
      >
        No agents yet
      </Text>
      <Text className="text-[--cv-text-secondary] text-sm text-center mb-6">
        Build your first AI agent and put it to work across your customer channels.
      </Text>
      <Pressable
        className="bg-[--cv-bg-accent] rounded-[--cv-radius-md] min-h-[44px] px-6 items-center justify-center"
        onPress={onCreate}
        accessibilityRole="button"
        accessibilityLabel="Create your first agent"
      >
        <Text className="text-[--cv-text-on-accent] font-medium">Create Agent</Text>
      </Pressable>
    </View>
  );
}

// Usage:
<FlatList
  data={agents}
  keyExtractor={(item) => item.id}
  renderItem={({ item }) => <AgentCard agent={item} />}
  ListEmptyComponent={<EmptyAgentListMobile onCreate={handleCreate} />}
/>
```

## Anti-Patterns

- Never leave a content area completely blank — always show an Empty State.
- Never use a generic "No data" message — write copy specific to the screen and what the user should do.
- Never show Empty State while content is loading — show Skeleton until data resolves, then show Empty State if it's actually empty.
- Never omit a CTA unless the user truly cannot take action (read-only view, no permissions) — in that case, explain why and who can help.
- Never use error-style copy ("Nothing found", "Failed to load") for a normal empty state — tone communicates state. Reserve clinical language for actual errors.
