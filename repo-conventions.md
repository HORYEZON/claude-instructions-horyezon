# Repo conventions

## `.claude/settings.json` rides along

`.claude/settings.json` is tracked in each repo and grows every session from "don't ask again" permission approvals. The user's standing choice is to just include it in the push rather than prune the allow list or move entries to `.claude/settings.local.json` (already ignored via the global `~/.config/git/ignore`).

**Why:** the entries are noise, not secrets, and the user would rather push than maintain the list.

**How to apply:** when `.claude/settings.json` is the only dirty file, commit it along with the work (or as a lone `chore:` commit) without re-raising the cleanup suggestion. The pre-push build/lint/tsc run adds nothing when no source file changed.

## Docs folder is `Docs/` (capital D)

The documentation folder is tracked in git as `Docs/` — capital D (`Docs/User_Training.md`, `Docs/Progress_Timeline.md`, `Docs/Media_Storage_R2.md`, …).

**Why:** on kalamari.arts the directory on disk once drifted to lowercase `docs` while the index had `Docs/`. With macOS `core.ignorecase=true`, editing tracked files worked, but `git add Docs/<newfile>` silently staged nothing — and on Vercel's case-sensitive Linux build a new doc would land in a second, lowercase directory, breaking relative links. Fixed 09/06/26 by `mv docs .tmp && mv .tmp Docs`.

**How to apply:** always write `Docs/`. If a new doc ever shows up as untracked `docs/...` in `git status`, the directory case has drifted — re-do the two-step rename rather than adding the lowercase path.

## Vercel & Supabase CLIs (machine-level)

Installed via Homebrew on 2026-09-04 (`brew tap supabase/tap && brew install vercel supabase`) because the npm global prefix `/usr/local` isn't writable without `sudo`. Vercel CLI is logged in as `horyezon` (team `horyezons-projects`).

**The gotcha:** brew's `vercel` formula pulls in its own `node` (26.x), which when linked sits earlier on PATH than the project's `/usr/local/bin/node` (v24). `brew unlink node` was run; `vercel` still works because its shebang is an absolute path.

**How to apply:** if `node -v` ever reports 26.x, brew's node got relinked — run `brew unlink node`. Don't `brew uninstall node` (`vercel` depends on it).
