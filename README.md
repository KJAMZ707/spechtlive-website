# Specht Live — website

**Live at [spechtlive.com](https://spechtlive.com).**

Source for spechtlive.com. Covers both business tracks under one operator, Kieran Specht:

- **Specht Staging Solutions** — sound, lighting, and staging production/rental
- **DJ R3$pecht** — DJ performance

Plain HTML/CSS/JS, no build step, no framework. Deployed as a static site on Netlify, repo hosted at [github.com/KJAMZ707/spechtlive-website](https://github.com/KJAMZ707/spechtlive-website).

## Structure

```
index.html                        Homepage
about.html                        Bio + credentials
specht-staging-solutions.html     Services & pricing (Staging Solutions track)
dj-respecht.html                  Packages & pricing (DJ track)
gallery.html                      Photo/video gallery — 4 photos + flyer live, more being added over time
faq.html                          Booking terms, policies, service FAQs
book.html                         Inquiry form (Netlify Forms)
thanks.html                       Form submission success page
css/style.css                     Shared styles (deep purple dark theme)
js/main.js                        Mobile nav toggle
img/                              Photos + logo marks (see CLAUDE.md for asset details)
```

## Current status

**Fully deployed and operational as of this writing.** Verified end-to-end: HTTPS/SSL valid on the custom domain, HTTP→HTTPS redirect works, all 8 pages return 200, booking form tested live (submission → redirect → Netlify Forms capture → email notification arriving at booking@spechtlive.com, confirmed by an actual received email).

- [x] **Deployed** — pushed to GitHub (`main` branch), imported into Netlify, live at spechtlive.com with a valid Let's Encrypt SSL certificate.
- [x] **DNS** — domain's nameservers point to Netlify DNS (`*.p05.nsone.net`). Netlify manages all DNS records for the domain now, not GoDaddy.
- [x] **Email routing (MX record)** — `spechtlive.com` has an MX record (`1 smtp.google.com`) pointing to Google Workspace, added manually in Netlify's DNS records. This was necessary because switching nameservers to Netlify DNS did *not* carry over the Google Workspace mail configuration that used to live in GoDaddy's DNS zone — without this record, no `@spechtlive.com` address (including booking@, billing@, kieran@) can receive mail from outside senders.
- [x] **Netlify Forms fully working** — `book.html`'s form submits correctly, redirects to `thanks.html`, and triggers an email notification to booking@spechtlive.com. This required manually enabling **"Form detection"** on the Forms page in Netlify's dashboard (Site → Forms → "Enable form detection") followed by a fresh deploy — this is *not* on by default even though Netlify's UI lets you configure a notification rule for a form before detection is actually enabled, which is misleading and was the source of a real 404-on-submit bug before it was found and fixed.
- [x] Deep purple dark theme, firm DJ pricing, real photos/logo — see `CLAUDE.md` for details on what's implemented and why.
- [x] **Primary logo** — `img/treble-clef-mark.png` is now the header icon (small scale, `.logo-mark`, 28px tall) and favicon (`treble-clef-mark-black.png`) on all 8 pages, in addition to its original large placement on the homepage hero. The old ribbon "swish" mark (`img/logo-mark.png`) is no longer used as the header icon.
- [x] **DJ page header graphic** — `dj-respecht.html`'s `<h1>DJ R3$pecht</h1>` was replaced with `img/dj-graffiti-logo.png`, a gold graffiti-style wordmark cut from a supplied business-card PDF with a transparent background.
- [x] **Staging Solutions photo** — `specht-staging-solutions.html` had no photo at all; added `img/concert-silhouette.png` (purple stage-lighting shot) to the hero area.
- [x] **Homepage/DJ page photo swap** — close-crop portrait (`dj-scarf-portrait.jpg`) now on the homepage, wide shot (`dj-wedding-wide.jpg`) now on `dj-respecht.html` (previously reversed).
- [x] **Phone number in footer** — `(707) 298-8561` added to the Contact list in the footer on all 7 pages that have one (`thanks.html` has no footer-grid).
- [x] **FAQ: power/generators** — new entry under "Services & equipment" clarifying Specht Live doesn't supply major power/generators; house/venue power required, smaller mobile audio packages available on request.
- [x] **Testimonials** — 3 real quotes (The Undercovers, Bobby Amirkhan/Blue Lake Wave Lounge, Love Taaa) added to a new "What people are saying" section on `index.html`, using `.testimonial-grid`/`.testimonial-card` in `css/style.css`. Supplied directly by the operator as text, not fabricated.
- [x] **Thank-you page photo** — `img/gig-purple-rain.jpg` (DJ set from atop a light-rigged car in the rain, purple stage lighting) added to `thanks.html` after the hero-actions buttons.
- [ ] **More gallery photos** — Kieran is gathering additional photos/material to add to `gallery.html` over time; several supplied photos from this round are still unused and available for a future gallery/promo update.
- [ ] **`booking@spechtlive.com` / `billing@spechtlive.com` Google Workspace access** — was blocked by a circular "verify it's you" email-code loop on first login (no recovery phone/backup email configured on those accounts yet). Kieran has since gotten into booking@ (confirmed — he received and pasted back the test notification email). Worth setting a recovery phone number on both accounts to prevent this happening again; unclear if billing@ access has been separately resolved.
- [ ] **Logo licensing** — source images for the logo marks (`Treble.jpeg`, `Swish.png`, supplied directly by Kieran) have unconfirmed provenance; a sibling reference file had a visible stock-photo watermark. Kieran has said he'll handle this later and considers it a low near-term priority — not blocking, but not resolved either.

## Deploying (reference — already done, for redeploy/reference purposes)

The steps below are what was actually followed to get this live; keep this if the site ever needs to be redeployed from scratch (new repo, new Netlify site, etc.).

1. Push the folder to a GitHub repository. Currently: `git remote add origin https://github.com/KJAMZ707/spechtlive-website.git` then `git push -u origin main`.
   - **Auth note:** GitHub no longer accepts password auth for git operations. Use a **classic** personal access token (Settings → Developer settings → Tokens (classic) → Generate new token (classic), `repo` scope checked) as the password when prompted. Fine-grained tokens are more error-prone for this (repository-access scoping is easy to get wrong) — classic is simpler and was what actually worked here after a fine-grained token failed with a 403.
2. Sign in to [netlify.com](https://netlify.com) with GitHub, **Add new site** → **Import an existing project** → select the repo.
3. Leave build command and publish directory blank (or `/`) — static site, nothing to build.
4. Deploy.
5. **Enable Netlify Forms properly** — go to Site → **Forms**, click **"Enable form detection"** (this is a separate, non-default toggle — configuring a notification rule for a form does *not* imply detection is on), then trigger a fresh deploy (Deploys → Trigger deploy → Deploy site) so the setting actually takes effect. Skipping this causes a silent 404 on every form submission even though everything else looks correctly configured.
6. In Site → Forms → **Form submission notifications**, add an email notification for the `booking` form to booking@spechtlive.com.
7. In Netlify → Domain management, add `spechtlive.com`. Choose **Netlify DNS** (simplest — hands nameserver control to Netlify) and update the nameservers at the domain registrar (GoDaddy) to the 4 values Netlify provides.
8. **Re-add mail records** — once Netlify DNS takes over, manually re-add the Google Workspace MX record (`MX`, host `@`, priority `1`, value `SMTP.GOOGLE.COM`) in Netlify's DNS records editor for the domain, or mail stops routing for every `@spechtlive.com` address. SSL certificate provisioning also won't complete until DNS has fully propagated to Netlify.
