---
name: datepicker
description: Calendar-based date selection control with platform-appropriate UX on web, iOS, and Android
user_invocable: false
---

# Datepicker

**Gallery reference:** https://component.gallery/components/datepicker/
**Aliases:** Calendar picker, Date selector, Date picker, Calendar input

## When to Use

Use Datepicker when the user needs to select a specific date from a calendar — scheduling, report date ranges, setting a trial end date, filtering by date. Datepicker provides a visual calendar that makes date context (day of week, proximity to today) immediately clear.

## When NOT to Use

Do not use Datepicker for date of birth where the user knows their date exactly — a Text Input with date format hint (Date Input component) is faster. Do not use Datepicker for year-only selection — use a Select. Do not use Datepicker for time-only selection — use a time picker input.

## Anatomy

- **Trigger:** Text Input appearance, displays selected date or placeholder, calendar icon trailing
- **Calendar panel (web):** Month grid, prev/next navigation, today highlighted, selected day teal
- **Time picker (optional):** Below calendar, hour + minute selects
- **Range handles (date-range variant):** Start and end day selection with teal range fill

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Single date | Select one day | Due dates, scheduled send, start date |
| Date range | Select start + end day | Report periods, booking, trial duration |
| Date + time | Date + time selector | Scheduled messages, calendar events |
| Inline | Calendar displayed always (not in popover) | Booking interfaces, dedicated date screens |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default (closed) | Trigger with placeholder | Ready, date expected |
| Open | Calendar popover visible | Choosing |
| Day hover | Light teal bg on hovered day | Responsive |
| Today | Teal ring on today's date | Orientation |
| Selected | Teal fill, white text | Confirmed |
| Range selected | Teal fill start/end, cv-bg-accent-subtle fill between | Range clear |
| Past dates (if disabled) | cv-text-tertiary, not clickable | Not selectable |
| Focus | 2px cv-border-accent ring on trigger | Keyboard-accessible |

## Chatverce Styling

- Calendar day: 32px circle, `cv-bg-accent` when selected
- Today ring: 1.5px `cv-border-accent`
- Range fill: `cv-bg-accent-subtle`
- Calendar bg: `cv-bg-surface-elevated`
- Calendar shadow: `cv-shadow-md`
- Trigger: same as Text Input (`cv-bg-surface-sunken`, `cv-border-default`)

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use shadcn/ui `<Calendar>` + `<Popover>` pattern (powered by `react-day-picker`). Calendar handles keyboard navigation (arrow keys, Page Up/Down for month), ARIA grid, and date formatting.

### iOS (Expo + NativeWind)
Use `@react-native-community/datetimepicker` with `mode="date"`. On iOS, this renders a native wheel or calendar UI (depending on iOS version) via `display="spinner"` or `display="inline"`. Wrap in a Modal or BottomSheet with a "Done" button to confirm selection.

### Android (Expo + NativeWind)
Use `@react-native-community/datetimepicker` with `display="default"` — Android shows the Material date picker dialog automatically. The dialog has native confirm/cancel buttons.

## Accessibility

- Web: `<Calendar>` uses `role="grid"` for the month grid, `role="gridcell"` for days, `aria-selected` for selected day, `aria-current="date"` for today. The trigger input has `aria-haspopup="dialog"` and `aria-expanded`.
- Mobile: Native datepicker components are fully accessible via VoiceOver/TalkBack. The trigger Pressable must have `accessibilityRole="button"` and `accessibilityLabel` including the current value.

## Code Examples

### Web

