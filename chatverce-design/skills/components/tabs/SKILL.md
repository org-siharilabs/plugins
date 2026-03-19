---
name: tabs
description: In-page view switcher for organizing content into sections without navigating away
user_invocable: false
---

# Tabs

**Gallery reference:** https://component.gallery/components/tabs/
**Aliases:** Tab bar, Tab panel, Tab list, View switcher

## When to Use

Use Tabs to organize distinct content categories within a single screen where only one category is shown at a time: agent analytics (Overview / Conversations / Settings), a contact detail (Profile / History / Notes), a documentation page (Guide / API / Examples). Tabs reduce page length by dividing content into scannable categories without requiring navigation.

## When NOT to Use

Do not use Tabs for primary app navigation between major sections — that is Navigation. Do not use Tabs for sequential steps where order matters — that is a Stepper. Do not use Tabs when the user needs to compare content across sections simultaneously. Do not use more than 6–7 tabs — beyond that, use a Select or segmented navigation.

## Anatomy

- **Tab list container:** `role="tablist"`, horizontal, `cv-border-default` bottom border
- **Tab trigger:** Label (Body 500), `cv-radius-sm` top corners, 40px height, horizontal padding 16px
- **Active tab indicator:** 2px `cv-bg-accent` bottom border on active tab, `cv-text-accent` label
- **Inactive tab:** `cv-text-secondary`, hover `cv-text-primary`
- **Tab panel:** `role="tabpanel"`, content area below the tab list, `aria-labelledby` pointing to active tab

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Default underline | Bottom border indicator | Standard in-page tabs |
| Pill tabs | Active tab has pill bg (cv-bg-surface-elevated) | Cards, floating panels |
| Top tabs (mobile) | Horizontally scrollable tabs at top | Mobile content sections |
| Icon + label | Tab has icon + text | When visual differentiation helps scanning |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Inactive | cv-text-secondary label | Navigable, present |
| Hover (web) | cv-text-primary | Responsive |
| Active | cv-text-accent, 2px accent bottom border | Located, in view |
| Focus | 2px cv-border-accent ring | Keyboard-navigable |
| Disabled | cv-text-tertiary, not interactive | Not available |

## Chatverce Styling

- Tab list border: `cv-border-default` bottom
- Active indicator: 2px solid `cv-bg-accent`, bottom of tab
- Active text: `cv-text-accent`, weight 500
- Inactive text: `cv-text-secondary`
- Hover: `cv-text-primary`
- Tab height: 40px
- Panel padding: 20px top

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Tabs>`, `<TabsList>`, `<TabsTrigger>`, `<TabsContent>` (built on Radix UI). Radix handles `role="tablist"`, `role="tab"`, `role="tabpanel"`, arrow-key navigation, and `aria-selected` automatically.

### iOS (Expo + NativeWind)
On mobile, tabs are often rendered as a horizontally scrollable row of Pressable items at the top of the content area. Use a `ScrollView horizontal` with `showsHorizontalScrollIndicator={false}` for tabs that may overflow. Manage `activeTab` state manually.

### Android (Expo + NativeWind)
Same horizontal scroll approach as iOS. Android also supports the Material `TabLayout` pattern but the custom React Native approach gives more styling control. Use `FlatList horizontal` for performance with many tab items.

## Accessibility

- Web: `role="tablist"` on the container, `role="tab"` on each trigger, `aria-selected="true/false"`, `aria-controls` pointing to the panel id. `role="tabpanel"` on each panel, `aria-labelledby` pointing to the tab id. Keyboard: Left/Right arrow keys move between tabs. Tab key moves into the panel content.
- Mobile: `accessibilityRole="tablist"` on container, `accessibilityRole="tab"` and `accessibilityState={{ selected }}` on each tab. Tab panel content is naturally read in order.

## Code Examples

### Web

```tsx
import { Tabs, TabsList, TabsTrigger, TabsContent } from "@/components/ui/tabs";

function AgentDetailTabs({ agent }) {
  return (
    <Tabs defaultValue="overview">
      <TabsList className="border-b border-[--cv-border-default] bg-transparent h-auto p-0 gap-0 rounded-none w-full justify-start">
        {["overview", "conversations", "settings"].map(tab => (
          <TabsTrigger
            key={tab}
            value={tab}
            className="
              capitalize rounded-none h-10 px-4 text-sm border-b-2 border-transparent
              text-[--cv-text-secondary] hover:text-[--cv-text-primary]
              data-[state=active]:text-[--cv-text-accent] data-[state=active]:border-[--cv-bg-accent]
              data-[state=active]:font-medium focus-visible:ring-2 focus-visible:ring-[--cv-border-accent]
            "
          >
            {tab}
          </TabsTrigger>
        ))}
      </TabsList>
      <TabsContent value="overview" className="pt-5">
        <AgentOverview agent={agent} />
      </TabsContent>
      <TabsContent value="conversations" className="pt-5">
        <AgentConversations agent={agent} />
      </TabsContent>
      <TabsContent value="settings" className="pt-5">
        <AgentSettings agent={agent} />
      </TabsContent>
    </Tabs>
  );
}
```

### Mobile

```tsx
import { View, Text, Pressable, ScrollView } from "react-native";
import { useState } from "react";

const TAB_DEFS = [
  { id: "overview", label: "Overview" },
  { id: "conversations", label: "Conversations" },
  { id: "settings", label: "Settings" },
];

function AgentDetailTabsMobile({ agent }) {
  const [active, setActive] = useState("overview");

  return (
    <View className="flex-1">
      <ScrollView
        horizontal
        showsHorizontalScrollIndicator={false}
        className="border-b border-[--cv-border-default]"
        contentContainerStyle={{ paddingHorizontal: 16 }}
        accessibilityRole="tablist"
      >
        {TAB_DEFS.map(tab => {
          const isActive = tab.id === active;
          return (
            <Pressable
              key={tab.id}
              onPress={() => setActive(tab.id)}
              className="mr-4 py-3"
              style={{ borderBottomWidth: 2, borderBottomColor: isActive ? "var(--cv-bg-accent)" : "transparent" }}
              accessibilityRole="tab"
              accessibilityState={{ selected: isActive }}
              accessibilityLabel={tab.label}
            >
              <Text style={{ color: isActive ? "var(--cv-text-accent)" : "var(--cv-text-secondary)", fontWeight: isActive ? "500" : "400", fontSize: 14 }}>
                {tab.label}
              </Text>
            </Pressable>
          );
        })}
      </ScrollView>

      <View className="flex-1 pt-4">
        {active === "overview" && <AgentOverview agent={agent} />}
        {active === "conversations" && <AgentConversations agent={agent} />}
        {active === "settings" && <AgentSettings agent={agent} />}
      </View>
    </View>
  );
}
```

## Anti-Patterns

- Never use Tabs for primary app navigation — that is the Navigation sidebar/tab bar.
- Never make tabs scroll horizontally on web when they can all fit in one row — horizontal scroll on web is unexpected.
- Never use Tabs for sequential flows where order and completion matter — use Stepper.
- Never put form submit actions inside a tab panel where changing tabs might cause confusion about what was submitted.
- Never disable arrow-key navigation between tabs — it is expected behavior for keyboard users.
