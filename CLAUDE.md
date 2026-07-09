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

**Preview screenshot/scroll gotcha:** `html { scroll-behavior: smooth; }` (in `css/style.css`) combined with `window.scrollTo()` via `preview_eval` can leave the automated browser in a stuck state where `preview_screenshot` returns a blank frame and `window.scrollY` stops updating, even though the page is genuinely fine. Fix: run `document.documentElement.style.scrollBehavior = 'auto'` and set `document.documentElement.scrollTop` directly (not `window.scrollTo`) before screenshotting. Simplest workaround: resize the viewport tall enough (e.g. height 3000+) to capture the whole page in one shot without scrolling at all.

**Deploy:** **already live** — pushed to `github.com/KJAMZ707/spechtlive-website` (`origin`/`main`), deployed on Netlify, live at `spechtlive.com` with valid SSL. See `README.md` for the full deploy steps (kept for redeploy/reference purposes) and current live-status checklist.

**Netlify Forms gotcha (cost real debugging time — read this before touching `book.html` or its form handling):** Netlify's dashboard lets you configure a form submission notification rule (Site → Forms → "Form submission notifications") for a form it's detected by name in the HTML, **without that meaning form *detection* is actually enabled**. Detection is a separate toggle at Site → Forms → "Enable form detection," off by default, and enabling it only takes effect starting from the *next* deploy — not retroactively. Symptom when it's off: submitting the form returns a raw Netlify 404 on the POST (both to the form's own page and to its `action` target), even though the form's HTML is 100% correct (`data-netlify="true"`, hidden `form-name` input, unique field names, relative `action` path — all the things Netlify's own troubleshooting docs tell you to check first). If a booking-form submission on this site ever 404s again, check this toggle before anything else.

**DNS / email routing:** the domain's nameservers were switched to **Netlify DNS** (`*.p05.nsone.net`) to get SSL working smoothly. This move does not carry over any DNS records that existed at the old registrar (GoDaddy) — in particular it silently dropped the **Google Workspace MX record**, breaking mail delivery to every `@spechtlive.com` address until it was manually re-added in Netlify's DNS records editor (`MX`, host `@`, priority `1`, value `SMTP.GOOGLE.COM` — Workspace uses this single simplified record now, not the old 5-record `ASPMX.L.GOOGLE.COM` setup). If the domain's DNS is ever migrated again (new registrar, back to GoDaddy, etc.), re-check this MX record isn't lost in the process.

## Architecture

**No templating — every page repeats the full header/nav/footer markup verbatim.** There are 8 HTML pages (`index`, `about`, `specht-staging-solutions`, `dj-respecht`, `gallery`, `faq`, `book`, `thanks`), each a complete standalone document with its own copy of the `<header class="site-header">` nav block and `<footer class="site-footer">` block. **When adding/removing/renaming a nav item or footer link, it must be edited identically across all 8 files** — there is no shared include mechanism. `thanks.html` is the one exception: it has the nav but no footer-grid (just a copyright line).

**Styling is one shared stylesheet** (`css/style.css`) using CSS custom properties for the color system (`--navy`, `--teal`, `--rust`, defined in `:root`) and a small set of reusable component classes applied directly in the HTML: `.btn`/`.btn-primary`/`.btn-outline`, `.pricing-table`, `.callout` (dark gold-tinted box, used for the overtime/travel notice and the gallery "more coming soon" note — not exclusively a placeholder-content warning anymore), `.track-card`/`.track-grid`, `.sidebar-box`, `.two-col`, `.gallery-grid`, `.photo-rounded`. No CSS-in-JS, no preprocessor.

**Theme is a dark "deep purple"** — `--bg: #170f22`, `--bg-alt: #211630`, `--ink: #f2f0f5` (near-white text), `--muted: #ab9bc0`, `--navy`/`--teal` both repointed to the purple accent `#C9A6F5` (kept as two variable names for low-risk-refactor reasons, but they're the same color), `--rust: #E8B84B` (gold, used only to differentiate the DJ track card border from the Staging Solutions purple one). Footer uses a separate near-black `#0d0815`, not a variable, since it needs to read as "grounded" below the page `--bg`. When editing colors, keep in mind `--navy`/`--teal` are light purple now (not dark) — anything using them as a background needs dark text (`#170f22`), and anything using them as text needs a dark backdrop.

**`js/main.js`** is the entire JS surface: a single `DOMContentLoaded` listener that toggles the `.open` class on `.nav-links` when `.nav-toggle` (the mobile hamburger button) is clicked. That's it.

**Images live in `img/`.** Photos are sourced from the operator's phone/social media and are typically multi-MB originals — always downsize before adding one: `sips -Z 1600 --setProperty formatOptions 75 path/to/photo.jpg` gets a phone photo down to a reasonable web size in place.

**Logo assets:** `img/logo-mark.png` (small piano-key ribbon "swish" mark, white/light transparent PNG) is used as the header logo icon (`.logo-mark` class, next to the "Specht Live" wordmark) — it's white because it sits on the dark theme's header background. `img/treble-clef-mark.png` (a treble clef built entirely from piano keys, also white/light transparent) is the primary brand mark, shown large on the homepage hero above the `<h1>` — it's meant to be placed wherever it can be seen in its entirety (large), never shrunk down to icon size, since the piano-key detail becomes illegible/muddy at small sizes. Both have black-on-transparent counterparts (`img/logo-mark-black.png`, `img/treble-clef-mark-black.png`) kept for any future light-background context. The **favicon** deliberately still uses `logo-mark-black.png`, not the white version — browser tab chrome is typically light regardless of the site's own theme, so a white-only icon risks being invisible there. All four were generated from source files supplied directly by the operator (`Swish.png`, `Treble.jpeg`) using a luminance-based alpha script (white background → transparent, black linework → opaque) run via Pillow — see git history for the exact script if regenerating. **Licensing status of the original source images is unconfirmed** — a sibling reference file had a visible stock-photo watermark — operator has acknowledged this and said he'll address it later; don't assume it's resolved, but also don't block on it since the operator has explicitly deprioritized it.

**Every indexable page** (all except `thanks.html`) carries a matched set of `<meta name="description">` + Open Graph (`og:title`, `og:description`, `og:image`, `og:url`) + `twitter:card` tags in `<head>`. The `og:image`/`og:url` values are hardcoded to `https://spechtlive.com/...` absolute URLs (required for social crawlers to resolve them), so they'll only actually resolve once that domain is live.

**Content has explicit placeholder vs. confirmed status** — this matters when editing copy or numbers:
- Specht Staging Solutions pricing (`specht-staging-solutions.html`) is pulled from a real rate sheet and confirmed accurate by the operator.
- DJ R3$pecht pricing (`dj-respecht.html`) is now firm, published numbers (no `~`, no placeholder callout) — derived from the Staging Solutions combo-package rates. Operator approved with "can adjust later if needed," so treat these as real but not necessarily permanent.
- Brand colors are implemented (deep purple dark theme, see Architecture above) — no longer a placeholder. Operator considered and rejected a carbon black/green option ("too Spotify") and a deep-blue option before picking purple.
- See `README.md`'s "Status / before this goes live" section for the current up-to-date checklist of what's confirmed vs. still open.
