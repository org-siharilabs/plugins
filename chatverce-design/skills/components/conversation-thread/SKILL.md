---
name: conversation-thread
description: Full message history view for a single conversation between a user and a Chatverce AI agent, with human/AI message styling, channel indicator, typing indicator, and time grouping
user_invocable: false
---

# Conversation Thread

**Gallery reference:** https://component.gallery/components/chat/
**Aliases:** Chat thread, Message list, Chat view, Inbox thread, Conversation view

## When to Use

Use Conversation Thread to display the full message history of a Chatverce agent conversation. It appears in the Conversations inbox (right pane), the agent testing panel, and any context where a back-and-forth exchange needs to be rendered. It is the most complex display component in the Chatverce system.

## When NOT to Use

Do not use Conversation Thread for one-way notifications — use Alert or Toast. Do not use it for comment sections or document annotations — the visual language is optimized for conversational AI exchanges. Do not use it for agent-to-agent orchestration logs — use a code/log view instead.

## Anatomy

- **Thread header:** Channel indicator badge, conversation ID or contact name, status badge, overflow menu
- **Message list:** Scrollable, virtualized, newest at bottom (inverted FlatList on mobile)
- **AI message bubble:** Left-aligned, `cv-bg-surface` bubble, `cv-text-primary`, Avatar (agent icon) left
- **Human message bubble:** Right-aligned, `cv-bg-accent-subtle` bubble, `cv-text-primary`
- **System message:** Centered, `cv-text-secondary`, Caption — e.g., "Conversation started", "Agent handoff"
- **Timestamp group header:** Centered date label (Today, Yesterday, specific date), `cv-text-tertiary`, Caption
- **Typing indicator:** AI bubble with animated 3-dot pulse (teal dots)
- **Read receipt:** Single/double check marks below human messages, `cv-text-secondary`
- **Message actions (hover/long-press):** Copy, React, Reply, Delete — via Dropdown Menu

## Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| Full inbox view | Complete thread with header | Conversations inbox pane |
| Testing panel | Compact, no header overhead | Agent builder test panel |
| Read-only replay | Non-interactive, all messages visible | Analytics, conversation review |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Loading | Skeleton messages | Content is coming |
| Empty | Empty State — first message prompt | New conversation, inviting |
| Receiving AI message | Typing indicator, then message appears | AI is working, alive |
| Sending human message | Message appears immediately (optimistic) | Responsive, immediate |
| Failed send | Message with retry icon, cv-text-error | Transparent, recoverable |
| Long-press (mobile) | Message action menu | Contextual, controllable |
| Hover (web) | Message action icons appear | Discoverable |

## Chatverce Styling

**AI message bubble:**
- Background: `cv-bg-surface`
- Border: `cv-border-default` (subtle)
- Border radius: `cv-radius-md` (all corners)
- Max width: 75% of container
- Shadow: `cv-shadow-sm`

**Human message bubble:**
- Background: `cv-bg-accent-subtle`
- Border: none
- Border radius: `cv-radius-md`
- Max width: 75% of container

**Typing indicator:**
- Same as AI bubble with 3 teal dots (`cv-bg-accent`), 400ms stagger animation

**Time group header:**
- `cv-text-tertiary`, Caption, centered, margin top/bottom 16px

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use `react-virtual` (TanStack Virtual / `@tanstack/react-virtual`) for virtualizing the message list. This is required for conversations with thousands of messages — rendering all messages at once causes severe performance degradation. Scroll anchor to the bottom on new messages with `useEffect` watching message count.

### iOS (Expo + NativeWind)
Use React Native `<FlatList>` with `inverted={true}`. Inverted FlatList renders the list upside-down and scrolled to the bottom by default, which is the correct behavior for chat interfaces. Pass messages in reverse order or use `inverted` prop (which reverses the internal rendering). Use `maintainVisibleContentPosition` to prevent scroll jump on new messages.

### Android (Expo + NativeWind)
Same as iOS. `inverted` FlatList is essential on Android — without it, scroll management during message load is extremely difficult. Android keyboard handling requires `KeyboardAvoidingView` with `behavior="height"`.

## Accessibility

