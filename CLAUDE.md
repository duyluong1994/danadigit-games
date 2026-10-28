# danadigit-games — games.danadigit.com

Static site (plain HTML/CSS, no build step) that hosts the support and privacy pages for every game. Repo `duyluong1994/danadigit-games` (public), deployed by GitHub Pages from `main` / root.

- Domain: `CNAME` file = `games.danadigit.com`. DNS at GoDaddy: CNAME `games` → `duyluong1994.github.io`. HTTPS is enforced; the certificate auto-renews.
- If HTTPS ever gets stuck at "DNS Check in Progress", remove and re-add the custom domain:
  `gh api -X PUT repos/duyluong1994/danadigit-games/pages -F cname=` and then `-f cname=games.danadigit.com` (with `source[branch]=main`, `source[path]=/`).
- Each game gets `/<game-slug>/index.html` (support) and `/<game-slug>/privacy.html`, and a card on the root `index.html`. The App Store Support and Privacy URLs point here, so never move or delete existing pages.
- Shared studio rules are in `../CLAUDE.md`.

## Design system

- `style.css` (root) is the shared layout and holds design tokens as CSS variables (`--bg`, `--bg-2`, `--panel`, `--panel-edge`, `--text`, `--dim`, `--accent`, `--accent-ink`, `--heading`, `--glow`, `--serif`, `--bullet`). Every page links `../style.css` first, then the game's own `style.css`.
- A game's `style.css` is a **theme**: it overrides `:root` tokens to match the game's `src/ui/kit.ts` colors, sets `.game-hero { background-image }`, and may add one small signature touch (Lantern Drift: CSS star field).
- Root `index.html` has one `<article class="card card--<slug>">` per game; its palette lives in the `<style>` block there (`--card-bg`, `--card-bg-solid`, `--card-edge`, `--card-heading`, `--card-dim`, `--card-accent`). Status badge: genre (`badge live`) once released, `In development` before. A game still in development gets a card only (inline SVG art, no raster art invented), no pages until submission.
- Page building blocks: `.topbar`, `.game-hero` (icon + h1 + tagline + `.actions`), `.shots` (screenshot strip), `.panel`, `.grid-2`, `.rules`, `.faq` (`<details>`), `.doc` (privacy), `footer`. Copy `lantern-drift/` as the template for a new game.
- Privacy pages: restyle freely, but don't change policy wording or the "Last updated" date unless the policy itself changes.
- Images live in `<slug>/img/`, copied (never linked) from the game repo:
  ```sh
  sips -Z 256 AppIcon-512@2x.png --out img/icon-256.png   # also 180 → apple-touch-icon.png, 64 → favicon.png
  cwebp -q 78 -resize 1600 0 public/assets/title.jpg -o img/hero.webp
  cwebp -q 78 -resize 600 0 store/screenshots/iphone-6.9/0X.png -o img/shot-0X.webp
  # og.jpg: 1200×630 crop of the title art (absolute URL in og:image)
  ```
- App Store button: label "Coming soon to the App Store" until `https://itunes.apple.com/lookup?id=<appId>` returns a result, then "Download on the App Store" and add `<meta name="apple-itunes-app" content="app-id=<appId>">` to that game's pages.
- Preview locally: `python3 -m http.server` from this folder, then check 390px and 1280px widths.
