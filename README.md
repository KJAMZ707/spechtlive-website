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
- [x] Real logo assets in place: `img/logo-mark.png` (small piano-key ribbon "swish") as the header icon and favicon, `img/treble-clef-mark.png` (treble clef built from piano keys) as the primary brand mark, shown large on the homepage hero. **⚠️ Provenance unconfirmed** — both files were supplied directly by Kieran from a "BRAND IDEAS" folder; one sibling file (`BRAND IDEAS.jpeg`, not used) had a visible Depositphotos watermark, so these may be stock-derived. Kieran has said he'll address licensing later — don't treat this as resolved, follow up before the site goes fully public.
- [x] Open Graph / Twitter card tags added to all indexable pages (`og:title`, `og:description`, `og:image`, `og:url`) so links shared to Instagram/Facebook/iMessage show a real preview card instead of nothing. Images point to `https://spechtlive.com/...` — will resolve once the domain is live.
- [x] Repo is clean and ready to push — loose review photos/video/PDF moved to `../website-photo-review-batch/` (sibling folder, not inside the site), local git repo initialized with an initial commit on `main`.

### Still open (not launch-blockers, but worth finalizing)
- Real color palette/logo — currently placeholder navy/teal/rust
- Final DJ R3$pecht dollar figures — currently placeholder ranges in `dj-respecht.html`, honestly flagged with a callout box
- Testimonials — none collected yet
- Confirm the Rate Sheet v2 figures are still accurate to publish as-is

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