- Web: `role="log"` on the message list container — screen readers treat log as a live region that announces new messages. Each message group has `aria-label`. Time group headers are `<time>` elements with `dateTime` attribute. The typing indicator has `aria-live="polite"` and `aria-label="Agent is typing"`.
- Mobile: `accessibilityLiveRegion="polite"` on the message list. Each message has `accessibilityLabel` with sender name + message content. The typing indicator has `accessibilityLabel="Agent is typing"`.

## Code Examples

### Web

```tsx
import { useVirtualizer } from "@tanstack/react-virtual";
import { useRef, useEffect } from "react";
import { Avatar } from "@/components/ui/avatar";
import { ChannelBadge } from "@/components/channel-badge";

function TypingIndicator() {
  return (
    <div className="flex items-start gap-3 py-1">
      <div className="w-8 h-8 rounded-full bg-[--cv-bg-accent-subtle] flex items-center justify-center shrink-0">
        <BotIcon size={16} className="text-[--cv-text-accent]" />
      </div>
      <div
        className="bg-[--cv-bg-surface] border border-[--cv-border-default] rounded-[--cv-radius-md] shadow-[--cv-shadow-sm] px-4 py-3 flex items-center gap-1"
        aria-live="polite"
        aria-label="Agent is typing"
      >
        {[0, 1, 2].map(i => (
          <span
            key={i}
            className="w-2 h-2 rounded-full bg-[--cv-bg-accent] animate-bounce"
            style={{ animationDelay: `${i * 133}ms` }}
          />
        ))}
      </div>
    </div>
  );
}

function MessageBubble({ message }) {
  const isAI = message.sender === "agent";
  return (
    <div className={`flex items-start gap-3 py-1 ${isAI ? "" : "flex-row-reverse"}`}>
      {isAI && (
        <div className="w-8 h-8 rounded-full bg-[--cv-bg-accent-subtle] flex items-center justify-center shrink-0">
          <BotIcon size={16} className="text-[--cv-text-accent]" />
        </div>
      )}
      <div className={`max-w-[75%] rounded-[--cv-radius-md] px-4 py-2.5 text-sm text-[--cv-text-primary] ${
        isAI
          ? "bg-[--cv-bg-surface] border border-[--cv-border-default] shadow-[--cv-shadow-sm]"
          : "bg-[--cv-bg-accent-subtle]"
      }`}>
        {message.text}
        <span className="block text-[10px] text-[--cv-text-tertiary] mt-1 text-right">
          {format(new Date(message.timestamp), "HH:mm")}
        </span>
      </div>
    </div>
  );
}

function ConversationThread({ conversation, messages, isTyping }) {
  const scrollRef = useRef(null);

  useEffect(() => {
    if (scrollRef.current) {
      scrollRef.current.scrollTop = scrollRef.current.scrollHeight;
    }
  }, [messages.length, isTyping]);

  return (
    <div className="flex flex-col h-full bg-[--cv-bg-primary]">
      {/* Thread header */}
      <div className="flex items-center gap-3 px-4 py-3 bg-[--cv-bg-surface] border-b border-[--cv-border-default]">
        <ChannelBadge channel={conversation.channel} />
        <span className="text-[--cv-text-primary] font-medium text-sm">{conversation.contactName}</span>
        <AgentStatusBadge status={conversation.status} />
      </div>

      {/* Message list */}
      <div
        ref={scrollRef}
        className="flex-1 overflow-y-auto px-4 py-4 space-y-1"
        role="log"
        aria-label="Conversation messages"
        aria-live="polite"
      >
        {messages.map((message, i) => {
          const prevMessage = messages[i - 1];
          const showDateSep = !prevMessage || !isSameDay(new Date(message.timestamp), new Date(prevMessage.timestamp));
          return (
            <div key={message.id}>
              {showDateSep && (
                <div className="text-center my-4">
                  <time className="text-[--cv-text-tertiary] text-xs" dateTime={message.timestamp}>
                    {isToday(new Date(message.timestamp)) ? "Today" : format(new Date(message.timestamp), "MMMM d")}
                  </time>
                </div>
              )}
              <MessageBubble message={message} />
            </div>
          );
        })}
        {isTyping && <TypingIndicator />}
      </div>
    </div>
  );
}
```

