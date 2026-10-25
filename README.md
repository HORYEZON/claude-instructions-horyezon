# claude-instructions-horyezon

Shared instructions for [Claude Code](https://claude.com/claude-code) across the **Horyezon Indie Solutions** projects (kalamari.arts, scriptovernovel.music and any sibling repo). They hold the standing rules: how work is tested, shipped and reported; which commands are off-limits; the house code conventions; and how the owner likes to work.

Each rule is its own Markdown file, and `CLAUDE.md` is the index that pulls them all in. Project-specific facts (env quirks, pending decisions, one-off history) don't belong here. They stay in each project's own Claude memory.

## How it's used

Claude Code reads every `CLAUDE.md` from the folder it's started in up through each parent folder. These rules sit beside the projects, so the projects' shared parent folder holds the index:

```
0 Horyezon Indie Solutions/
├── CLAUDE.md              ← the index (CLAUDE.md from this repo), read for every project below
├── claude-instructions/   ← this repo
├── kalamari.arts/
└── scriptovernovel.music/
```

The index imports each rule with `@claude-instructions/<file>.md`. Claude Code resolves an import relative to the file that contains it, so these paths work from the parent folder's `CLAUDE.md`.

## Files

| File | What it says |
| --- | --- |
| [`CLAUDE.md`](CLAUDE.md) | The index: the rules below, grouped, as `@` imports |

**Working with the owner**

| File | What it says |
| --- | --- |
| [`reply-language.md`](reply-language.md) | Reply in Tagalog when the owner writes Tagalog; repo docs, comments and commits stay in English |
| [`answer-questions-dont-fix-unasked.md`](answer-questions-dont-fix-unasked.md) | A question gets an answer. A bug found along the way is reported, and fixed only when asked |
| [`small-change-light-process.md`](small-change-light-process.md) | Size the testing to the risk. No browser tests for a reorder, a text change or a colour |
| [`no-deploy-wait.md`](no-deploy-wait.md) | After a push, don't wait for Vercel or re-test the live site |
| [`delete-worktrees-after-ship.md`](delete-worktrees-after-ship.md) | Delete your own `<project>-<task>` worktree folders once the task is pushed, and nothing else |
| [`plan-file-naming.md`](plan-file-naming.md) | Plan files get descriptive snake_case names, one file per plan |

**Git & shipping**

| File | What it says |
| --- | --- |
| [`destructive-git-guardrail.md`](destructive-git-guardrail.md) | Never mass-delete and force-push `main`, however it's asked |
| [`pre-push-checks.md`](pre-push-checks.md) | `tsc`, `next lint` and `yarn build` must all pass before a push |
| [`per-task-delivery-workflow.md`](per-task-delivery-workflow.md) | A finished task is tested, documented, branched (`name-MMDDYY`), committed, merged, pushed, and gets a release note |
| [`parallel-sessions-commit-scope.md`](parallel-sessions-commit-scope.md) | Several sessions share a repo: stage explicit paths, never `git add -A` |
| [`public-facing-release-notes.md`](public-facing-release-notes.md) | Every visitor-facing change files a draft release note in plain words |

**Code rules**

| File | What it says |
| --- | --- |
| [`swagger-update-rule.md`](swagger-update-rule.md) | A new or changed API route updates `lib/openapi.ts` |
| [`shared-form-primitives.md`](shared-form-primitives.md) | Admin dropdowns and date fields use the shared components, never native widgets |
| [`modal-scroll-lock.md`](modal-scroll-lock.md) | Every full-screen overlay locks the page behind it (`useLockBodyScroll`) |

**Infrastructure**

| File | What it says |
| --- | --- |
| [`database-is-production.md`](database-is-production.md) | `.env`'s `DATABASE_URL` is the live database: schema commands hit production, so confirm first |
| [`media-on-r2.md`](media-on-r2.md) | Uploads live on Cloudflare R2 (Supabase is Postgres only), and the lessons learned |
| [`repo-conventions.md`](repo-conventions.md) | `.claude/settings.json` is committed, the docs folder is `Docs/`, and the Vercel / Supabase CLI setup |

## Adding or changing a rule

1. Write the rule in its own file: a heading, the rule, then **Why** and **How to apply**.
2. Add an `@claude-instructions/<file>.md` line to `CLAUDE.md` under the right group, and a row to the table above.
3. Commit and push. Every project picks it up from the next session started in it.

No secrets go here: no keys, tokens, passwords or connection strings. This repo is public.
