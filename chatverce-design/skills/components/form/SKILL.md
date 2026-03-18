---
name: form
description: Container that orchestrates data collection with consistent layout, validation, and submission patterns
user_invocable: false
---

# Form

**Gallery reference:** https://component.gallery/components/form/
**Aliases:** Form container, Field set, Data entry form, Input form

## When to Use

Use Form whenever collecting structured input from the user: creating or editing an agent, account settings, billing details, team invitations, onboarding flows. Form is the container that coordinates the layout, validation state, and submission behavior of all input controls within it.

## When NOT to Use

Do not build free-floating input fields without a Form wrapper — validation, error state, and submission become inconsistent. Do not put a Form inside a Form (no nesting). Do not use Form for read-only display — use a description list or Card instead.

## Anatomy

- **Form container:** Semantic `<form>` element (web) with submit handler
- **Field groups:** One or more Fieldsets grouping related inputs with a legend
- **Labels:** Above each input, never placeholder-only
- **Inputs:** text-input, textarea, select, checkbox, radio, toggle, etc.
- **Helper text:** Below input, `cv-text-secondary`, Caption size
- **Validation messages:** Below input, `cv-text-error`, Caption size, with error icon
- **Submit area:** Sticky or inline, one Primary button + optional Ghost cancel

## Variants

| Variant | Description | When to Use |
|---------|-------------|-------------|
| Single column | Full-width stacked fields | Default for all forms — most readable |
| Two column | Side-by-side fields for short related inputs | First name + last name, start date + end date |
| Inline | Label beside input | Compact settings rows in a table-like layout |
| Progressive | Steps revealed as previous steps complete | Complex setup flows (agent builder wizard) |
| Modal form | Form inside a Modal | Quick create / invite — max 4 fields |

## Interaction States

| State | Visual | Emotional Intention |
|-------|--------|---------------------|
| Default | Fields resting, submit enabled | Open, ready |
| Validation error | Error fields highlighted, summary at top | Helpful, not punishing |
| Submitting | Submit button in loading state, inputs disabled | Processing, feedback given |
| Success | Toast confirmation, form resets or closes | Completed, clear |
| Dirty unsaved | Optional: alert on navigate-away | Protective, prevents data loss |

## Chatverce Styling

- Layout: single-column, max-width 480px (standalone), full-width within panels/modals
- Field spacing: 20px between fields, 32px between fieldset groups
- Labels: `cv-text-primary`, Body size (14–16px), margin-bottom 4–6px
- Helper text: `cv-text-secondary`, Caption (12–13px), margin-top 4px
- Error text: `cv-text-error`, Caption, with AlertCircle icon, margin-top 4px
- Submit area: margin-top 32px, right-aligned (web), full-width stacked (mobile)

## Platform Considerations

### Web (Next.js + shadcn/ui + Tailwind)
Use `react-hook-form` + `zod` for form management and validation. Use shadcn/ui form primitives (`<Form>`, `<FormField>`, `<FormItem>`, `<FormLabel>`, `<FormControl>`, `<FormMessage>`). These wire up `aria-invalid`, `aria-describedby`, and validation messages automatically.

### iOS (Expo + NativeWind)
Use `react-hook-form` (works in React Native). Replace shadcn form wrappers with custom View/Text composition. Ensure `returnKeyType` on TextInput moves focus to the next field (`returnKeyType="next"`, `onSubmitEditing={() => nextRef.current?.focus()`).

### Android (Expo + NativeWind)
Same as iOS. Set `android:windowSoftInputMode="adjustResize"` (via `app.json` `android.softwareKeyboardLayoutMode`) to prevent the keyboard from covering inputs.

## Accessibility

- Web: `<form>` element with `aria-label` or `aria-labelledby`. Each input has `<label>` with `htmlFor`. Validation errors use `aria-invalid="true"` on the input and `aria-describedby` pointing to the error message element.
- Mobile: Each input has `accessibilityLabel`. Error state communicates via `accessibilityLiveRegion="assertive"` on the error message. Use `ref` chains to move focus through fields.

## Code Examples

### Web

