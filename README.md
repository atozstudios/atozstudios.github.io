# atozstudios.github.io

Website for **A2Z Studios**, an independent Android game studio.
Live at <https://atozstudios.github.io>.

Plain static HTML and CSS — no build step, no dependencies, no framework.
Edit a file, commit, push; GitHub Pages serves it.

## Layout

```
index.html            Landing page: hero, one block per app, about, next-up cards
numova.html           Numova detail page
pinball-rush.html     Pinball Rush detail page
privacy.html          Hub linking out to each app's privacy policy
404.html              Not-found page
robots.txt            Points crawlers at the sitemap
sitemap.xml           Update when you add a page
.nojekyll             Stops GitHub Pages running Jekyll over the files
assets/
  css/styles.css      All styling; design tokens at the top
  favicon.svg         Studio mark
  img/                Icons, screenshots, social share image
```

## Adding a new app

Four steps. Nothing is generated, so everything is a copy-paste.

**1. Add the assets.** Drop the icon and screenshots into `assets/img/`, named
`<slug>-icon-256.webp`, `<slug>-icon-180.png`, and `<slug>-1.webp`, `<slug>-2.webp`, …

Screenshots are stored at 720 px wide, WebP quality 82 (they render around 300 px, so
that covers retina). To match:

```bash
python3 - <<'PY'
from PIL import Image
im = Image.open("raw-screenshot.png").convert("RGB")
w, h = im.size
if w > 720: im = im.resize((720, round(h * 720 / w)), Image.LANCZOS)
im.save("assets/img/myapp-1.webp", "WEBP", quality=82, method=6)
PY
```

**2. Pick the app's accent colour.** Add three custom properties in `assets/css/styles.css`
next to the existing ones, then one line in the `[data-accent=…]` block:

```css
--myapp:       #5f3392;   /* the accent itself, must clear 4.5:1 on --bg */
--myapp-ink:   #45217a;   /* darker, for link text */
--myapp-wash:  #ece4f7;   /* very pale, for notice backgrounds */

[data-accent="myapp"] { --accent: var(--myapp); --accent-ink: var(--myapp-ink); --accent-wash: var(--myapp-wash); }
```

The existing accents were sampled from each app's own store icon and then darkened until
they passed 4.5:1 against the cream background. Worth doing the same — it keeps each app
page feeling like the app.

**3. Add a block to `index.html`.** Copy the whole `<article class="app" data-accent="…">`
(there is a comment marking it) and edit the icon, title, category, hook, features and links.
Also add a `<a class="card">` to the "What's next" section and a `<li>` to both footer lists.

**4. Copy a detail page.** `cp numova.html myapp.html`, then update the `<title>`,
meta description, canonical URL, Open Graph tags, JSON-LD, `data-accent`, and the content.
Add the new URL to `sitemap.xml`.

## Content rules

Everything on this site is checked against the Google Play listings and the privacy
policies. Two things to keep that way:

- **No invented numbers.** There are no download counts, star ratings, review quotes or
  award badges anywhere, because none were verified. Do not add them unless they are real.
- **No cash-reward claims.** The Pinball Rush store listing mentions "guaranteed cash
  rewards"; that is deliberately left off this site, and the two store screenshots carrying
  that messaging were not included in this repo.
- **Scope claims to the app you can prove them for.** "Plays offline, no account" is stated
  about Numova specifically, because that is what Numova's privacy policy says. Pinball Rush
  uses Google Play Services, so the same claim is not made for it or for the studio overall.

Privacy policies are linked, never copied, so this site cannot drift from the versions
Google Play points at.

## Local preview

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deploying

Push to `main`. GitHub Pages publishes from the branch root.

```bash
git add -A && git commit -m "Update site" && git push
```
