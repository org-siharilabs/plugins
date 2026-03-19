
# Accordion

**Gallery reference:** https://component.gallery/components/accordion/
**Aliases:** Collapsible, Disclosure, Expandable, Details

## When to Use

Use Accordion when content falls into distinct categories and showing all at once would overwhelm the user. Ideal for FAQ sections, settings panels grouped by topic, and agent configuration options where only one category is relevant at a time.

## When NOT to Use

Do not use Accordion for content users need to compare across sections — collapsing hides the comparison. Do not collapse critical information (errors, required steps) that users must see. Do not use for navigation — use Tabs or a sidebar nav instead.

## Anatomy

- **Trigger row:** Full-width button. Label text + chevron/arrow icon on the right.
- **Chevron icon:** Rotates 180° when expanded. `cv-duration-normal` transition.
- **Content panel:** Revealed below trigger. Padding `cv-space-4` on sides, `cv-space-3` top/bottom.
- **Divider:** `cv-border-default` separating each item.

## Variants

| Variant | Description |
|---------|-------------|
| Single-expand | Only one panel open at a time. Opening one closes others. Default for FAQs. |
| Multi-expand | Multiple panels open simultaneously. Use when sections are independent and comparison is needed. |

## Chatverce Styling

- Container border: `cv-border-default`, `cv-radius-md`
- Trigger background: `cv-bg-surface` (resting), `cv-bg-surface-elevated` (hover)
- Trigger text: `cv-text-primary`, weight 500
- Chevron: `cv-text-secondary`, 16px, rotates 180° on expand
- Content background: `cv-bg-surface`
- Animation: height expand/collapse at `cv-duration-normal` (200ms), ease-out

## Platform Considerations

Web: Use shadcn/ui `<Accordion>` with `type="single"` or `type="multiple"`. Mobile: Use `Animated.View` with height interpolation or `LayoutAnimation`.

## Accessibility

- Trigger is a `<button>` with `aria-expanded="true|false"` and `aria-controls` pointing to the panel id.
- Panel has `role="region"` and `aria-labelledby` referencing the trigger id.
- Mobile: `accessibilityRole="button"`, `accessibilityState={{ expanded: bool }}`.

## Code Example

```tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";

<Accordion type="single" collapsible className="border border-[--cv-border-default] rounded-[--cv-radius-md]">
  <AccordionItem value="item-1">
    <AccordionTrigger className="px-4 text-[--cv-text-primary] font-medium hover:bg-[--cv-bg-surface-elevated]">
      What channels does Chatverce support?
    </AccordionTrigger>
    <AccordionContent className="px-4 pb-4 text-[--cv-text-secondary]">
      Chatverce supports WhatsApp, Telegram, Webchat, Phone, and Email channels.
    </AccordionContent>
  </AccordionItem>
</Accordion>
```

## Anti-Patterns

- Never nest accordions — this creates disorienting depth.
- Never animate with `display: none` toggling — always use height or opacity transitions.
- Never rely solely on the chevron icon to convey state; the `aria-expanded` attribute must also be set.
