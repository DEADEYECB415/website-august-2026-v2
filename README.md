# Dead Eye Coffee — website

Two static pages. No build step, no dependencies. Open `index.html` in a
browser to preview locally, or upload the whole folder to any host.

## Files

```
index.html                    home page
menu.html                     full menu
site.webmanifest              app name + icons for mobile home screens
assets/
  favicon.ico                 browser tab (16/32/48/64 in one file)
  favicon.svg                 browser tab, scalable — modern browsers prefer this
  apple-touch-icon.png        iOS home screen, 180px, opaque
  icon-192.png / icon-512.png Android + manifest
  og-image.jpg                link preview image, 1200x630
  deadeye-badge.png           badge logo, transparent, bone (#EFE8DD)
  deadeye-lockup.png          full lockup, transparent, bone — 2501px
  deadeye-lockup@1x.png       same, half size
```

Keep `index.html`, `menu.html`, `site.webmanifest`, and the `assets` folder
together — the pages reference them by relative path.

## Deploying

Drag the folder onto Netlify Drop (netlify.com/drop), or push to GitHub and
enable Pages. Both are free and handle static files. The homepage must be
named `index.html` for either to serve it as the root.

## Favicon

Already wired up. Five tags sit at the top of `<head>` on both pages:

```html
<link rel="icon" href="assets/favicon.ico" sizes="any">
<link rel="icon" href="assets/favicon.svg" type="image/svg+xml">
<link rel="apple-touch-icon" href="assets/apple-touch-icon.png">
<link rel="manifest" href="site.webmanifest">
<meta name="theme-color" content="#0D0A10">
```

The icons put the bone badge on a dark ink tile rather than leaving it
transparent. A near-white logo on a transparent background disappears
against the light tab bar most people use — the tile keeps it legible on
light and dark alike, and iOS composites transparent icons onto black
anyway.

Browsers cache favicons aggressively. If an old icon persists after
deploying, hard-refresh or check in a private window.

To regenerate at a different size or padding, the source is
`assets/deadeye-badge.png`.

## Editing

Everything for each page — HTML, CSS, JS — is in that page's single file.
Colors and spacing live in the `:root` block at the top of the `<style>` tag.

| Variable | What it controls |
|---|---|
| `--ink` | page background |
| `--bone` | primary text |
| `--bone-dim` | secondary text |
| `--ube` | accent (nav underline, notice bar edge, step numbers) |
| `--f-*` | menu swatch dots, one per flavor |
| `--gutter` | left/right page margin |
| `--rule` | vertical space between sections |

### Outbound links

| What | Where it appears |
|---|---|
| order.deadeyecoffeebar.com | nav, hero, highlights, footer, menu CTA |
| app.squareup.com/gift/… | hero button, footer of both pages |
| Instagram | footer of both pages |
| Google Maps | hours strip, directions block, footer |

### Hero photo

The hero background is CSS gradients standing in for a photo. In
`index.html`, find `.hero__media` and the comment marked `SWAP ME`. Replace
the gradients with:

```css
background:url('assets/hero.jpg') center/cover no-repeat;
```

The vignette and grain layers on top will still apply.

### Menu items

Each item is one block. Copy an existing one and change the name and swatch:

```html
<div class="pour" style="--c:var(--f-pandan)">
  <span class="pour__dot" aria-hidden="true"></span>
  <p class="pour__name">Pandan Latte</p>
</div>
```

Adding a category also needs a matching link in the sticky rail near the top
of the page, pointing at the section's `id`.

### Logo

The badge is inline SVG in two places — the nav mark on both pages, and the
hero watermark on the home page. Each copy needs its own mask `id`
(`navMask`, `badgeMask`); duplicate ids on one page break the second one.
The nav copy fills with `currentColor`, so it follows the nav text color.

### Link previews (Open Graph)

Both pages carry Open Graph and Twitter Card tags, so links shared to
iMessage, Instagram, Facebook, WhatsApp, Slack, or X show a large photo card
with a title and description. The image is `assets/og-image.jpg`.

IMPORTANT: these tags use absolute URLs, hard-coded to
`https://www.deadeyecoffeebar.com`. Open Graph does not accept relative
paths. If the site lives at a different domain — including a temporary
Netlify or GitHub Pages address — find and replace that string in both
`index.html` and `menu.html`, or the preview image will not load.

Test with Facebook's Sharing Debugger or opengraph.xyz after deploying.
Platforms cache previews hard; the Debugger has a "Scrape Again" button that
forces a refresh.

Replacing the image: crop to exactly 1200x630, save as JPG under ~200KB,
and keep the filename.

### Ordering notice bar

The bar appears Friday–Sunday only, in Pacific time, and hides the rest of
the week. To change which days, edit `ordersClosed` in the script at the
bottom of each page. Dismissing it lasts for that page view only; making it
stick across pages needs `localStorage`, which was left out because it does
not run in preview environments.

## Known gaps

- Hero photo or video is a placeholder.
- Interior pages for About and Catering do not exist yet; nothing links to them.
- Menu prices are deliberately absent — Square is the source of truth.
- Cross-street reference in the directions card ("between Linden and Maple")
  was unverified. Check it.
- Open Graph URLs are hard-coded to www.deadeyecoffeebar.com. Update them if
  the domain differs.
- No analytics.
