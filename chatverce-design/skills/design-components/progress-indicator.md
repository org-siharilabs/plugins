
# Progress Indicator

**Gallery reference:** https://component.gallery/components/progress-indicator/
**Aliases:** Step indicator, Wizard progress, Stepper progress, Onboarding steps, Multi-step progress, Breadcrumb stepper

## When to Use

Use Progress Indicator for multi-step flows where orientation matters: onboarding (4 steps), agent setup wizard (Configure → Connect → Test → Publish), checkout (Cart → Details → Review → Confirm). It communicates how far the user has come and how far they have left to go — reducing uncertainty and providing a sense of momentum.

## When NOT to Use

Do not use Progress Indicator for fewer than 3 steps — it adds overhead without value. Do not use Progress Indicator for non-linear flows where steps can be skipped or revisited freely. Do not use Progress Indicator for status tracking of a system process (file upload, build status) — use Progress Bar instead.

## Anatomy

- **Step dot/circle:** Numbered or icon-filled circle for each step
- **Completed step:** Teal fill (`cv-bg-accent`), white checkmark icon
- **Current step:** Teal stroke circle with number or icon, teal label
- **Upcoming step:** `cv-bg-surface-sunken` fill, `cv-text-tertiary` label
- **Connector line:** Horizontal line between dots, `cv-border-default` (upcoming), `cv-bg-accent` (completed)
- **Step label:** Caption size, `cv-text-primary` (current), `cv-text-tertiary` (upcoming), `cv-text-secondary` (completed)

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Numbered dots | Circles with step numbers | Short flows (3–5 steps) |
| Icon dots | Circles with step-specific icons | Onboarding with distinct visual steps |
| Simple dots | Small filled/hollow dots without numbers | Minimal carousel-style progress |
| Horizontal | Steps spread horizontally | Standard wizard header |
| Vertical | Steps stacked vertically | Side panel stepper, detailed step descriptions |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Upcoming step | Hollow, cv-text-tertiary | Future, available |
| Current step | Teal stroke, bold label | Present, focused |
| Completed step | Teal fill, checkmark | Accomplished, done |
| Clickable completed | Hover underline on label | Return to edit previous step |
| Non-clickable | No pointer cursor | Linear, forward only |

## Chatverce Styling

- Completed dot: `cv-bg-accent`, white check icon
- Current dot: 2px `cv-border-accent` stroke, number `cv-text-accent`, label `cv-text-accent` weight 500
- Upcoming dot: `cv-bg-surface-sunken`, number/icon `cv-text-tertiary`, label `cv-text-tertiary`
- Connector line: `cv-bg-accent` (completed portion), `cv-border-default` (upcoming portion)
- Dot diameter: 28–32px
- Label: Caption, 11–12px

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Build a custom component — shadcn/ui does not include a step indicator out of the box. Use Flexbox with a relative-positioned connector line behind the dots. If returning to previous steps is allowed, wrap dots in `<button>` elements.

### iOS (Expo + NativeWind)
Render as a horizontal `View` with `flexDirection: "row"` and centered alignment. Connector lines are `View` elements with absolute positioning or flexGrow between dots. Use `Animated.spring` for the dot fill transition when a step completes.

### Android (Expo + NativeWind)
Same as iOS. Android Material also uses horizontal stepper patterns; the custom React Native approach matches Chatverce styling better than any native equivalent.

## Accessibility

- Web: `role="list"` on the step container, `role="listitem"` per step. Current step has `aria-current="step"`. Completed steps indicate they are done via `aria-label="Step 1: Configure — completed"`. Screen readers should be able to navigate the step list.
- Mobile: `accessibilityRole="progressbar"` on the container with `accessibilityValue={{ min: 1, max: totalSteps, now: currentStep }}`. Each dot/step has `accessibilityLabel` describing its name and state.

## Code Examples

### Web

