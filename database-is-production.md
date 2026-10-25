# DATABASE_URL is the live production database

In every project here, `DATABASE_URL` / `DIRECT_URL` in `.env` point at the hosted Supabase Postgres (pooler host `aws-0-ap-southeast-1.pooler.supabase.com`) — even where a `.env` comment says "LOCAL". There is no staging buffer: schema commands hit **production** directly.

- `yarn db:push` / `prisma db push` alters the live schema.
- `yarn db:migrate` / `prisma migrate dev` (projects with `prisma/migrations/`) writes a migration file **and** applies it to the live DB.
- `yarn db:reset` (`prisma db push --force-reset`) **wipes** the live DB.

**Why:** a destructive Prisma command here is irreversible data loss for real visitors.

**How to apply:** confirm with the user before running `db:push` / `db:migrate`; never run `db:reset` / `--force-reset` without re-checking `DATABASE_URL` first. Adding a nullable column is safe/non-destructive; dropping or retyping columns is not. `yarn prisma generate` is always safe (local only, needed for `tsc`). For dev speed, `supabase start` + localhost URLs is the recommended setup.

**Vercel:** Prisma 7's `prisma.config.ts` uses `env("DIRECT_URL")`, which *throws* when the variable is missing (the `?? env("DATABASE_URL")` fallback never runs). `postinstall: prisma generate` therefore fails the Vercel build unless **both** `DATABASE_URL` and `DIRECT_URL` are set in the Vercel project's environment variables.
