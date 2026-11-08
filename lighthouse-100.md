# Lighthouse 100

Every public page aims for 100 in all four Lighthouse categories (Performance, Accessibility, Best Practices, SEO), measured on mobile. Each finding is either fixed or written down as a known exception, with its reason and what it would take to fix. Never fake a score: what Lighthouse measures must be what a visitor gets.

**Why:** the owner, 11/08/26: "gawin 100% Lighthouse para sa mga projects". Most visitors are on phones. The score stands in for how fast and usable a page is for them, and the reports name exactly what to fix.

**How to apply:** measure, read the reports, fix, then measure again. The details below come from scriptovernovel.music v0.97 and v1.1 (its Progress_Timeline has both, worked through).

## Measure the same way every time

- **Use a production build:** `yarn build && yarn start`, never `next dev`.
- **Use Lighthouse 12 on mobile (its default):** `npx lighthouse@12 http://localhost:3000/<page> --only-categories=performance,accessibility,best-practices,seo --output=json --output-path=<file>`. In a headless setup, point `CHROME_PATH` at a Chromium.
- **Block every write the page makes on load,** so tests aren't counted as visits: for example `--blocked-url-patterns='*/api/visitor-count*'`. A local build may still use the live database.
- **Run each page twice:** scores move by about 3 points between runs. Cover the home page, every top-level public page, and one detail page of each kind.
- **Read the report, not only the score.** Check the LCP element and its phases (`largest-contentful-paint-element`), and every failing audit with its elements.
- **Lighthouse simulates a slow 4G phone.** A page that feels instant locally can still score 60. If the simulated LCP looks wrong, trace a real browser too: a `PerformanceObserver` on `largest-contentful-paint`, with the CPU throttled 4×.
- **Test before shipping** any new public page, and any change to the shared layout (header, fonts, hero, splash, global CSS).

## Performance

- **The largest paint must happen early, without waiting for scripts:**
  - Don't start an entrance animation at `opacity: 0` on text or images in the first screen. Chrome never counts a paint at opacity 0 as the LCP, so the title only counts when something repaints it later. Start at `0.01`; the eye can't tell.
  - Render the LCP image on the server, as an `<img>` with `fetchPriority="high"`. Never put `loading="lazy"` on it, and use a variant no bigger than it's shown: a blurred backdrop takes the thumbnail.
  - Preconnect to the image CDN's origin.
- **Fonts:** preload only the faces the first screen shows, usually one body face and one heading face.
  - Give every other next/font face `preload: false`, or make it an `@font-face` that loads on demand, all with `font-display: swap`.
  - If a menu needs them ready, fetch them after `load`, when the browser is idle, Latin files only (scriptovernovel.music's `components/FontWarmup.tsx`).
  - See also [`shared-font-selection.md`](shared-font-selection.md).
- **JavaScript:**
  - Load heavy client code (three.js, audio engines, editors) with `dynamic(() => import(…), { ssr: false })`, or on interaction.
  - No admin code on public pages.
  - Third-party scripts only on the pages that use them: reCAPTCHA only where a form posts.
  - Embeds (Spotify, YouTube, Bandcamp) mount near the viewport, or on a click.
- **Images:** give every `<img>` its width and height or an aspect-ratio box. Below the first screen, add `loading="lazy"` and `decoding="async"`. Serve WebP or AVIF, at a variant sized to the display. Use `srcset` and `sizes` where one image serves both phone and desktop.
- **Layout shift (CLS 0):** reserve the space for embeds, banners and marquees before they load. next/font's fallback metrics keep text from jumping when a font swaps in.
- **Main thread:** no long tasks while the page loads. Defer anything that can wait (service worker registration, warm-ups, analytics) to after `load` or `requestIdleCallback`.
- **Server response:** cache what public pages read (`unstable_cache`, revalidation), avoid N+1 queries, and render statically wherever the data allows.

## Accessibility

- **Contrast:** at least 4.5:1 for text, and 3:1 for large text.
  - Translucent text counts: `text-cream/40` on near-black fails, so use `/60` or more.
  - Give `<html>` a real background colour, or the check measures translucent panels against white.
- **Headings** go in order (h1, h2, h3, no skipping), with one h1 per page. Keep the look with classes, not the tag.
- **Every control has a name:**
  - an icon-only button gets `aria-label`;
  - an input gets its label;
  - a link has text that says where it goes.
- **Touch targets** are at least 24 × 24 CSS px, carousel dots and chips included.
- **Hidden panels are `inert`,** not only transparent.
- **A decorative animated overlay** (such as a splash lockup) is `inert`, with a text alternative beside it, so a frame caught mid-fade isn't checked as text.
- **Basics:** `lang` on `<html>`, `alt` on every image (`""` for decoration), ARIA roles only where they're valid, no keyboard trap, and a visible focus ring.

## Best Practices

- **No console errors on load:** no hydration mismatch, no 404s, no failed fetches.
- **No third-party cookies on load.** An embed that sets cookies (Spotify) loads on a click, or it's a known exception the owner accepted.
- **Images** keep their aspect ratio and have enough resolution for the screen's pixel ratio.
- **HTTPS everywhere, and no deprecated APIs.**
- **Ignore `/_vercel/insights` locally:** its 404 only happens off Vercel, so check that audit on the deployed site.

## SEO

- **Every page has:**
  - its own `<title>` and meta description (`generateMetadata`);
  - a canonical URL;
  - robots rules that allow public pages;
  - an entry in the sitemap.
- **Links are real `<a href>`s,** with text that describes them.
- **Most text is at least 12 px.** Every image has `alt` text.
- **Structured data, if any, validates.**

## Never

- **No gaming the score.** No user-agent or `navigator.webdriver` checks that hide things from Lighthouse, and nothing skipped or made worse only for tests.
- **Don't trade something visitors see or do for points without asking the owner.** Examples: a click before a player loads, softer images, a shorter splash. Offer it with the numbers, and let the owner choose.

## After the work

Record each page's scores before and after (performance / accessibility / best practices / SEO) in `Docs/Progress_Timeline.md`. Add a "Lighthouse notes" paragraph to the README naming what was changed and every exception still open, with its reason.
