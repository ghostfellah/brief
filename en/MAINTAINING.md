# Maintaining the English brief

You publish the English edition of the daily brief. The French site stays at the repository root and on GitHub Pages. Change only files inside `en/`.

## Layout

| Path | Role |
|---|---|
| `index.html` | Latest edition. A copy of the newest `YYYY-MM-DD.html`. |
| `YYYY-MM-DD.html` | That day’s permalink. |
| `assets/theme.css` | GhostVoice theme. Every color and font is a variable at the top. |
| `assets/motif-mono.svg` | Pixel ghost, copied from GhostVoice `gabarits/motifs/motif-mono.svg`. |
| `vercel.json` | Static hosting. No build, no server. |

A new day does not get its own stylesheet or logo.

## Add a day

1. Read `../YYYY-MM-DD.html` (the French page at the repo root). Do not edit it, and do not edit `index.html` at the root.
2. Copy the previous English page to `en/YYYY-MM-DD.html`.
3. Translate that page. Follow the rules below. Keep the same regions: masthead, section nav, editions nav, three-headline opener, market board, Crypto, AI, Macro, Geopolitics, fact-check box, sources on every story, footer, `@ghostfellah` signature.
4. In the editions nav, add the new date as `<span class="is-current">D Mon YYYY</span>` and turn the previous current date into a link: `<a href="YYYY-MM-DD.html">D Mon YYYY</a>`. Only one `is-current`.
5. Point `<title>`, the meta description, `canonical`, both `hreflang` links, `<time datetime>`, the “French edition” links, and the footer permalink at the new date. The French URL is `https://ghostfellah.github.io/brief/YYYY-MM-DD.html`.
6. Copy the finished file over the latest page: `cp en/YYYY-MM-DD.html en/index.html`.
7. Check the result:
   - same story counts as the French page;
   - every external `href` identical to the French page, including `/fr/` paths;
   - every figure from the French page present, including figures the fact-check disagrees about;
   - the page still loads `assets/theme.css` and `assets/motif-mono.svg`;
   - links stay relative (`assets/theme.css`, `2026-11-20.html`), never root-absolute (`/assets/...`). This folder is the Vercel root and also `/en/` on GitHub Pages.

Leave older dated files as they were. The homepage list is whatever the new file contains, because `index.html` is that file.

## Translation rules

Translate faithfully. Do not add a story, drop a story, round a figure, or replace a figure with a later quote.

Keep each external URL character for character.

French number formatting becomes English formatting only:

- thousands space to a comma (`61 300` → `61,300`);
- decimal comma to a point (`97,17` → `97.17`);
- `M$` → `$… million`, `Md$` → `$… billion` (`1 800 Md$` → `$1,800 billion`);
- `pb` → `bp`;
- the minus sign stays `−` (U+2212).

If the table and the story, or the story and the fact-check, give two different figures, keep both. Do not reconcile them. The 20 November page does this on purpose for ETH (`$1,733` on the board, about `$1,735` in the story) and for Anthropic’s valuation.

Keep the issue number, the Paris clock times, and the place.

Unconfirmed items use exactly:

```html
<p class="badges"><span class="badge unconfirmed">Unconfirmed</span></p>
```

A figure the company itself published uses `<span class="badge">Company figure</span>` (charcoal border, no magenta square).

Use `’` and `“ ”`. Story titles stay in sentence case. Do not add inline styles. The CSS already sets Space Grotesk titles, DM Sans body, and DM Mono dates.

## Theme

Taken from GhostVoice `design.md` (article charter, line-art, light). Do not restyle a single edition.

| Token | Hex | Use |
|---|---|---|
| `--gv-cream` | `#F5F0E6` | Page |
| `--gv-mist` | `#E4DDF2` | Fact-check ground |
| `--gv-foam` | `#D3ECE7` | Market board |
| `--gv-greige` | `#CFC8BB` | Rules |
| `--gv-lavender` | `#B6A6E3` | Selection |
| `--gv-lagoon-light` | `#63C1B5` | Reserved |
| `--gv-violet` | `#4A2C8F` | Titles, links |
| `--gv-amethyst` | `#6B4FBF` | Reserved |
| `--gv-lagoon` | `#3FA99D` | Kickers, up moves |
| `--gv-magenta` | `#E0177A` | Accent only: kicker square, unconfirmed badge, down moves |
| `--gv-charcoal` | `#25232E` | Text, and the ghost |
| `--gv-slate` | `#6E6A7C` | Sources, notes |

Fonts, from Google Fonts in the page head: Space Grotesk 600 and 700, DM Sans 400–600 (italic 400 only if you quote someone), DM Mono 400–500. Kickers are DM Sans 600, uppercase, tracking `0.16em`, lagoon, with the magenta square from `.kicker::before`. Titles are Space Grotesk 700, tracking `-0.02em`, not italic, not uppercase.

Magenta and lagoon text stays at least 18px (the charter rule). Chart labels stay charcoal; the bars carry the color (`.bar.down`, `.bar.up`).

The site is the light charter. Do not invent a dark palette. A dark ground would need GhostVoice `motif-creme.svg` (cream ghost). That file is not in this folder.

Charts are inline SVG. Scale each chart so its largest absolute percent move is 130px wide, axis at x = 248. Copy the percent from the market board. Skip moves that are not percents (the 10-year is in basis points, so it stays in the table only). Do not add an image the French page does not have.

## Skeleton

```html
<link rel="stylesheet" href="assets/theme.css">
<header class="mast">… h1, @ghostfellah, French edition link, meta …</header>
<nav class="sommaire">section anchors</nav>
<nav class="editions">
  <span class="editions-label">Editions</span>
  <span class="is-current">21 Nov 2026</span>
  <a href="2026-11-20.html">20 Nov 2026</a>
</nav>
<main>
  <section class="opener">lead h2, two h3s, aside.board tables</section>
  <section class="rub" id="crypto">h2.kicker, article#crypto-1.lead-story, figure.chart, .rest</section>
  <section class="rub" id="ai">…</section>
  <section class="rub" id="macro">…</section>
  <section class="rub" id="geopolitics">…</section>
  <section class="factcheck" id="fact-check">
    <article class="ok" id="fact-check-1"><h3>Verified</h3></article>
    <article class="hold" id="fact-check-2"><h3>To be confirmed</h3></article>
  </section>
</main>
<footer class="colophon">generation line, badge line, French link, permalink, signature</footer>
```

Story ids stay stable for that file (`crypto-1`, `ai-2`, `macro-1`, `geopolitics-1`) because the opener links to them.

## Vercel

Point the project’s Root Directory at `en`. Framework preset: Other. Build command: empty. Install command: empty. There is no output directory to set: Vercel serves this folder as a static site.

`vercel.json` only turns on clean URLs and two response headers. The HTML keeps `.html` in links so `python -m http.server` inside `en/` works the same way.

GitHub Pages keeps serving the French site from the repository root (`https://ghostfellah.github.io/brief/`). Publishing `en/` does not replace it. Do not add a redirect at the repository root.