```tsx
import { Check } from "lucide-react";

const STEPS = ["Configure", "Connect", "Test", "Publish"];

function AgentSetupProgress({ currentStep }) {
  return (
    <nav aria-label="Setup progress">
      <ol role="list" className="flex items-center justify-between w-full max-w-lg mx-auto">
        {STEPS.map((step, index) => {
          const stepNum = index + 1;
          const completed = stepNum < currentStep;
          const current = stepNum === currentStep;
          const upcoming = stepNum > currentStep;

          return (
            <li key={step} className="flex items-center flex-1" aria-current={current ? "step" : undefined}>
              <div className="flex flex-col items-center gap-1.5">
                <div className={`
                  w-8 h-8 rounded-full flex items-center justify-center text-sm font-medium transition-colors
                  ${completed ? "bg-[--cv-bg-accent]" : ""}
                  ${current ? "border-2 border-[--cv-border-accent] text-[--cv-text-accent]" : ""}
                  ${upcoming ? "bg-[--cv-bg-surface-sunken] text-[--cv-text-tertiary]" : ""}
                `}>
                  {completed ? <Check size={14} className="text-white" strokeWidth={3} /> : stepNum}
                </div>
                <span className={`text-xs ${current ? "text-[--cv-text-accent] font-medium" : upcoming ? "text-[--cv-text-tertiary]" : "text-[--cv-text-secondary]"}`}>
                  {step}
                </span>
              </div>
              {index < STEPS.length - 1 && (
                <div className={`flex-1 h-0.5 mx-2 mb-5 ${completed ? "bg-[--cv-bg-accent]" : "bg-[--cv-border-default]"}`} />
              )}
            </li>
          );
        })}
      </ol>
    </nav>
  );
}
```

### Mobile

```tsx
import { View, Text } from "react-native";
import { Check } from "lucide-react-native";

const STEPS = ["Configure", "Connect", "Test", "Publish"];

function AgentSetupProgressMobile({ currentStep }) {
  return (
    <View
      className="flex-row items-start px-4 py-3"
      accessibilityRole="progressbar"
      accessibilityValue={{ min: 1, max: STEPS.length, now: currentStep }}
      accessibilityLabel={`Step ${currentStep} of ${STEPS.length}: ${STEPS[currentStep - 1]}`}
    >
      {STEPS.map((step, index) => {
        const stepNum = index + 1;
        const completed = stepNum < currentStep;
        const current = stepNum === currentStep;

        return (
          <View key={step} className="flex-1 items-center">
            <View className="flex-row items-center w-full">
              <View style={{
                width: 28, height: 28, borderRadius: 14,
                backgroundColor: completed ? "var(--cv-bg-accent)" : "var(--cv-bg-surface-sunken)",
                borderWidth: current ? 2 : 0,
                borderColor: current ? "var(--cv-border-accent)" : "transparent",
                alignItems: "center", justifyContent: "center",
              }}
                accessibilityLabel={`Step ${stepNum}: ${step} — ${completed ? "completed" : current ? "current" : "upcoming"}`}
              >
                {completed
                  ? <Check size={12} color="white" strokeWidth={3} />
                  : <Text style={{ color: current ? "var(--cv-text-accent)" : "var(--cv-text-tertiary)", fontSize: 12, fontWeight: "500" }}>{stepNum}</Text>
                }
              </View>
              {index < STEPS.length - 1 && (
                <View style={{ flex: 1, height: 2, backgroundColor: completed ? "var(--cv-bg-accent)" : "var(--cv-border-default)", marginHorizontal: 4 }} />
              )}
            </View>
            <Text style={{ fontSize: 10, marginTop: 4, color: current ? "var(--cv-text-accent)" : "var(--cv-text-tertiary)", fontWeight: current ? "500" : "400" }}>
              {step}
            </Text>
          </View>
        );
      })}
    </View>
  );
}
```

## Anti-Patterns

- Never use Progress Indicator for fewer than 3 steps — a header label is sufficient.
- Never show more than 7 steps — the indicator becomes too compressed to read. Break into phases if needed.
- Never truncate step labels — at small sizes, use a compact variant with number-only dots.
- Never leave the user without a way to go back to previous steps unless the flow is truly one-directional and irreversible.
- Never use Progress Indicator for process status (upload, build) — that is Progress Bar's role.
