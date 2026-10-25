# Small change, light process

Size the verification to the risk. For a **trivial change**, skip the headless browser / Playwright test entirely. Trivial means:
- moving or reordering an element or tab;
- editing text or a label;
- swapping a colour.

For those, run only the required pre-push checks (tsc, lint, build). Add the docs line, ship, and keep the report short.

Use browser tests where the diff can't show whether it works: audio, timing, layout that may break, data flows, interactions.

**Why:** the owner, 10/26/26, after a one-line tab reorder was verified with Playwright: "bakit ang dami mong process nag Playwright ka pa eh papalitan mo lang din position or text lang". It wastes their time and tokens.
