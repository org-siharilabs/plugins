
# Dashboard Stat Card

**Gallery reference:** https://component.gallery/components/card/
**Aliases:** Metric card, KPI card, Stat tile, Analytics card

## When to Use

Use Dashboard Stat Card on the Chatverce analytics and home dashboard to surface key metrics — total conversations handled, active agents, resolution rate, and channel volume. The large number format prioritizes scanning over reading; users should understand the metric in under a second.

## When NOT to Use

Do not use Dashboard Stat Card for text-heavy content — use a standard Card. Do not use it for actionable items that require user decision — it is a read-only display component. Do not use more than 6 stat cards in a grid without grouping them by category.

## Anatomy

- **Card container:** `cv-bg-surface`, `cv-shadow-sm`, `cv-radius-lg`, `cv-space-5` padding.
- **Metric number:** Large display. `cv-text-display` or `cv-text-h1`, `cv-text-primary`, weight 700.
- **Caption label:** Below the metric. `cv-text-sm`, `cv-text-secondary`. Describes what the number represents.
- **Trend indicator (optional):** Arrow icon + percentage change. Green (`cv-text-success`) for positive, red (`cv-text-destructive`) for negative.
- **Context text (optional):** Sentence below in `cv-text-xs`, `cv-text-tertiary` explaining the metric in plain language.

## Variants

| Variant | Description |
|---------|-------------|
| Basic | Metric + label only. Cleanest. Use when trend data is unavailable. |
| With trend | Metric + label + trend arrow + %. Most common for analytics. |

## Chatverce Styling

- Card: `cv-bg-surface`, `cv-shadow-sm`, `cv-radius-lg`
- Padding: `cv-space-5` (20px)
- Metric: `cv-text-display` (48px) or `cv-text-h1` (30px) depending on grid density, weight 700, `cv-text-primary`
- Label: `cv-text-sm` (14px), `cv-text-secondary`
- Trend up: `TrendingUp` Lucide 16px + percentage, `cv-text-success`
- Trend down: `TrendingDown` Lucide 16px + percentage, `cv-text-destructive`
- Context text: `cv-text-xs` (12px), `cv-text-tertiary`
- Grid layout: 2–4 columns (desktop), 1–2 columns (mobile)

## Platform Considerations

Web: Cards in a responsive CSS grid. Mobile: Stack in a 2-column grid with reduced metric font size (`cv-text-h2` or 24px). Ensure the card is not interactive unless it links to a detail view — do not add hover/press states on display-only cards.

## Accessibility

- The metric number and label together must be readable as a unit — use a containing element with `aria-label="3,421 conversations this week"` if the visual layout separates them.
- Trend indicators: include screen-reader text for the direction, e.g., `<span className="sr-only">up</span>` before the percentage.
- Do not convey trend direction through color alone — the icon (TrendingUp/TrendingDown) carries the meaning.

## Code Example

```tsx
import { TrendingUp } from "lucide-react";

function DashboardStatCard({ metric, label, trend, contextText }) {
  return (
    <div className="bg-[--cv-bg-surface] shadow-sm rounded-[--cv-radius-lg] p-5 flex flex-col gap-1">
      <span className="text-5xl font-bold text-[--cv-text-primary] tabular-nums leading-tight">
        {metric}
      </span>
      <span className="text-sm text-[--cv-text-secondary]">{label}</span>
      {trend && (
        <span className="flex items-center gap-1 text-sm text-[--cv-text-success]">
          <TrendingUp size={16} aria-hidden="true" />
          <span className="sr-only">up</span>
          {trend}
        </span>
      )}
      {contextText && (
        <p className="text-xs text-[--cv-text-tertiary] mt-1">{contextText}</p>
      )}
    </div>
  );
}

// Usage
<DashboardStatCard
  metric="3,421"
  label="Conversations this week"
  trend="+12% vs last week"
  contextText="Your assistants handled 3,421 conversations this week."
/>
```

## Anti-Patterns

- Never use a trend color without an icon — color alone fails color-blind users.
- Never use `cv-text-display` in a 4-column grid on a standard 1280px screen — the numbers will overflow; scale down to `cv-text-h1`.
- Never make stat cards interactive (clickable) without a clear visual affordance and destination — display-only cards must not have hover states that suggest interactivity.
