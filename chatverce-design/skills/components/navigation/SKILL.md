---
name: navigation
description: Primary wayfinding structure — sidebar on web, bottom tab bar on mobile
user_invocable: false
---

# Navigation

**Gallery reference:** https://component.gallery/components/navigation/
**Aliases:** Nav bar, Sidebar, Tab bar, App nav, Menu, Primary navigation

## When to Use

Navigation is the persistent structure that lets users move between the major sections of the Chatverce app. It appears on every authenticated screen. On web, it is a left sidebar. On mobile, it is a bottom tab bar.

## When NOT to Use

Do not use Navigation for in-page section switching — that is Tabs. Do not add more than 5 items to the bottom tab bar on mobile. Do not use Navigation for contextual menus or action lists — that is Dropdown Menu. Do not use Navigation as a secondary panel for settings within a section — that is Drawer or a settings sub-layout.

## Anatomy

### Web Sidebar
- **Container:** Fixed left, 240px wide, `cv-bg-surface`, `cv-border-default` right border, full viewport height
- **Logo area:** Top 64px, Chatverce wordmark or icon
- **Nav items:** Icon (20px) + label (Body), 48px row height, `cv-radius-md` pill active state
- **Active state:** `cv-bg-accent-subtle` background, `cv-text-accent` text and icon
- **Inactive state:** `cv-text-secondary` text and icon, hover `cv-bg-surface-elevated`
- **Section headers (optional):** Caption uppercase, `cv-text-tertiary`, margin above group
- **Bottom area:** User avatar + name + settings link

### Mobile Bottom Tab Bar
- **Container:** Fixed bottom, full width, `cv-bg-surface`, `cv-border-default` top border, safe area padding
- **Tab items:** Icon (24px) + label (Caption, 10–11px), max 5 items, equal width
- **Active state:** `cv-text-accent` icon and label, subtle 10% bg pill behind icon
- **Inactive state:** `cv-text-secondary`

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Web sidebar (full) | Icon + label, 240px | Desktop/tablet — authenticated app shell |
| Web sidebar (collapsed) | Icon only, 64px | When sidebar is collapsed by user |
| Mobile tab bar | Bottom tabs, max 5 | Mobile app — all authenticated screens |
| Mobile drawer nav | Full-screen overlay drawer | Overflow nav items beyond 5 on mobile |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default (inactive) | cv-text-secondary icon + label | Present, not shouting |
| Hover (web) | cv-bg-surface-elevated | Responsive |
| Active | cv-text-accent, cv-bg-accent-subtle pill | Located, in place |
| Focus | 2px cv-border-accent ring | Keyboard-navigable |
| Badge on item | Red dot or count badge | Attention needed in that section |

## Chatverce Styling

**Web sidebar:**
- Background: `cv-bg-surface`
- Width: 240px (full), 64px (collapsed)
- Active pill: `cv-bg-accent-subtle`, `cv-radius-md`, `cv-text-accent`
- Inactive: `cv-text-secondary`, hover `cv-bg-surface-elevated`
- Border right: `cv-border-default`

**Mobile tab bar:**
- Background: `cv-bg-surface`
- Border top: `cv-border-default`
- Active: `cv-text-accent`
- Inactive: `cv-text-secondary`
- Safe area: pad bottom by `useSafeAreaInsets().bottom`

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Build a custom sidebar component using Next.js App Router layouts. The sidebar lives in `app/(dashboard)/layout.tsx` and persists across all child routes. Use `usePathname()` to determine the active route and apply active styles. Use `next/link` for navigation items.

### iOS (Expo + NativeWind)
Use Expo Router `<Tabs>` component with `tabBarStyle` overrides. The tab bar is natively rendered by React Navigation and handles safe area insets automatically. Customize `tabBarActiveTintColor`, `tabBarInactiveTintColor`, and `tabBarStyle` to match Chatverce tokens.

### Android (Expo + NativeWind)
Same as iOS with Expo Router `<Tabs>`. Android renders a Material bottom navigation bar by default. Override with `tabBarStyle` to match Chatverce styling. Ensure tab labels are short (1–2 words) to prevent truncation on small screens.

## Accessibility

- Web: Use `<nav>` element with `aria-label="Main navigation"`. Each link is an `<a>` with `aria-current="page"` on the active item. Keyboard: Tab moves between items, Enter activates.
- Mobile: Expo Router Tabs handles `accessibilityRole="tab"` and `accessibilityState={{ selected }}` automatically. Ensure `tabBarAccessibilityLabel` is set for each tab.

