# Specht Live — website

Source for spechtlive.com. Covers both business tracks under one operator, Kieran Specht:

- **Specht Staging Solutions** — sound, lighting, and staging production/rental
- **DJ R3$pecht** — DJ performance

Plain HTML/CSS/JS, no build step, no framework. Deploys as a static site.

## Structure

```
index.html                        Homepage
about.html                        Bio + credentials
specht-staging-solutions.html     Services & pricing (Staging Solutions track)
dj-respecht.html                  Packages & pricing (DJ track)
gallery.html                      Photo/video gallery (placeholder until photos are curated)
faq.html                          Booking terms, policies, service FAQs
book.html                         Inquiry form (Netlify Forms)
thanks.html                       Form submission success page
css/style.css                     Shared styles (deep purple dark theme)
js/main.js                        Mobile nav toggle
```

## Status / before this goes live

- [x] Real dark "deep purple" theme implemented in `css/style.css` — background `#170f22`/`#211630`, accent purple `#C9A6F5`, gold `#E8B84B` for DJ-track differentiation (staging=purple, DJ=gold), near-black footer `#0d0815`. Chosen over a carbon (black/green — "too Spotify") and deep-blue alternative, which were mocked up and rejected. Logo marks were regenerated as white/light transparent PNGs (`logo-mark.png`, `treble-clef-mark.png`) since the originals were black-on-transparent and would've disappeared on the new dark background; black transparent versions kept as `*-black.png` for any future light-context use. Favicon intentionally still uses the black version (`logo-mark-black.png`) since browser tab chrome is typically light regardless of site theme.
- [x] DJ R3$pecht dollar figures are now firm published numbers in `dj-respecht.html` (Ceremony/Cocktail $175 flat, Reception/Party $700/$825, Full Wedding $1,150/$1,400, additional hour $125/hr, Guest DJ/Festival $250–350). Derived from the confirmed Staging Solutions combo-package rates per the original plan doc's pricing logic, approved by Kieran with "can adjust later if needed." The placeholder callout box has been removed from the page.
- [x] Real photos in place on Home (hero), DJ R3$pecht (portrait + Wave Lounge flyer), About (studio shot), and Gallery (4 photos + flyer). All resized/compressed for web via `sips`. Still worth adding more over time — Instagram @dj_respecht and Facebook are the ongoing source — but the site no longer has bare placeholder sections.
- [ ] Testimonials — none collected. Kieran will collect real ones; explicitly decided against generating fake/generic testimonials (would be misleading advertising for a real business) — do not suggest fabricating these.
- [x] Rate Sheet v2 confirmed accurate as published by Kieran — no changes needed to Specht Staging Solutions pricing.
- [x] Real logo assets in place: `img/logo-mark.png` (small piano-key ribbon "swish") as the header icon, `img/treble-clef-mark.png` (treble clef built from piano keys) as the primary brand mark, shown large on the homepage hero. Provenance/licensing was unconfirmed (a sibling reference file had a visible stock watermark) — Kieran has stated this is handled and not a near-term concern.
- [x] Open Graph / Twitter card tags added to all indexable pages (`og:title`, `og:description`, `og:image`, `og:url`) so links shared to Instagram/Facebook/iMessage show a real preview card instead of nothing. Images point to `https://spechtlive.com/...` — will resolve once the domain is live.
- [x] Repo is clean and ready to push — loose review photos/video/PDF moved to `../website-photo-review-batch/` (sibling folder, not inside the site), local git repo initialized with commits on `main`.

### Still open (not launch-blockers, but worth finalizing)
- Testimonials — none collected yet; real ones only, no fabricated placeholders.

## Deploying (Netlify)

1. Push this folder to a new GitHub repository (see below).
2. Sign in to [netlify.com](https://netlify.com) with your GitHub account.
3. Click "Add new site" → "Import an existing project" → select this repo.
4. Netlify auto-detects it as a static site — no build command or publish directory needed (leave both blank, or set publish directory to `/`).
5. Deploy. Netlify Forms is enabled automatically for `book.html` because the form has `data-netlify="true"` — submissions appear under the site's **Forms** tab in Netlify, and you can turn on email notifications there to reach booking@spechtlive.com.
6. In Netlify → Domain management, add `spechtlive.com` and follow the DNS instructions — you'll either update nameservers at GoDaddy to point to Netlify, or add the specific A/CNAME records Netlify gives you directly in GoDaddy's DNS settings.

## Pushing to GitHub

Run these in Terminal, from inside this folder:

```bash
cd "path/to/spechtlive-website"
git init
git add .
git commit -m "Initial site build"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO-NAME.git
git push -u origin main
```

Create the empty repository on github.com first (no README/license needed, since this folder already has one), then swap in your actual username and repo name above.
