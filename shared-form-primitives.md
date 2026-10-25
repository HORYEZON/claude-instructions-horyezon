# Shared form primitives (AdminSelect / AdminDatePicker / CalendarPanel)

Form controls in the admin go through the shared primitives, never the browser's native widget:

- `components/admin/AdminSelect.tsx` — every dropdown. See the select-chevron pattern below.
- `components/admin/AdminDatePicker.tsx` — the date/datetime field. Keeps the native value formats (`YYYY-MM-DD`, or `YYYY-MM-DDTHH:mm` with `withTime`), so replacing a native input needs no other change to the form.
- `components/admin/CalendarPanel.tsx` — the month grid, shared by `AdminDatePicker` and the sidebar's `DashboardCalendar`.

**Why:** the user wants one UI/UX across the whole admin, matched to the Dashboard calendar. Every browser draws its own date picker and its own select caret, so native controls guarantee the pages don't match each other — and the native caret sits flush against the field's border.

**How to apply:** before adding any dropdown or date field, reach for the primitive. If a new one is needed, extract it into `components/admin/` rather than styling it inline, and point the README's "Standing rules" table at it.

## Select chevron pattern

Never ship a bare `<select>` with the browser's native arrow — it crowds the value and the border (the "dikit" the user keeps reporting: the native caret has no padding of its own, so it sits flush against the input's right edge). Use `AdminSelect`, which is this pattern packaged. Only hand-roll it where a shared component can't reach:

```tsx
<div className="relative">
  <select className="admin-input w-full pl-3 pr-9 py-2 rounded-xl text-sm cursor-pointer appearance-none">
    <option className="bg-white dark:bg-ink-900">…</option>
  </select>
  <ChevronDown size={14} className="pointer-events-none absolute right-3 top-1/2 -translate-y-1/2 text-ink-400" />
</div>
```

(The wider variant used elsewhere is `px-4 py-2.5 pr-10` with the icon at `right-4`.)

Before adding or reviewing any dropdown, grep the codebase for an existing `appearance-none` select and copy that exact class string + icon placement rather than inventing new spacing. Same rule for any other control where a browser-drawn affordance would sit inside our input box.
