# Media on Cloudflare R2 (Supabase = Postgres only)

All uploads (artwork images, `.glb` props, audio, video) live in one Cloudflare R2 bucket (`R2_BUCKET`, public origin `R2_PUBLIC_URL`). Supabase is **Postgres only** — none of its Storage keys are used. Storage code: `lib/storage/{r2,server,browser,cache}.ts`; sign routes return `uploadUrl` (presigned PUT), not `token`. Full record per project: `Docs/Media_Storage_R2.md`.

**R2 is the only copy of the media** — the Settings → Backup export (with media) is how to make another.

**Lessons learned (kalamari.arts, 09/2026):**

- `draco3dgltf` / `sharp` read their `.wasm` / native binaries via `__dirname` + `readFileSync`; webpack-bundled, the file isn't there → ENOENT → the route's non-fatal catch → raw URL stored, silently. **Any wasm/native package used in a route handler must be in `next.config` `serverExternalPackages`** (+ `outputFileTracingIncludes`). "Works under tsx" proves nothing about the Vercel bundle — verify with `next build && next start`.
- CORS on the bucket is load-bearing for museum WebGL textures.
- Upload-time compression lives in `lib/images/compress.ts` + `lib/images/variants.ts`; masters are kept in `originals/<id>.<ext>` for the artist, never linked publicly (theft concern — no "print this" feature by the artist's decision).

**How to apply:**

- Never put an R2 credential in a client component or `NEXT_PUBLIC_` var — mint a presigned PUT from `lib/storage/server.ts` instead.
- New image render sites use `imageVariantUrl(url, "thumb"|"medium")`.
- `scripts/backfill-image-variants.ts` has a hand-kept column list — a new image-URL column must be added there (JSON-nested ones as `substring: true`).
- The five `R2_*` vars must also be set in the project's Vercel environment.
