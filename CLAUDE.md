# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Source for spechtlive.com — a static marketing/booking site for one operator, Kieran Specht, running two business tracks under one umbrella brand ("Specht Live"):

- **Specht Staging Solutions** — sound, lighting, and staging production/rental (courts venues, planners, bands)
- **DJ R3$pecht** — DJ performance (courts weddings, private parties, festivals)

Plain HTML/CSS/JS. No framework, no build step, no package.json, no bundler. Every `.html` file is deployed as-is.

## Commands

There is no build, lint, or test tooling in this repo — there's nothing to compile or check beyond opening the HTML.

**Local preview:** any static file server works, e.g. `python3 -m http.server 8000` from the repo root, then visit `http://localhost:8000`.

If previewing via the Claude Code `Preview` MCP tool specifically: this repo lives under iCloud Drive (`~/Library/Mobile Documents/...`), and the Preview tool's dev-server subprocess cannot get OS-level access to that path — it fails even on `os.getcwd()`, regardless of what directory you point it at. Workaround: copy the site files to a location outside iCloud (e.g. the session scratchpad dir) and serve from there with a small script using `http.server.SimpleHTTPRequestHandler` with an explicit `directory=` argument — do **not** use `python3 -m http.server --directory ...` as a CLI invocation, since `http.server`'s argument parser calls `os.getcwd()` unconditionally while building its parser (before even reading `--directory`), which triggers the same failure.

**Deploy:** push to GitHub, then import the repo in Netlify (auto-detects as a static site, no build command, publish directory `/`). See `README.md` for the exact git commands and DNS/domain steps. Netlify Forms is wired up for `book.html` via `data-netlify="true"` — no backend code involved.

## Architecture

**No templating — every page repeats the full header/nav/footer markup verbatim.** There are 8 HTML pages (`index`, `about`, `specht-staging-solutions`, `dj-respecht`, `gallery`, `faq`, `book`, `thanks`), each a complete standalone document with its own copy of the `<header class="site-header">` nav block and `<footer class="site-footer">` block. **When adding/removing/renaming a nav item or footer link, it must be edited identically across all 8 files** — there is no shared include mechanism. `thanks.html` is the one exception: it has the nav but no footer-grid (just a copyright line).

**Styling is one shared stylesheet** (`css/style.css`) using CSS custom properties for the color system (`--navy`, `--teal`, `--rust`, defined in `:root`) and a small set of reusable component classes applied directly in the HTML: `.btn`/`.btn-primary`/`.btn-outline`, `.pricing-table`, `.callout` (amber warning box, used for placeholder-pricing disclaimers), `.track-card`/`.track-grid`, `.sidebar-box`, `.two-col`, `.gallery-grid`, `.photo-rounded`. No CSS-in-JS, no preprocessor.

**`js/main.js`** is the entire JS surface: a single `DOMContentLoaded` listener that toggles the `.open` class on `.nav-links` when `.nav-toggle` (the mobile hamburger button) is clicked. That's it.

**Images live in `img/`.** Photos are sourced from the operator's phone/social media and are typically multi-MB originals — always downsize before adding one: `sips -Z 1600 --setProperty formatOptions 75 path/to/photo.jpg` gets a phone photo down to a reasonable web size in place.

**Logo assets:** `img/logo-mark.png` (small piano-key ribbon "swish" mark) is used as both the header logo icon (`.logo-mark` class, next to the "Specht Live" wordmark) and the favicon. `img/treble-clef-mark.png` (a treble clef built entirely from piano keys) is the primary brand mark, shown large on the homepage hero above the `<h1>` — it's meant to be placed wherever it can be seen in its entirety (large), never shrunk down to icon size, since the piano-key detail becomes illegible/muddy at small sizes. Both were supplied directly by the operator (not generated). **Their licensing status is unconfirmed** — a sibling reference file had a visible stock-photo watermark — operator has acknowledged this and said he'll address it later; don't assume it's resolved.

**Every indexable page** (all except `thanks.html`) carries a matched set of `<meta name="description">` + Open Graph (`og:title`, `og:description`, `og:image`, `og:url`) + `twitter:card` tags in `<head>`. The `og:image`/`og:url` values are hardcoded to `https://spechtlive.com/...` absolute URLs (required for social crawlers to resolve them), so they'll only actually resolve once that domain is live.

**Content has explicit placeholder vs. confirmed status** — this matters when editing copy or numbers:
- Specht Staging Solutions pricing (`specht-staging-solutions.html`) is pulled from a real rate sheet and is treated as close to final.
- DJ R3$pecht pricing (`dj-respecht.html`) is explicitly placeholder — approximate `~$` ranges wrapped in a `.callout` box saying so. Don't "clean up" the `~` or the callout without being told the real numbers are locked in.
- Brand colors in `css/style.css` `:root` are a placeholder palette, not final brand colors.
- See `README.md`'s "Status / before this goes live" section for the current up-to-date checklist of what's confirmed vs. still open.