## Code Examples

### Web

```tsx
// app/(dashboard)/layout.tsx
import Link from "next/link";
import { usePathname } from "next/navigation";
import { LayoutDashboard, Bot, MessageSquare, Users, Settings } from "lucide-react";

const navItems = [
  { href: "/dashboard", label: "Dashboard", icon: LayoutDashboard },
  { href: "/agents", label: "Agents", icon: Bot },
  { href: "/conversations", label: "Conversations", icon: MessageSquare },
  { href: "/contacts", label: "Contacts", icon: Users },
];

function Sidebar() {
  const pathname = usePathname();
  return (
    <nav
      aria-label="Main navigation"
      className="fixed left-0 top-0 h-screen w-60 bg-[--cv-bg-surface] border-r border-[--cv-border-default] flex flex-col z-30"
    >
      <div className="h-16 flex items-center px-4 border-b border-[--cv-border-default]">
        <span className="text-[--cv-text-primary] font-semibold text-lg">Chatverce</span>
      </div>
      <div className="flex-1 overflow-y-auto px-3 py-4 space-y-1">
        {navItems.map(({ href, label, icon: Icon }) => {
          const active = pathname.startsWith(href);
          return (
            <Link
              key={href}
              href={href}
              aria-current={active ? "page" : undefined}
              className={`flex items-center gap-3 px-3 py-2.5 rounded-[--cv-radius-md] text-sm transition-colors ${
                active
                  ? "bg-[--cv-bg-accent-subtle] text-[--cv-text-accent] font-medium"
                  : "text-[--cv-text-secondary] hover:bg-[--cv-bg-surface-elevated] hover:text-[--cv-text-primary]"
              }`}
            >
              <Icon size={20} />
              {label}
            </Link>
          );
        })}
      </div>
      <div className="px-3 pb-4 border-t border-[--cv-border-default] pt-3">
        <Link href="/settings" className="flex items-center gap-3 px-3 py-2.5 rounded-[--cv-radius-md] text-sm text-[--cv-text-secondary] hover:bg-[--cv-bg-surface-elevated]">
          <Settings size={20} />
          Settings
        </Link>
      </div>
    </nav>
  );
}
```

### Mobile

```tsx
// app/(tabs)/_layout.tsx
import { Tabs } from "expo-router";
import { useSafeAreaInsets } from "react-native-safe-area-context";
import { LayoutDashboard, Bot, MessageSquare, Users } from "lucide-react-native";

export default function TabsLayout() {
  const insets = useSafeAreaInsets();
  return (
    <Tabs
      screenOptions={{
        tabBarActiveTintColor: "var(--cv-text-accent)",
        tabBarInactiveTintColor: "var(--cv-text-secondary)",
        tabBarStyle: {
          backgroundColor: "var(--cv-bg-surface)",
          borderTopColor: "var(--cv-border-default)",
          paddingBottom: insets.bottom,
          height: 56 + insets.bottom,
        },
        tabBarLabelStyle: { fontSize: 10, fontFamily: "Inter_400Regular" },
        headerShown: false,
      }}
    >
      <Tabs.Screen name="dashboard" options={{ title: "Home", tabBarIcon: ({ color }) => <LayoutDashboard size={24} color={color} />, tabBarAccessibilityLabel: "Dashboard" }} />
      <Tabs.Screen name="agents" options={{ title: "Agents", tabBarIcon: ({ color }) => <Bot size={24} color={color} />, tabBarAccessibilityLabel: "Agents" }} />
      <Tabs.Screen name="conversations" options={{ title: "Inbox", tabBarIcon: ({ color }) => <MessageSquare size={24} color={color} />, tabBarAccessibilityLabel: "Conversation inbox" }} />
      <Tabs.Screen name="contacts" options={{ title: "Contacts", tabBarIcon: ({ color }) => <Users size={24} color={color} />, tabBarAccessibilityLabel: "Contacts" }} />
    </Tabs>
  );
}
```

## Anti-Patterns

- Never put more than 5 items in the mobile bottom tab bar — beyond 5, navigation becomes cluttered and items need a "More" overflow, which obscures structure.
- Never use icons without labels in the sidebar on web — icons alone require memorization.
- Never use Navigation for in-page section switching — that is Tabs.
- Never highlight more than one nav item as active at the same time.
- Never put a destructive action (delete, sign out) in the primary nav without clear separation — destructive items should be at the bottom, separated from primary navigation.
