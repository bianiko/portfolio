# Anikó Biró – portfolio

Live site: https://bianiko.github.io/portfolio/ (GitHub Pages, deployed from `main`, repo root).
Every push to `main` goes live in ~1–2 minutes. There is no build step and no staging.

The owner is a UX/UI designer and writes in Hungarian; answer her in Hungarian.
Site copy is English.

## Structure

- `index.html` – the entire site: CSS in one `<style>`, all markup rendered by one `<script>`.
  - Plain HTML + vanilla JS. No framework, no npm, no bundler.
  - The only external dependency is Google Fonts: Source Serif 4 for headings, Inter for body text.
- `img/` – all images, WebP only.

## How `index.html` is organized

- **Design tokens.** They live in `:root` at the top of the CSS: colors, spacing, and type sizes.
  - Desktop overrides apply at `@media (min-width:720px)`.
  - Light theme only.
  - The layout uses a 1024px content column (`.wrap`) with the `--gutter` side padding.
- **Breakpoint.** Mobile is below 720px. Always check both sizes.
- **Routing.** Navigation is hash based: `#home`, `#resume`, `#human-ai`, `#chat`, `#setup`, `#skill`.
  - `route()` renders `PAGES[hash].html()` into `<main id="main">`.
  - It also runs that page's optional `after()` hook (for example `initProto` or `initStrip`).
  - It renders the footer through `renderFooter()`.
- **`PAGES`.** One entry per page: `{title, footer:{label,href}|null, html:()=>template, after?}`.
  - Page content is written with the helper functions defined above it: `sec`, `textSection`, `textSub`, `textFull`, `twoParas`, `steps`, `card`, `grid`, `quote`, `flow`, `deviceImg`, `HERO`, `rrow`, `tag`.
  - Inside text strings, `¶` means a paragraph break (handled by `prose()`).
  - Reuse these helpers instead of writing raw markup, so spacing and type stay consistent.
- **`IMG`.** Maps each image name to its intrinsic `[width, height]`.
  - `pic(name, alt, {sizes, zoom, scale, cls})` builds a responsive `<img>` from it. The image gets `srcset` when the `-900` variant exists and is wrapped in a zoom button by default.
- **Lightbox (`#lb`).** Clicking a `.zoom` image opens it full screen.
  - It relies on the global `[hidden]{display:none!important}` rule at the top of the CSS. Do not remove that rule, or the overlay covers the page on load.
- **Setup guide prototype (`#setup`).** It is a carousel with prev/next buttons and dots, and no autoplay.
  - Its images are `img/screen-1..8.webp`, named in `SCREENS`.
  - The `?v=N` query on these images is cache busting. Bump it when the images change.
- **Progress screens (`#skill`).** They show as 3 columns on desktop.
  - On mobile they become a single centered slide, with the arrows and dots above the image (`initStrip`).
- **Device frame (`.device`).** Uses a 10px border and 32px radius on desktop, and a 4px border and 14px radius on mobile.
  - Images inside keep their natural aspect ratio and are never cropped.
- **Footer contact.** Each line holds selectable text plus an icon-only copy button (`.copy`).
  - The button shows a check icon for about 1.6 s after copying.

## Images

- Use WebP, quality around 92.
- For every image wider than 1100px, also add `name-900.webp`, scaled to 900px wide.
- When adding or replacing an image, update its `[w,h]` in `IMG`.
- Export at a resolution high enough that nothing looks pixelated on retina screens.
  - Most source art is 2x the displayed size.
- The design source is Figma file `nwwNXrHVivPOwCl7HYT7HU`.
  - Page frames are in section `160:6980`.
  - Components are on page "Komponensek".
  - Setup guide screen variants are in set `124:36695`, built from the "Template" section.
- Keep the content and structure identical to the Figma frames unless asked otherwise.

## Copy conventions

- Headings use sentence case. Proper nouns and job titles in the resume keep their capitals.
- Product names "Mailchimp", "Intuit" and "QuickBooks" must not appear; write "XXX" instead.

## Workflow

1. Make the change in `index.html` (and `img/` if needed).
2. Preview locally with `python3 -m http.server` in the repo root, then open `http://localhost:8000/#home`.
   - Check every page you touched at about 390px and about 1280px wide.
   - Check the browser console for errors.
3. Commit with a short message describing the visible change, then push to `main`. That is the deploy.
4. Tell the owner what changed, and that a hard refresh (Ctrl/Cmd+Shift+R) may be needed.

Never paste a partial `index.html` or replace the script with placeholder comments such as "rest unchanged". The page renders only from the script, so a truncated file leaves only the header on screen.
