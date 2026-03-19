---
name: table
description: Structured data grid for listing, sorting, and acting on multiple records
user_invocable: false
---

# Table

**Gallery reference:** https://component.gallery/components/table/
**Aliases:** Data table, Grid, Data list, List table

## When to Use

Use Table for displaying structured records where the user needs to scan across multiple fields, sort by columns, or act on rows: agent list, conversation history, billing history, contact list, team members. Table is appropriate when comparing across rows matters and multiple attributes per record need to be visible simultaneously.

## When NOT to Use

Do not use Table for simple single-attribute lists — use a flat list with Separator. Do not use Table when rows are complex enough to deserve their own cards (rich preview, media) — use a Card grid instead. Do not use Table without an Empty State — a loading spinner over an invisible table is confusing.

## Anatomy

- **Container:** Full-width, `cv-bg-surface`, `cv-radius-md`, `cv-shadow-sm` (card style)
- **Header row:** `cv-bg-surface-sunken`, `cv-border-default` bottom, `cv-text-secondary` labels, Caption
- **Sortable header:** Arrow icon, `cv-text-accent` when sorted column
- **Data rows:** `cv-text-primary` (primary data), `cv-text-secondary` (secondary data), 48px row height, `cv-border-default` bottom
- **Row hover:** `cv-bg-surface-elevated` tint
- **Row selected:** `cv-bg-accent-subtle` tint
- **Action column:** Dropdown Menu trigger, right-aligned, visible on hover
- **Footer:** Pagination, result count, `cv-text-secondary`

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Default | Column headers + rows | Standard data listing |
| With row selection | Checkbox per row | Bulk actions |
| Sortable | Clickable column headers | Any table where ordering helps |
| With actions | Per-row action column | Edit/delete per item |
| Compact | 36px row height | Dense data views, settings tables |
| Mobile card view | Each row becomes a Card | Narrow screens, complex row data |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Loading | Skeleton rows | Content is coming |
| Empty | Empty State component | Zero data, clear next action |
| Row hover | cv-bg-surface-elevated | Scannable, interactive |
| Row selected | cv-bg-accent-subtle | Selected, acknowledged |
| Sorted column | cv-text-accent header, arrow icon | Orientation |
| Sort descending/ascending | Arrow direction | Visual sort confirmation |

## Chatverce Styling

- Container bg: `cv-bg-surface`
- Header bg: `cv-bg-surface-sunken`
- Header text: `cv-text-secondary`, Caption, uppercase optional
- Row height: 48px (default), 36px (compact)
- Row border: `cv-border-default` bottom
- Row hover: `cv-bg-surface-elevated`
- Row selected: `cv-bg-accent-subtle`
- Border radius: `cv-radius-md` on container (overflow hidden)
- Container shadow: `cv-shadow-sm`

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Table>` primitives. For sortable tables with large datasets, use `@tanstack/react-table` (TanStack Table) which provides sorting, filtering, pagination, and row selection. Wrap the table in an `overflow-x-auto` container for responsive horizontal scroll.

### iOS (Expo + NativeWind)
React Native has no `<table>` element. Options:
1. **Card view:** Render each row as a Card — appropriate for rich rows with 3–4 fields.
2. **Horizontal scroll table:** Use a `ScrollView` with `horizontal` containing a `FlatList`. This gives the table-like feel with horizontal scroll.
3. Use FlatList with `ListHeaderComponent` for the column headers.

### Android (Expo + NativeWind)
Same as iOS. The card view pattern is generally preferred on Android (Material design convention). Horizontal scroll tables are acceptable for data-dense screens.

## Accessibility

- Web: `<table>` with `<caption>` (visually hidden if needed). `<th scope="col">` for column headers. `<th scope="row">` for row headers if applicable. Sort headers include `aria-sort="ascending/descending/none"`. Selected rows have `aria-selected="true"`.
- Mobile: FlatList is not natively a table. Use `accessibilityRole` carefully. Each card/row should have an `accessibilityLabel` summarizing the row content.

## Code Examples

### Web

```tsx
import { Table, TableHeader, TableRow, TableHead, TableBody, TableCell } from "@/components/ui/table";
import { ChevronUp, ChevronDown } from "lucide-react";

