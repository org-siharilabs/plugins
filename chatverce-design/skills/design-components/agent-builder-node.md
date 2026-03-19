
# Agent Builder Node

**Gallery reference:** https://component.gallery/components/card/
**Aliases:** Flow node, Workflow node, Agent step, Canvas node, Node card

## When to Use

Use Agent Builder Node as the building block of agent workflows within the Agent Builder Canvas. Every step in a Chatverce agent workflow is represented as a Node: where the conversation starts (Trigger), what the agent does (Action), how it branches (Condition), and what it says (Response). Nodes are only used inside the Canvas component.

## When NOT to Use

Do not use Agent Builder Node outside the Canvas context. Do not use a generic Card in place of a Node in the canvas — the Node has specific port and drag-handle anatomy that Cards do not have. Do not create new node type colors outside the defined type system.

## Anatomy

- **Card container:** `cv-bg-surface`, `cv-shadow-sm`, `cv-radius-md`, width 200px fixed
- **Left border:** 4px solid, color by node type
- **Drag handle:** Top-right corner, GripVertical icon, `cv-text-tertiary`, cursor: grab — visible on hover
- **Node type badge:** Caption, color-matched to left border, top-left
- **Node title:** H3 (Body weight 500), `cv-text-primary`, truncated at 1 line
- **Node description (optional):** Caption, `cv-text-secondary`, max 2 lines, truncated
- **Input port:** Left side, centered vertically, 12px teal dot — receives connections from upstream nodes
- **Output port(s):** Right side, 12px teal dot(s) — connects to downstream nodes
- **Status indicator:** Bottom-right, shows error/valid/untested state
- **Selected state:** 2px `cv-border-accent` ring, 4px offset

## Node Types and Colors

| Type | Left Border Color | Badge Color | Description |
|------|------------------|-------------|-------------|
| Trigger | Chatverce Teal (`cv-bg-accent` / `#2EC4B6`) | Teal | Starting point — incoming message, schedule, webhook |
| Action | Midnight Navy (`#1B2A4A`) | Navy | Do something — send message, call API, update CRM |
| Condition | Warm Amber (`#E8A838`) | Amber | Branch logic — if/else, contains keyword, status check |
| Response | Sage Green (`#52B788`) | Green | AI-generated or template reply to the user |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | Card at rest, ports visible | Stable building block |
| Hover | Drag handle appears, slight shadow lift | Moveable, interactive |
| Drag in progress | 50% opacity, cursor: grabbing | Being moved |
| Selected | cv-border-accent 2px ring | Focused, properties open in Drawer |
| Connected (ports) | Port dots fill to solid teal | Connection established |
| Hover over port | Port dot scales 1.3x | Port is a target/source |
| Error state | cv-border-error ring, red badge on node | Configuration issue |
| Untested | Grey indicator | Not yet validated |
| Valid | Green indicator | Passed validation |

## Chatverce Styling

- Container: `cv-bg-surface`, `cv-shadow-sm`, `cv-radius-md`
- Width: 200px (fixed — prevents uneven canvas layouts)
- Left border: 4px solid, node-type color
- Selected ring: 2px `cv-border-accent`, outline 4px offset
- Port dot: 12px diameter, `cv-bg-accent` fill, `cv-bg-surface` border 2px (ring)
- Drag handle: `cv-text-tertiary`, visible on node hover only
- Font: Inter, type badge Caption 10px, title Body 14px

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Custom React Flow node via `nodeTypes` registration. Extends `NodeProps` from `@xyflow/react`. Use `Handle` component from React Flow for input/output ports — React Flow manages port hover, connection dragging, and connection validation. The drag handle uses React Flow's `NodeResizer` pattern or a custom `drag-handle` className.

### iOS / Android (Expo + NativeWind)
In read-only mode, nodes are rendered as simple `View` components with the same color-coding and layout. No Handle components — ports are visual-only decorations.

## Accessibility

- Web: Each node is focusable with `tabIndex={0}`. `aria-label` describes the node type, title, and connection status (e.g., "Trigger node: New WhatsApp message, connected to 1 action"). Arrow keys can move focus between connected nodes when canvas focus mode is enabled. Selected node opens the Properties Drawer which receives focus.
- Mobile: Nodes in read-only view have `accessibilityLabel` describing type and title. The overall canvas has a text-based description of the workflow structure.

## Code Examples

### Web