```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import * as z from "zod";
import { Form, FormField, FormItem, FormLabel, FormControl, FormMessage, FormDescription } from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";

const agentSchema = z.object({
  name: z.string().min(2, "Name must be at least 2 characters"),
  description: z.string().optional(),
});

function CreateAgentForm({ onSubmit }) {
  const form = useForm({ resolver: zodResolver(agentSchema) });
  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-5 max-w-[480px]">
        <FormField
          control={form.control}
          name="name"
          render={({ field }) => (
            <FormItem>
              <FormLabel className="text-[--cv-text-primary] text-sm font-medium">Agent Name</FormLabel>
              <FormControl>
                <Input {...field} className="bg-[--cv-bg-surface-sunken] border-[--cv-border-default] rounded-[--cv-radius-md] text-base focus:border-[--cv-border-accent] focus:ring-2 focus:ring-[--cv-border-accent]/20" />
              </FormControl>
              <FormDescription className="text-[--cv-text-secondary] text-xs">
                This name appears in your dashboard and in conversations.
              </FormDescription>
              <FormMessage className="text-[--cv-text-error] text-xs flex items-center gap-1" />
            </FormItem>
          )}
        />
        <div className="flex justify-end gap-3 pt-4">
          <Button type="button" variant="ghost" className="text-[--cv-text-primary]">Cancel</Button>
          <Button type="submit" className="bg-[--cv-bg-accent] text-[--cv-text-on-accent]" disabled={form.formState.isSubmitting}>
            {form.formState.isSubmitting ? "Creating…" : "Create Agent"}
          </Button>
        </div>
      </form>
    </Form>
  );
}
```

### Mobile

```tsx
import { useForm, Controller } from "react-hook-form";
import { View, Text, TextInput, Pressable, ScrollView, KeyboardAvoidingView, Platform } from "react-native";

function CreateAgentFormMobile({ onSubmit }) {
  const { control, handleSubmit, formState: { errors, isSubmitting } } = useForm();
  return (
    <KeyboardAvoidingView behavior={Platform.OS === "ios" ? "padding" : "height"} className="flex-1">
      <ScrollView className="flex-1 px-4 py-6" keyboardShouldPersistTaps="handled">
        <Controller
          control={control}
          name="name"
          rules={{ required: "Name is required", minLength: { value: 2, message: "At least 2 characters" } }}
          render={({ field: { onChange, value, ref } }) => (
            <View className="mb-5">
              <Text className="text-[--cv-text-primary] text-sm font-medium mb-1.5" accessibilityRole="text">
                Agent Name
              </Text>
              <TextInput
                ref={ref}
                value={value}
                onChangeText={onChange}
                className="bg-[--cv-bg-surface-sunken] border border-[--cv-border-default] rounded-[--cv-radius-md] px-4 py-3 text-base text-[--cv-text-primary]"
                style={{ borderColor: errors.name ? "var(--cv-border-error)" : undefined }}
                accessibilityLabel="Agent name"
                accessibilityInvalid={!!errors.name}
              />
              {errors.name && (
                <Text className="text-[--cv-text-error] text-xs mt-1" accessibilityLiveRegion="assertive">
                  {errors.name.message}
                </Text>
              )}
            </View>
          )}
        />
        <Pressable
          className="bg-[--cv-bg-accent] rounded-[--cv-radius-md] min-h-[44px] items-center justify-center mt-4"
          onPress={handleSubmit(onSubmit)}
          disabled={isSubmitting}
          accessibilityRole="button"
          accessibilityLabel="Create agent"
        >
          <Text className="text-[--cv-text-on-accent] font-medium">{isSubmitting ? "Creating…" : "Create Agent"}</Text>
        </Pressable>
      </ScrollView>
    </KeyboardAvoidingView>
  );
}
```

## Anti-Patterns

- Never use placeholder text as the only label — placeholders disappear on focus and are not read by all screen readers.
- Never place a form submit button far from the last input without visual connection.
- Never validate the entire form on first keystroke — validate on blur for non-destructive fields, or on submit.
- Never use multi-column layouts for mobile — always collapse to single column below 640px.
- Never omit the loading state on submit — a form that appears frozen after submission destroys trust.
