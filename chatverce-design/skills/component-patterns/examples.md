# Component Examples

Working code snippets using shadcn + Tailwind + Chatverce design tokens (`cv-*` classes). Each example follows the patterns defined in the component-patterns skill.

## 1. Primary Button

```tsx
<Button className="bg-cv-primary text-white hover:bg-cv-primary/90 h-11 px-6 rounded-cv-md">
  Create assistant
</Button>
```

## 2. Secondary Button

```tsx
<Button variant="outline" className="border-cv-accent text-cv-accent hover:bg-cv-accent/5 h-11 px-6 rounded-cv-md">
  View details
</Button>
```

## 3. Ghost Button

```tsx
<Button variant="ghost" className="text-cv-text-secondary hover:text-cv-text-primary hover:bg-cv-surface h-11 px-4">
  Cancel
</Button>
```

## 4. Destructive Button

```tsx
<Button className="bg-cv-error text-white hover:bg-cv-error/90 h-11 px-6 rounded-cv-md">
  Delete assistant
</Button>
```

## 5. Card

```tsx
<div className="bg-cv-surface-elevated shadow-cv-sm rounded-cv-md p-5 hover:shadow-cv-md transition-shadow duration-150 ease-out">
  {/* Content-first: no decorative header */}
  <h3 className="font-inter font-medium text-base text-cv-text-primary">Card Title</h3>
  <p className="text-sm text-cv-text-secondary mt-1">Description</p>
</div>
```

## 6. Form Input with Focus State

```tsx
<div className="space-y-1.5">
  <label className="text-sm font-medium text-cv-text-primary">Assistant name</label>
  <input
    className="w-full h-11 px-3 text-sm rounded-cv-md border border-cv-border bg-cv-surface-elevated
    focus:border-cv-accent focus:ring-2 focus:ring-cv-accent/10 outline-none transition-colors"
    placeholder="e.g. Customer Support"
  />
</div>
```

## 7. Form Error State

```tsx
<div className="space-y-1.5">
  <label className="text-sm font-medium text-cv-text-primary">Email</label>
  <input className="w-full h-11 px-3 text-sm rounded-cv-md border border-cv-error bg-cv-surface-elevated" />
  <p className="text-xs text-cv-error">Enter a valid email address</p>
</div>
```

## 8. Navigation Sidebar Active Item

```tsx
<nav className="space-y-0.5">
  <a className="flex items-center gap-3 px-3 py-2 rounded-cv-md text-cv-accent bg-cv-accent/10 text-sm font-medium">
    <BotIcon size={20} strokeWidth={1.5} />
    Assistants
  </a>
  <a className="flex items-center gap-3 px-3 py-2 rounded-cv-md text-cv-text-secondary hover:text-cv-text-primary hover:bg-cv-surface text-sm">
    <InboxIcon size={20} strokeWidth={1.5} />
    Conversations
  </a>
</nav>
```

## 9. Empty State

```tsx
<div className="flex flex-col items-center justify-center py-16 px-4 text-center">
  {/* Simple line-art illustration */}
  <BotIcon size={48} strokeWidth={1} className="text-cv-text-tertiary mb-4" />
  <h2 className="text-lg font-semibold text-cv-text-primary">Create your first assistant</h2>
  <p className="text-sm text-cv-text-secondary mt-1 max-w-sm">
    Your assistant will handle customer conversations across all your channels.
  </p>
  <Button className="bg-cv-primary text-white h-11 px-6 rounded-cv-md mt-6">
    Create assistant
  </Button>
</div>
```

## 10. Modal

```tsx
<Dialog>
  <DialogContent className="bg-cv-surface-elevated shadow-cv-lg rounded-cv-lg p-6 animate-in fade-in zoom-in-95 duration-300">
    <DialogHeader>
      <DialogTitle className="text-lg font-semibold text-cv-text-primary">Delete assistant</DialogTitle>
      <DialogDescription className="text-sm text-cv-text-secondary mt-1">
        This will permanently delete the assistant and its conversation history.
      </DialogDescription>
    </DialogHeader>
    <DialogFooter className="mt-6 flex gap-3 justify-end">
      <Button variant="ghost">Cancel</Button>
      <Button className="bg-cv-error text-white">Delete assistant</Button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

## 11. Agent Builder Node Card

```tsx
<div className="bg-cv-surface-elevated shadow-cv-sm rounded-cv-md p-4 border-l-3 border-l-cv-accent w-56">
  <div className="flex items-center gap-2">
    <ZapIcon size={16} className="text-cv-accent" />
    <span className="text-xs font-medium text-cv-text-secondary uppercase tracking-wide">When this happens</span>
  </div>
  <p className="text-sm font-medium text-cv-text-primary mt-2">New WhatsApp message</p>
</div>
```

## 12. AI Working Indicator (Teal Pulse)

```tsx
<div className="flex items-center gap-2">
  <span className="relative flex h-2.5 w-2.5">
    <span className="animate-ping absolute inline-flex h-full w-full rounded-full bg-cv-accent opacity-75" />
    <span className="relative inline-flex rounded-full h-2.5 w-2.5 bg-cv-accent" />
  </span>
  <span className="text-sm text-cv-text-secondary">Setting things up...</span>
</div>
```