### Mobile

```tsx
import { FlatList, View, Text, KeyboardAvoidingView, Platform } from "react-native";
import { format, isToday, isSameDay } from "date-fns";

function MessageBubbleMobile({ message }) {
  const isAI = message.sender === "agent";
  return (
    <View className={`flex-row items-start gap-3 py-1 ${isAI ? "" : "flex-row-reverse"}`}>
      {isAI && (
        <View className="w-8 h-8 rounded-full bg-[--cv-bg-accent-subtle] items-center justify-center">
          <BotIcon size={16} color="var(--cv-text-accent)" />
        </View>
      )}
      <View
        className={`max-w-[75%] rounded-[--cv-radius-md] px-4 py-2.5 ${isAI ? "bg-[--cv-bg-surface]" : "bg-[--cv-bg-accent-subtle]"}`}
        style={isAI ? { shadowColor: "#000", shadowOpacity: 0.05, shadowRadius: 4, elevation: 1 } : {}}
        accessibilityLabel={`${isAI ? "Agent" : "You"}: ${message.text}`}
      >
        <Text className="text-[--cv-text-primary] text-sm">{message.text}</Text>
        <Text className="text-[--cv-text-tertiary] text-[10px] mt-1 text-right">
          {format(new Date(message.timestamp), "HH:mm")}
        </Text>
      </View>
    </View>
  );
}

function TypingIndicatorMobile() {
  const dot1 = useRef(new Animated.Value(0.3)).current;
  // …animate dots with Animated.loop

  return (
    <View className="flex-row items-start gap-3 py-1" accessibilityLabel="Agent is typing" accessibilityLiveRegion="polite">
      <View className="w-8 h-8 rounded-full bg-[--cv-bg-accent-subtle] items-center justify-center">
        <BotIcon size={16} color="var(--cv-text-accent)" />
      </View>
      <View className="bg-[--cv-bg-surface] rounded-[--cv-radius-md] px-4 py-3 flex-row gap-1 items-center">
        {[0, 1, 2].map(i => (
          <View key={i} style={{ width: 8, height: 8, borderRadius: 4, backgroundColor: "var(--cv-bg-accent)", opacity: 0.6 + (i * 0.2) }} />
        ))}
      </View>
    </View>
  );
}

function ConversationThreadMobile({ messages, isTyping }) {
  const allItems = isTyping ? [...messages, { id: "typing", type: "typing" }] : messages;

  return (
    <KeyboardAvoidingView
      behavior={Platform.OS === "ios" ? "padding" : "height"}
      className="flex-1 bg-[--cv-bg-primary]"
    >
      <FlatList
        data={allItems}
        inverted
        keyExtractor={item => item.id}
        contentContainerStyle={{ padding: 16 }}
        accessibilityLiveRegion="polite"
        maintainVisibleContentPosition={{ minIndexForVisible: 0 }}
        renderItem={({ item, index }) => {
          if (item.type === "typing") return <TypingIndicatorMobile />;
          const prevItem = allItems[index + 1]; // inverted: next in array = previous in time
          const showDateSep = !prevItem || prevItem.type === "typing" || !isSameDay(new Date(item.timestamp), new Date(prevItem.timestamp));
          return (
            <View>
              {showDateSep && (
                <View className="items-center my-4">
                  <Text className="text-[--cv-text-tertiary] text-xs">
                    {isToday(new Date(item.timestamp)) ? "Today" : format(new Date(item.timestamp), "MMMM d")}
                  </Text>
                </View>
              )}
              <MessageBubbleMobile message={item} />
            </View>
          );
        }}
      />
    </KeyboardAvoidingView>
  );
}
```

## Anti-Patterns

- Never render all messages in a non-virtualized list — conversations can have thousands of messages; this will crash the UI.
- Never use `ScrollView` for the message list on mobile — `FlatList` with `inverted` is required for correct chat behavior.
- Never align all messages to one side — AI messages must be left-aligned, human messages right-aligned. This visual distinction is the core of the conversational interface.
- Never skip the typing indicator — the gap between sending a message and receiving an AI response without feedback feels broken.
- Never use Toast for incoming message notifications while the thread is open — messages should appear inline in the thread.
