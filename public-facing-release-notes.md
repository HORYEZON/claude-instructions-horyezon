# Public-facing release notes

Every push that changes something a **visitor** can see or do — Gallery, Artworks, Stories/Tales, Contact, About, Shop, Freedom Wall, Mini Games, Digital Museum, the public header/menu/hero — must also file a Release Note. Run it as the last step of the per-task delivery workflow, after the push.

```
yarn release-note --title "…" --body "…" --category "Digital Museum" --version "v6.26"
```

`scripts/create-release-note.ts` writes straight to the DB (the admin POST route is behind `requireAdmin()`, which needs a browser session) and reuses `lib/release-notes.ts`'s own sanitizers, so a note filed this way is validated exactly like one typed into Settings ▸ Release Notes.

**Always a draft.** The script defaults to `isPublished: false` and the user publishes it themselves after reading the wording. Do **not** pass `--publish` unless they explicitly ask — `DATABASE_URL` is the hosted prod DB, so a published note is live to visitors immediately.

**Scope:** the user chose *anything* public-facing, bug fixes included — a click-away fix counts. Skip only admin-only work, refactors, docs and internal changes. Categories come from `RELEASE_NOTE_CATEGORIES` in the project's `lib/release-notes.ts` (e.g. Digital Museum, Gallery, Stories or Tales, Shop, Freedom Wall, Mini Games, General) — check the file, the list differs slightly per project.

**Why:** the user maintains Settings ▸ Release Notes as the visitor-facing "what's new" panel and does not want to remember to write one for each shipped change. Draft-first because that panel's wording is meant to be plain rather than technical, and they want the final say on how it reads.

**How to apply:** write the body the way a visitor would describe it — what they can now see or do — not the way the commit message does. No file names, no component names, no jargon. Mention any visible change they might otherwise think is a bug (e.g. "the Arcade marquees and poster captions now match"). One note per shipped change, filed right after the push, then tell them it's waiting for review.
