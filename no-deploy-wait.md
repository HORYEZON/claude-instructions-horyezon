# Don't wait for deploys

After pushing, **don't wait for the Vercel deploy and don't re-test on the live site**. That means:
- no `vercel inspect --wait` background tasks;
- no live `curl` checks;
- no browser runs against the `*.vercel.app` sites.

**Why:** the owner, 10/24/26: "no need na hantayin mo yung deployment para i-test kasi nauubos lang time ko and token".

**How to apply:**
- Do the real verification locally: tsc, lint, build, a local `next start`, throwaway harness pages, mocked APIs.
- Then push, file the draft release note, and report.
- Say it's pushed and Vercel will deploy it. Never claim it was checked live.
- **Exceptions:**
  - The owner asks for a live check.
  - The problem only exists live, such as an env var or a Vercel-only build failure.
  - A destructive migration that the old deploy still depends on. Wait for the new deploy to be Ready, then apply it. That's sequencing, not re-testing.
