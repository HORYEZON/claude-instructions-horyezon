# Per-task delivery workflow

Every time a task the user gives is finished, run this delivery workflow. Also run it immediately whenever the user says the trigger keyword **"Ship It!"** or **"Push It!"** (treat those phrases as an explicit command to execute all steps below now):

1. **Code testing** — run the full pre-push check suite (`npx tsc --noEmit`, `yarn next lint`, `yarn build`) and fix all new errors.
2. **Update docs** — update `README.md`, `Docs/User_Training.md`, and `Docs/Progress_Timeline.md` to reflect the change, following each file's existing markdown structure/arrangement (don't restructure them). The folder is `Docs/` with a capital D.
3. **Branch** — create a git branch named `<name-date>` where `name` is a short kebab-case task name and `date` is `MMDDYY` (e.g. task finished 2026-09-02 → `arcade-room-090226`).
4. **Commit** — stage explicit paths (see the parallel-sessions rule) and write a commit with a clear summary line + a description body of what changed and why.
5. **Merge to main** — merge the branch into `main`.
6. **Push** — push `main`.
7. **Release note** — if the change is visitor-facing, file a draft one with `yarn release-note` (see the public-facing release notes rule).

**Why:** the user wants each task shipped end-to-end on its own dated branch with a proper summary/description, tested before it lands, then merged and pushed without a separate round-trip.

**How to apply:** treat "task done" as also meaning "test, docs, branch, commit, merge, push." Still honor confirm-before-push norms and the parallel-session cautions. Never violate the destructive-git guardrail. Also save/update a project memory when a task is created and when it is finished.
