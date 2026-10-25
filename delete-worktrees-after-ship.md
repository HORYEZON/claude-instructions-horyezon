# Delete my worktrees after shipping

When a task is worked in a separate git worktree beside a project in this folder (named `<project>-<task>`, e.g. `scriptovernovel.music-saved`), **delete it once the task is merged and pushed**. The owner gave a standing OK on 10/20/26: "next time kapag mag separate folder ka delete mo na lang para malinis".

**Scope:** only the `<project>-<task>` folders I created. Never touch anything else here: the main project folders, WL.HRZ, gods-eye-view, kstudio, questboard, "Backups : DB Dumps", or claude-instructions.

**Why:** worktrees keep work out of the shared folder that VS Code and parallel sessions use. But they hide the changes from the owner's Source Control, and stale ones piled up unseen.

**How to apply:**
- Before deleting, check that `git -C <wt> status --porcelain` is empty and that HEAD is in `origin/main`.
- Run `git -C <main repo> worktree remove <wt>`. Add `--force` only if an untracked symlink blocks it; `.env` there is a symlink, so removing it only drops the link.
- Clone node_modules into the worktree with `cp -cR`; never symlink it, or `prisma generate` rewrites the shared client.
- In the ship summary, name the folder, say it's deleted, and say the changes are visible as commits in Source Control Graph.