```tsx
import { useState } from "react";
import { format } from "date-fns";
import { CalendarIcon } from "lucide-react";
import { Calendar } from "@/components/ui/calendar";
import { Popover, PopoverContent, PopoverTrigger } from "@/components/ui/popover";
import { Button } from "@/components/ui/button";

function DatePickerField({ label, value, onChange }) {
  const [open, setOpen] = useState(false);

  return (
    <div className="space-y-1.5">
      <label className="text-[--cv-text-primary] text-sm font-medium">{label}</label>
      <Popover open={open} onOpenChange={setOpen}>
        <PopoverTrigger asChild>
          <Button
            variant="outline"
            aria-haspopup="dialog"
            aria-expanded={open}
            className="w-full justify-start bg-[--cv-bg-surface-sunken] border-[--cv-border-default] rounded-[--cv-radius-md] text-left font-normal h-10"
          >
            <CalendarIcon size={16} className="mr-2 text-[--cv-text-secondary]" />
            {value
              ? <span className="text-[--cv-text-primary]">{format(value, "PPP")}</span>
              : <span className="text-[--cv-text-tertiary]">Pick a date</span>
            }
          </Button>
        </PopoverTrigger>
        <PopoverContent className="w-auto p-0 bg-[--cv-bg-surface-elevated] shadow-[--cv-shadow-md] rounded-[--cv-radius-md]">
          <Calendar
            mode="single"
            selected={value}
            onSelect={(date) => { onChange(date); setOpen(false); }}
            className="text-[--cv-text-primary]"
            classNames={{
              day_selected: "bg-[--cv-bg-accent] text-white hover:bg-[--cv-bg-accent-hover]",
              day_today: "border border-[--cv-border-accent] text-[--cv-text-accent]",
              nav_button: "text-[--cv-text-secondary] hover:text-[--cv-text-primary]",
            }}
          />
        </PopoverContent>
      </Popover>
    </div>
  );
}
```

### Mobile

```tsx
import { View, Text, Pressable, Modal, Platform } from "react-native";
import DateTimePicker from "@react-native-community/datetimepicker";
import { format } from "date-fns";
import { Calendar } from "lucide-react-native";
import { useState } from "react";

function CvDatepicker({ label, value, onChange }) {
  const [show, setShow] = useState(false);
  const [tempDate, setTempDate] = useState(value || new Date());

  const handleChange = (event, selectedDate) => {
    if (Platform.OS === "android") {
      setShow(false);
      if (selectedDate && event.type !== "dismissed") onChange(selectedDate);
    } else {
      setTempDate(selectedDate || tempDate);
    }
  };

  return (
    <View className="mb-5">
      <Text className="text-[--cv-text-primary] text-sm font-medium mb-1.5">{label}</Text>
      <Pressable
        className="flex-row items-center bg-[--cv-bg-surface-sunken] border border-[--cv-border-default] rounded-[--cv-radius-md] px-4 min-h-[44px] gap-3"
        onPress={() => setShow(true)}
        accessibilityRole="button"
        accessibilityLabel={`${label}: ${value ? format(value, "PPP") : "not selected"}`}
      >
        <Calendar size={16} color="var(--cv-text-secondary)" />
        <Text style={{ color: value ? "var(--cv-text-primary)" : "var(--cv-text-tertiary)", fontSize: 16 }}>
          {value ? format(value, "PPP") : "Pick a date"}
        </Text>
      </Pressable>

      {/* iOS: Modal with inline calendar */}
      {Platform.OS === "ios" && (
        <Modal visible={show} transparent animationType="slide">
          <View className="flex-1 justify-end bg-black/40">
            <View className="bg-[--cv-bg-surface-elevated] rounded-t-[--cv-radius-lg] pb-8">
              <View className="flex-row justify-between items-center px-4 py-3 border-b border-[--cv-border-default]">
                <Pressable onPress={() => setShow(false)}>
                  <Text className="text-[--cv-text-secondary] text-base">Cancel</Text>
                </Pressable>
                <Pressable onPress={() => { onChange(tempDate); setShow(false); }}>
                  <Text className="text-[--cv-text-accent] text-base font-semibold">Done</Text>
                </Pressable>
              </View>
              <DateTimePicker value={tempDate} mode="date" display="spinner" onChange={handleChange} />
            </View>
          </View>
        </Modal>
      )}

      {/* Android: native dialog */}
      {Platform.OS === "android" && show && (
        <DateTimePicker value={value || new Date()} mode="date" display="default" onChange={handleChange} />
      )}
    </View>
  );
}
```

## Anti-Patterns

- Never use Datepicker for known dates the user memorizes (date of birth) — Text Input with date format mask is faster.
- Never use a text input alone for date entry without validation — users enter many different date formats.
- Never disable future dates without explaining why (e.g., "Past dates only — for historical reports").
- Never use a single calendar component for date + time — combine Calendar + time Select or use a dedicated Date+Time variant.
- Never leave the calendar open after the user selects a date — auto-close provides clear confirmation.
