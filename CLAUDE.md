# danadigit-games — games.danadigit.com

Static site (plain HTML/CSS, no build step) that hosts the support and privacy pages for every game. Repo `duyluong1994/danadigit-games` (public), deployed by GitHub Pages from `main` / root.

- Domain: `CNAME` file = `games.danadigit.com`. DNS at GoDaddy: CNAME `games` → `duyluong1994.github.io`. HTTPS is enforced; the certificate auto-renews.
- If HTTPS ever gets stuck at "DNS Check in Progress", remove and re-add the custom domain:
  `gh api -X PUT repos/duyluong1994/danadigit-games/pages -F cname=` and then `-f cname=games.danadigit.com` (with `source[branch]=main`, `source[path]=/`).
- Each game gets `/<game-slug>/index.html` (support) and `/<game-slug>/privacy.html`, and a card on the root `index.html`. The App Store Support and Privacy URLs point here, so never move or delete existing pages.
- Shared studio rules are in `../CLAUDE.md`.
