
# Agent Builder Canvas

**Gallery reference:** https://component.gallery/components/canvas/
**Aliases:** Flow canvas, Node canvas, Workflow editor, Visual editor, Agent flow

## When to Use

Use Agent Builder Canvas as the primary workspace for the Chatverce agent workflow editor. It is the environment where users drag, connect, and configure nodes to build the behavioral logic of an AI agent. This is a single-purpose, full-screen component for the builder screen.

## When NOT to Use

Do not use Canvas for simpler configuration — if an agent can be set up with a form, use a Form in a Card. Do not attempt to render an editable Canvas on mobile in v2 — mobile is read-only visualization only. Do not use Canvas for displaying data or static content — use Table or Dashboard components.

## Anatomy

- **Canvas background:** Full-screen work area, `cv-bg-primary` base, dot-grid pattern overlay in `cv-text-secondary` at 5% opacity (20px grid spacing)
- **Bezier connectors:** Curved lines connecting node output ports to input ports, `cv-text-secondary` stroke, 2px width
- **Animated flow dots:** Teal dots (`cv-bg-accent`) that travel along connector paths to indicate active data flow during test/run
- **Zoom controls:** Bottom-right corner, Card-elevated panel — zoom in (+), zoom out (-), fit to screen (arrows), current zoom % display
- **Minimap (optional):** Bottom-right corner, thumbnail of full canvas, visible viewport rectangle
- **Toolbar:** Top edge or floating, canvas-level actions (add node, undo, redo, delete selected)
- **Properties panel:** Drawer from right edge, opens on node selection

## Variants

| Variant | Description | Platform |
|---------|-------------|----------|
| Edit mode | Drag, connect, configure nodes | Web only (v2) |
| Read-only viz | View-only canvas with pan/zoom | Mobile (v2) |
| Preview/test mode | Runs flow, shows animated dots | Web only (v2) |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Empty canvas | Centered empty state with "Add your first node" | Inviting, not blank |
| Idle | Dot grid, nodes at rest, no animation | Stable, ready |
| Dragging node | Node follows cursor, connector ports highlighted | Interactive, fluid |
| Connecting ports | Bezier line preview follows cursor from port | Visual feedback |
| Selected node | Node has 2px cv-border-accent ring | Selected, properties open |
| Testing/running | Teal dots animate along connections | AI is active, data is flowing |
| Error on node | Node has cv-border-error ring + error badge | Issue requiring attention |
| Zoom | Canvas scales smoothly | Spatial control |

## Chatverce Styling

- Canvas bg: `cv-bg-primary`
- Dot grid: `cv-text-secondary` at 5% opacity, 20px spacing, 1.5px dots
- Connector lines: `cv-text-secondary` stroke, 2px, cubic Bezier
- Flow dots: `cv-bg-accent`, 8px diameter, 3 dots per connection with stagger
- Selected ring: 2px `cv-border-accent`, 4px offset
- Error ring: 2px `cv-border-error`
- Zoom panel: `cv-bg-surface`, `cv-shadow-sm`, `cv-radius-md`
- Properties drawer: standard Drawer component, right side, 400px

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use **React Flow** (`@xyflow/react`) as the canvas engine. React Flow handles:
- Node drag and drop
- Edge (connector) rendering with Bezier paths
- Pan and zoom with viewport controls
- Node/edge selection state
- Custom node and edge types

Custom node types extend React Flow's `NodeProps`. The dot grid background is implemented via an SVG pattern in the canvas style. Animated flow dots use React Flow's `AnimatedEdge` or a custom edge component with CSS animations.

### iOS / Android (Expo + NativeWind) — Read-only
For mobile v2, render a static or panzoomable view of the agent flow. Use `react-native-gesture-handler` with `react-native-reanimated` for pinch-to-zoom and pan. Node connections are rendered as SVG paths via `react-native-svg`. No drag or connection interaction.

## Accessibility

- Web: Canvas is a complex widget — provide an alternative list view of the flow (accessible tree view) for keyboard/screen reader users. Each node is focusable with `tabIndex` and navigable via arrow keys when in focus mode. `aria-label="Agent workflow canvas"` on the canvas container. `role="application"` signals to screen readers that keyboard interaction is managed by the app.
- Mobile: Read-only canvas has `accessibilityRole="image"` and `accessibilityLabel` describing the agent's flow structure in text form.

## Code Examples

### Web

