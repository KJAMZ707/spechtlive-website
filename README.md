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
css/style.css                     Shared styles (placeholder brand palette)
js/main.js                        Mobile nav toggle
```

## Status / before this goes live

- [ ] Swap the placeholder color palette (`css/style.css` `:root` variables) for real brand colors/logo once designed
- [ ] Confirm final DJ R3$pecht dollar figures — currently placeholder ranges in `dj-respecht.html`, flagged with a callout box
- [x] Real photos in place on Home (hero), DJ R3$pecht (portrait + Wave Lounge flyer), About (studio shot), and Gallery (4 photos + flyer). All resized/compressed for web via `sips`. Still worth adding more over time — Instagram @dj_respecht and Facebook are the ongoing source — but the site no longer has bare placeholder sections.
- [ ] Add testimonials once collected
- [ ] Confirm the underlying rate sheet figures are final enough to publish as-is
- [x] Interim logo/favicon in place — `img/logo-mark.svg`, a piano-key "scarf" ribbon (Kieran's established personal signature), used as the header icon and favicon. Can be replaced with a fully designed logo later, but this isn't a random placeholder.
- [ ] **Before deploying:** the site root currently has ~12 raw phone photos (`PXL_*.jpg/.mp4`), `BLUE_KJ.jpg`, and `KJ CAL POLY.pdf` sitting loose outside `img/` — these were a review batch, only a few got used (now copied into `img/` under descriptive names). Move or delete the unused originals before running `git add .`, or they'll get committed and bloat the repo.

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