function AgentsTable({ agents, sortField, sortDir, onSort }) {
  if (!agents.length) return <EmptyAgentList />;
  return (
    <div className="bg-[--cv-bg-surface] rounded-[--cv-radius-md] shadow-[--cv-shadow-sm] overflow-hidden">
      <div className="overflow-x-auto">
        <Table>
          <TableHeader>
            <TableRow className="bg-[--cv-bg-surface-sunken] border-b border-[--cv-border-default] hover:bg-[--cv-bg-surface-sunken]">
              {[{ key: "name", label: "Name" }, { key: "status", label: "Status" }, { key: "conversations", label: "Conversations" }].map(col => (
                <TableHead
                  key={col.key}
                  className="text-[--cv-text-secondary] text-xs font-medium py-3 px-4 cursor-pointer hover:text-[--cv-text-primary]"
                  onClick={() => onSort(col.key)}
                  aria-sort={sortField === col.key ? (sortDir === "asc" ? "ascending" : "descending") : "none"}
                >
                  <span className="flex items-center gap-1">
                    {col.label}
                    {sortField === col.key
                      ? (sortDir === "asc" ? <ChevronUp size={12} className="text-[--cv-text-accent]" /> : <ChevronDown size={12} className="text-[--cv-text-accent]" />)
                      : <ChevronUp size={12} className="opacity-30" />
                    }
                  </span>
                </TableHead>
              ))}
              <TableHead className="w-10" />
            </TableRow>
          </TableHeader>
          <TableBody>
            {agents.map(agent => (
              <TableRow
                key={agent.id}
                className="border-b border-[--cv-border-default] hover:bg-[--cv-bg-surface-elevated] group"
              >
                <TableCell className="py-3 px-4 text-[--cv-text-primary] text-sm font-medium">{agent.name}</TableCell>
                <TableCell className="py-3 px-4"><AgentStatusBadge status={agent.status} /></TableCell>
                <TableCell className="py-3 px-4 text-[--cv-text-secondary] text-sm">{agent.conversations}</TableCell>
                <TableCell className="py-3 px-4">
                  <div className="opacity-0 group-hover:opacity-100 transition-opacity">
                    <AgentActionsMenu agent={agent} />
                  </div>
                </TableCell>
              </TableRow>
            ))}
          </TableBody>
        </Table>
      </div>
    </div>
  );
}
```

### Mobile

```tsx
import { FlatList, View, Text, Pressable } from "react-native";

// Card-per-row pattern for mobile
function AgentsListMobile({ agents, onSelect }) {
  return (
    <FlatList
      data={agents}
      keyExtractor={item => item.id}
      ItemSeparatorComponent={() => <View className="h-2" />}
      contentContainerStyle={{ padding: 16 }}
      ListEmptyComponent={<EmptyAgentList />}
      renderItem={({ item }) => (
        <Pressable
          className="bg-[--cv-bg-surface] rounded-[--cv-radius-md] p-4 active:opacity-80"
          style={{ shadowColor: "#000", shadowOpacity: 0.05, shadowRadius: 4, elevation: 1 }}
          onPress={() => onSelect(item)}
          accessibilityRole="button"
          accessibilityLabel={`${item.name}, ${item.status}, ${item.conversations} conversations`}
        >
          <View className="flex-row items-center justify-between mb-1">
            <Text className="text-[--cv-text-primary] font-medium text-base">{item.name}</Text>
            <AgentStatusBadge status={item.status} />
          </View>
          <Text className="text-[--cv-text-secondary] text-sm">{item.conversations} conversations</Text>
        </Pressable>
      )}
    />
  );
}
```

## Anti-Patterns

- Never render a table without handling the empty state — a blank table looks broken.
- Never skip the loading/skeleton state — a table that appears instantly empty before data loads confuses users.
- Never put more than 6–7 columns in a table without making it horizontally scrollable.
- Never hide all row actions behind hover-only on mobile — mobile has no hover state.
- Never use a table on mobile for rows with more than 3–4 attributes without converting to card view.
