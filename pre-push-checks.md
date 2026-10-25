# Pre-push checks

Before every `git push`, run all three checks in order and fix any errors before committing:

1. **TypeScript** — `npx tsc --noEmit`
2. **ESLint** — `yarn next lint`
3. **Production build** — `yarn build`

Only push once all three pass. Pre-existing warnings (e.g. `<img>` tags in AdminSidebar, AnnouncementPopup, Navbar) are acceptable noise — errors are not.

**Why:** Vercel's build pipeline fails on ESLint errors (e.g. `react/no-unescaped-entities`) that tsc alone won't catch, causing broken deployments even when TypeScript is clean.

**How to apply:** after staging changes and before `git commit && git push`, run the three commands above. Fix any new errors (not pre-existing warnings) before proceeding. This is step 1 of the per-task delivery workflow.