```tsx
import ReactFlow, {
  ReactFlowProvider, Background, Controls, MiniMap,
  useNodesState, useEdgesState, addEdge, BackgroundVariant,
} from "@xyflow/react";
import "@xyflow/react/dist/style.css";
import { useCallback } from "react";
import { AgentBuilderNode } from "@/components/agent-builder-node";
import { AgentPropertiesDrawer } from "@/components/agent-properties-drawer";
import { EmptyCanvasState } from "@/components/empty-canvas-state";

const nodeTypes = { agentNode: AgentBuilderNode };

function AgentBuilderCanvas({ agentId }) {
  const [nodes, setNodes, onNodesChange] = useNodesState([]);
  const [edges, setEdges, onEdgesChange] = useEdgesState([]);
  const [selectedNode, setSelectedNode] = useState(null);

  const onConnect = useCallback(
    (params) => setEdges((eds) => addEdge({ ...params, animated: false, style: { stroke: "var(--cv-text-secondary)", strokeWidth: 2 } }, eds)),
    [setEdges]
  );

  const onNodeClick = useCallback((event, node) => {
    setSelectedNode(node);
  }, []);

  return (
    <div
      className="w-full h-full bg-[--cv-bg-primary] relative"
      role="application"
      aria-label="Agent workflow canvas"
    >
      <ReactFlowProvider>
        <ReactFlow
          nodes={nodes}
          edges={edges}
          onNodesChange={onNodesChange}
          onEdgesChange={onEdgesChange}
          onConnect={onConnect}
          onNodeClick={onNodeClick}
          nodeTypes={nodeTypes}
          fitView
          proOptions={{ hideAttribution: true }}
        >
          <Background
            variant={BackgroundVariant.Dots}
            gap={20}
            size={1.5}
            color="var(--cv-text-secondary)"
            style={{ opacity: 0.05 }}
          />
          <Controls
            position="bottom-right"
            className="bg-[--cv-bg-surface] shadow-[--cv-shadow-sm] rounded-[--cv-radius-md] border border-[--cv-border-default]"
          />
          <MiniMap
            position="bottom-right"
            style={{ marginBottom: "48px", backgroundColor: "var(--cv-bg-surface)" }}
          />
          {nodes.length === 0 && <EmptyCanvasState />}
        </ReactFlow>
      </ReactFlowProvider>

      <AgentPropertiesDrawer
        open={!!selectedNode}
        node={selectedNode}
        onClose={() => setSelectedNode(null)}
      />
    </div>
  );
}
```

### Mobile (Read-only)

```tsx
import { View, Text, ScrollView } from "react-native";
import { Gesture, GestureDetector } from "react-native-gesture-handler";
import Animated, { useSharedValue, useAnimatedStyle } from "react-native-reanimated";
import Svg, { Path, Circle } from "react-native-svg";

function AgentFlowReadOnly({ nodes, edges }) {
  const scale = useSharedValue(1);
  const translateX = useSharedValue(0);
  const translateY = useSharedValue(0);

  const pinchGesture = Gesture.Pinch()
    .onUpdate((e) => { scale.value = Math.max(0.3, Math.min(3, e.scale)); });

  const panGesture = Gesture.Pan()
    .onUpdate((e) => { translateX.value = e.translationX; translateY.value = e.translationY; });

  const composed = Gesture.Simultaneous(pinchGesture, panGesture);
  const animatedStyle = useAnimatedStyle(() => ({
    transform: [{ translateX: translateX.value }, { translateY: translateY.value }, { scale: scale.value }],
  }));

  return (
    <GestureDetector gesture={composed}>
      <View
        className="flex-1 bg-[--cv-bg-primary] overflow-hidden"
        accessibilityRole="image"
        accessibilityLabel={`Agent workflow with ${nodes.length} steps`}
      >
        <Animated.View style={[{ flex: 1 }, animatedStyle]}>
          {/* Render nodes and edges as static SVG + Views */}
          {nodes.map(node => (
            <View
              key={node.id}
              className="absolute bg-[--cv-bg-surface] rounded-[--cv-radius-md] p-3 shadow-sm"
              style={{ left: node.position.x, top: node.position.y, width: 160 }}
            >
              <Text className="text-[--cv-text-primary] text-sm font-medium">{node.data.label}</Text>
            </View>
          ))}
        </Animated.View>
        <View className="absolute bottom-4 right-4 bg-[--cv-bg-surface] rounded-[--cv-radius-md] px-3 py-1.5 shadow-sm">
          <Text className="text-[--cv-text-secondary] text-xs">View only</Text>
        </View>
      </View>
    </GestureDetector>
  );
}
```

## Anti-Patterns

- Never attempt editable canvas on mobile in v2 — drag-and-drop node building on touch is out of scope.
- Never leave the canvas blank with no empty state — always show an invitation to add the first node.
- Never render flow dots (animated teal) in edit mode — animation should only appear during test/preview runs.
- Never use the Canvas component outside the agent builder screen — it is a single-purpose full-screen workspace.
- Never hardcode node positions — all node coordinates must be managed by React Flow's state system.
