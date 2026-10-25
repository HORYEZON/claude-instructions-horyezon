# Parallel sessions & commit scope

The user frequently runs **multiple Claude sessions against the same repo at once**. Those sessions commit with `git add -A`, which sweeps in *this* session's uncommitted work-in-progress under their commit message.

**Why:** commits end up mixing unrelated features and wrong co-authors; a file you're mid-edit on can be reverted or committed out from under you.

**How to apply:**

- Commit your own work promptly once it builds — don't leave large WIP sitting uncommitted across turns.
- When committing, `git add` explicit paths you changed, not `git add -A`.
- Before committing, `git status` + check `git log --oneline origin/main..HEAD` and diff unfamiliar files — a parallel session may have already pushed, or swept your edits into their commit. `git pull --rebase` before push.
- The user is fine with "commit everything" when features are entangled in one file — but ask first.