```tsx
import { memo } from "react";
import { Handle, Position } from "@xyflow/react";
import { GripVertical, AlertCircle, CheckCircle2 } from "lucide-react";

const NODE_STYLES = {
  trigger:   { border: "#2EC4B6", badge: "#2EC4B6", badgeBg: "rgba(46,196,182,0.12)", label: "Trigger" },
  action:    { border: "#1B2A4A", badge: "#1B2A4A", badgeBg: "rgba(27,42,74,0.12)",   label: "Action" },
  condition: { border: "#E8A838", badge: "#E8A838", badgeBg: "rgba(232,168,56,0.12)", label: "Condition" },
  response:  { border: "#52B788", badge: "#52B788", badgeBg: "rgba(82,183,136,0.12)", label: "Response" },
};

function AgentBuilderNode({ data, selected }) {
  const style = NODE_STYLES[data.type] || NODE_STYLES.action;
  const hasInput = data.type !== "trigger";
  const hasOutput = data.type !== "response";

  return (
    <div
      className="relative bg-[--cv-bg-surface] rounded-[--cv-radius-md] w-[200px] group"
      style={{
        boxShadow: selected ? `0 0 0 2px var(--cv-border-accent)` : "var(--cv-shadow-sm)",
        borderLeft: `4px solid ${style.border}`,
      }}
      aria-label={`${style.label} node: ${data.label}`}
      tabIndex={0}
    >
      {/* Input port */}
      {hasInput && (
        <Handle
          type="target"
          position={Position.Left}
          className="w-3 h-3 rounded-full border-2 border-[--cv-bg-surface]"
          style={{ backgroundColor: "var(--cv-bg-accent)", left: -8 }}
        />
      )}

      {/* Drag handle — shown on hover */}
      <div className="absolute top-2 right-2 opacity-0 group-hover:opacity-100 cursor-grab active:cursor-grabbing drag-handle">
        <GripVertical size={14} className="text-[--cv-text-tertiary]" />
      </div>

      {/* Node content */}
      <div className="p-3 pr-6">
        {/* Type badge */}
        <span
          className="inline-block text-[10px] font-medium px-1.5 py-0.5 rounded-full mb-2"
          style={{ backgroundColor: style.badgeBg, color: style.badge }}
        >
          {style.label}
        </span>
        {/* Title */}
        <p className="text-[--cv-text-primary] text-sm font-medium leading-tight truncate">{data.label}</p>
        {/* Description */}
        {data.description && (
          <p className="text-[--cv-text-secondary] text-xs mt-1 line-clamp-2">{data.description}</p>
        )}
        {/* Status */}
        <div className="absolute bottom-2 right-2">
          {data.status === "error" && <AlertCircle size={12} className="text-[--cv-status-error]" />}
          {data.status === "valid" && <CheckCircle2 size={12} className="text-[--cv-status-success]" />}
        </div>
      </div>

      {/* Output port */}
      {hasOutput && (
        <Handle
          type="source"
          position={Position.Right}
          className="w-3 h-3 rounded-full border-2 border-[--cv-bg-surface]"
          style={{ backgroundColor: "var(--cv-bg-accent)", right: -8 }}
        />
      )}
    </div>
  );
}

export default memo(AgentBuilderNode);
```

### Mobile (Read-only)

```tsx
import { View, Text } from "react-native";

const NODE_STYLES = {
  trigger:   { border: "#2EC4B6", badgeBg: "rgba(46,196,182,0.12)", badgeText: "#2EC4B6", label: "Trigger" },
  action:    { border: "#1B2A4A", badgeBg: "rgba(27,42,74,0.12)",   badgeText: "#1B2A4A", label: "Action" },
  condition: { border: "#E8A838", badgeBg: "rgba(232,168,56,0.12)", badgeText: "#E8A838", label: "Condition" },
  response:  { border: "#52B788", badgeBg: "rgba(82,183,136,0.12)", badgeText: "#52B788", label: "Response" },
};

function AgentBuilderNodeMobile({ node }) {
  const style = NODE_STYLES[node.type] || NODE_STYLES.action;
  return (
    <View
      style={{
        position: "absolute",
        left: node.position.x,
        top: node.position.y,
        width: 160,
        backgroundColor: "var(--cv-bg-surface)",
        borderRadius: 8,
        borderLeftWidth: 4,
        borderLeftColor: style.border,
        shadowColor: "#000",
        shadowOpacity: 0.06,
        shadowRadius: 4,
        elevation: 2,
        padding: 10,
      }}
      accessibilityLabel={`${style.label} node: ${node.data.label}`}
    >
      <View style={{ backgroundColor: style.badgeBg, borderRadius: 10, alignSelf: "flex-start", paddingHorizontal: 6, paddingVertical: 2, marginBottom: 6 }}>
        <Text style={{ color: style.badgeText, fontSize: 9, fontWeight: "500" }}>{style.label}</Text>
      </View>
      <Text style={{ color: "var(--cv-text-primary)", fontSize: 12, fontWeight: "500" }} numberOfLines={1}>{node.data.label}</Text>
      {node.data.description && (
        <Text style={{ color: "var(--cv-text-secondary)", fontSize: 10, marginTop: 3 }} numberOfLines={2}>{node.data.description}</Text>
      )}
    </View>
  );
}
```

## Anti-Patterns

- Never create new node type colors outside the four defined types — Trigger (Teal), Action (Navy), Condition (Amber), Response (Green). New types must be added to the design system first.
- Never make the node width variable — 200px is fixed for consistent canvas alignment.
- Never put more than 2–3 lines of content inside a node — it is a summary; full configuration is in the Properties Drawer.
- Never render nodes as generic `<div>` elements without React Flow's `Handle` ports — connections will not work.
- Never skip the `aria-label` on nodes — the canvas is complex and screen readers need the node identity.
