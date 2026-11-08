# One font selection in every project

Every font picker in every Horyezon project offers the same fonts. That covers the museum's 3D text (banners, plaques, labels, standees, the Lyrics Wall and the Scene Editor) and every CSS picker (theme, Site Design, splash taglines, minimap labels, watermark captions, lyric looks and landing pages). Each project keeps the list in `lib/fonts/catalog.ts`. A change starts in scriptovernovel.music's catalog, and kalamari.arts and kstudio follow it.

| Group | Fonts |
| --- | --- |
| Clean | DM Sans (Regular, Bold, Italic), Space Grotesk (Medium, Bold), Plus Jakarta Sans |
| Heavy and condensed | Archivo Black, Anton, Bebas Neue, Oswald |
| Serif | Fraunces, Cormorant Garamond, Playfair Display, Bodoni Moda, Abril Fatface, Cinzel |
| Handwriting | Caveat, Great Vibes, Permanent Marker |
| Comic and arcade | Anime Ace, Bada Boom BB, Bangers, Monoton, Press Start 2P |
| Typewriter | Courier Prime, Special Elite |

That makes 26. A paid product offers 24: Anime Ace and Bada Boom BB are Blambot fonts, free only for non-commercial use, and using them as a webfont needs a licence. kstudio leaves them out, and its `lib/fonts/catalog.test.ts` checks that.

**Why:** the owner, 11/07/26: every project with a font picker gets the new selection ("kalamari.arts, kstudio, lahat ng may Font Text Selection"). On 11/08/26 they added: new projects too. With one list, a look made in one project can be made in the others.

**How to apply:**
- **A new project** with any font picker copies scriptovernovel.music's `lib/fonts/catalog.ts` and the font files it names from its `public/fonts/`. Leave out the two Blambot fonts if the product is paid. Build every picker from the catalog: 3D text reads `file`, a CSS picker reads `css`.
- **3D text:** `file` is what troika-three-text (drei's `<Text>`) loads. Use `.ttf` or `.woff`, never `.woff2`.
- **CSS:** a face the project already loads through next/font keeps its `var(--font-…)` value. Every other face is an `@font-face` rule over its file in `public/fonts`, with `font-display: swap`. That costs nothing until a page uses it.
- **Don't preload what a page doesn't show before it's scrolled.** Preload only the body face and the heading face. If the rest must be ready for a menu, fetch them after the page has loaded (scriptovernovel.music's `components/FontWarmup.tsx`). In scriptovernovel.music v1.1, 13 preloads were holding back every page's first paint.
- **Keep saved values working.** A catalog keeps the labels and paths that settings already store; never rename them.
- **Changing the list:** in one session, add the font to every project's catalog, `public/fonts`, `@font-face` rules and `NOTICE`. If a project is left out, say which one and why.
- **Licences:** a font under the SIL OFL or Apache 2.0 gets a line in the project's `NOTICE`. For any other licence, ask the owner first.
