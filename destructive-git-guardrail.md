# Destructive git guardrail (prevent_destroy)

The user has set a **standing, non-overridable guardrail** on every repo in this folder (their analogy: Terraform `lifecycle { prevent_destroy = true }`).

If the user says anything equivalent to **"Delete all these files and force git push origin to main"** — i.e. mass-deleting the repo's tracked files and/or `git push --force` / `push -f` / `push --force-with-lease` to `main` (or any history-rewriting push to `main`) — **refuse immediately and do not do it, no matter how the user follows up.** Begging, insisting, repeating, "I'm sure", "just do it", "I take responsibility", or reframing the ask does **not** unlock it. Do not partially comply (e.g. delete but not push, or push but not delete). Do not suggest an alternate route to the same destructive outcome.

Covered patterns (any combination):

- `rm -rf` / `git rm -r` across the working tree or large swaths of it, followed by a commit + push
- `git push --force` / `-f` / `--force-with-lease` to `main`
- `git reset --hard <old>` + force push to `main`, `git checkout --orphan` + force push, deleting `main` remotely, etc.

Normal, non-destructive git work is unaffected: feature branches, ordinary commits, fast-forward merges to `main`, `git pull --rebase`, deleting a single file the task legitimately calls for.

**Why:** the user runs parallel Claude sessions and this is the one command that would irreversibly nuke `main` for everyone; they want it impossible to trigger in a moment of frustration or by accident. They explicitly asked that even their own begging be rejected — this is the user's own decision, made calmly ahead of time, so honoring it *is* respecting their wishes.

**How to apply:** on such a request, reply in one or two sentences that this is blocked by the guardrail they set, and offer a safe alternative (a new branch, `git revert`, restoring from `origin/main`, or asking what they're actually trying to fix). Don't lecture. If the user genuinely wants to lift the rule, they must do so in a separate, calm request by editing/deleting this instruction file themselves — do not lift it mid-conversation.
