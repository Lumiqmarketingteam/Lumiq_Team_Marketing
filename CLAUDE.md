# LiteCone.ai website

Static GitHub Pages site — no build step. Every page is a self-contained HTML file
(inline CSS + JS). Live at https://lumiq-websites.github.io/Litecone_Website/
(repo: `lumiq-websites/Litecone_Website`, branch `main`, legacy Pages pipeline).

Pages: `index.html` (home) · `leo.html` (agent profile) · `404.html`.
Assets in `assets/` (logos, agents/ headshots, og-image, favicon).

## Non-negotiable standards — apply to EVERY new or edited page

### SEO checklist
- Exactly one `<h1>`; sections use `<h2>` (don't skip levels).
- `<title>` (≤60 chars) and `<meta name="description">` (≤160 chars), unique per page.
- `<link rel="canonical">` with the full live URL.
- Open Graph: `og:title/description/type/url/site_name/image` (+ `og:image:width/height`,
  1200×630, use `assets/og-image.png`) and Twitter `summary_large_image` tags.
- JSON-LD: `WebPage` (+ `BreadcrumbList` on subpages); homepage carries `Organization` + `WebSite`.
- `<meta name="theme-color">` pair (dark `#0A0A0A`, light `#FAFAF7`).
- Favicon links: `assets/favicon.svg` + `assets/apple-touch-icon.png`.
- Add the page to `sitemap.xml` (bump `lastmod`); 404-type pages get `noindex` instead.
- All absolute URLs use the CURRENT live origin — if the repo moves again, grep-replace
  the old origin across `*.html`, `robots.txt`, `sitemap.xml`.

### ADA / WCAG 2.1 AA checklist
- Skip link (`<a class="skip" href="#main">Skip to content</a>`) + `<main id="main">`.
- Color tokens only — never raw grays/oranges. `--ink-3` and `--accent-ink` are
  contrast-tuned (small orange TEXT must use `--accent-ink`, never `--accent`, in light mode).
- Form controls & chips use `--ctl-line` borders (3:1 against surface).
- `:focus-visible` outline rule on links/buttons; keyboard-test every interactive element.
- Images: meaningful `alt`, or `alt=""` when the name/label sits in adjacent text.
- Decorative SVGs get `aria-hidden="true"`.
- Anything auto-moving (marquees) needs pause-on-hover + a `.mq-pause` button,
  `aria-hidden` on the moving track, an `.sr-only` static copy of the content,
  and a `prefers-reduced-motion` fallback. All animations respect reduced motion.
- Disclosure buttons (nav dropdown) manage `aria-expanded`, close on Escape/outside click.
- Modals: `role="dialog" aria-modal` + labelledby, focus trap, focus restore, Esc close;
  errors set `aria-invalid` + `aria-describedby`; send failures use `role="alert"`.
- Repeated links get distinguishing `aria-label`s (e.g. "Know more about LEO").
- Both themes must pass: check light AND dark after any color change.

## Shared chrome (copy from an existing page — keep identical everywhere)
- Header: themed logo pair (`.logo-dark`/`.logo-light`), nav links, "Agentic AI
  Co-workers" dropdown (add each new agent page to it, mark current with
  `aria-current="page"`), theme toggle, briefing CTA.
- Footer: foot-big wordmark + Platform/Engage columns + bottom bar.
- Theme system: boot script in `<head>` (localStorage `lc-theme`), `data-theme`
  overrides, `color-scheme`, toggle JS. New colors need values in BOTH themes.
- Fonts: Fraunces (display serif) / Inter (body/UI) / IBM Plex Mono (labels).

## Forms & leads
All forms POST JSON to `https://formsubmit.co/ajax/team.marketing@lumiq.ai`
(`LEAD_ENDPOINT`). Cross-page CTAs deep-link as `/#open-briefing|demo|contact`;
the homepage (and leo.html for briefing) auto-open the matching modal from that hash.
New agent pages: copy leo.html's briefing modal block wholesale.

## Deploy notes
- Push to `main` → legacy Pages build (~45s). If a deploy stalls, request a build:
  `POST /repos/lumiq-websites/Litecone_Website/pages/builds`.
- Verify a deploy by diffing the live URL (cache-busted query) against the local file.
- GitHub Pages serves with ~10-min browser cache — check in incognito.

## Verification habits before committing
- `node --check` on extracted inline `<script>` blocks.
- Grep-assert structural counts (forms, modals, nav items) after scripted edits.
- Confirm both themes, keyboard path, and mobile breakpoints (920px/560px) locally
  via `python3 -m http.server 5544`.
