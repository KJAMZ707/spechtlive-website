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
lessons.html                      Private lessons, group lessons, workshops/clinics (Education track)
gallery.html                      Photo/video gallery — past shows + upcoming, more being added over time
faq.html                          Booking terms, policies, service FAQs
book.html                         Event inquiry form (Netlify Forms — "booking")
thanks.html                       Form submission success page
css/style.css                     Shared styles (deep purple dark theme)
js/main.js                        Mobile nav toggle
img/                              Photos + logo marks (see CLAUDE.md for asset details)
source-material/                  Raw supplied files (business card PDFs, etc.) — gitignored, not web assets
```

## Current status

**Fully deployed and operational as of this writing.** Verified end-to-end: HTTPS/SSL valid on the custom domain, HTTP→HTTPS redirect works, all 9 pages return 200, `booking` form tested live (submission → redirect → Netlify Forms capture → email notification arriving at booking@spechtlive.com, confirmed by an actual received email).

- [x] **Deployed** — pushed to GitHub (`main` branch), imported into Netlify, live at spechtlive.com with a valid Let's Encrypt SSL certificate.
- [x] **DNS** — domain's nameservers point to Netlify DNS (`*.p05.nsone.net`). Netlify manages all DNS records for the domain now, not GoDaddy.
- [x] **Email routing (MX record)** — `spechtlive.com` has an MX record (`1 smtp.google.com`) pointing to Google Workspace, added manually in Netlify's DNS records.
- [x] **Netlify Forms fully working** — `book.html`'s `booking` form submits correctly, redirects to `thanks.html`, and triggers an email notification to booking@spechtlive.com. Required manually enabling **"Form detection"** in Netlify's dashboard (Site → Forms → "Enable form detection", not on by default) followed by a fresh deploy.
- [x] Deep purple dark theme, firm DJ pricing, real photos/logo, testimonials, upcoming/past events — see `CLAUDE.md` for full details on what's implemented and why.
- [x] **Lessons & Workshops page** (`lessons.html`) — new third track alongside Staging Solutions and DJ R3$pecht. Private lessons ($30/hr, $20/hr first lesson) are bookable now via a new `lesson-inquiry` Netlify form; Group Lessons and Workshops/Clinics are published as pricing/subject reference only, clearly labeled "Program launching Mid-Sept–Early Oct 2026" (not bookable yet — no live class dates or 24-hour advance/walk-up cutoff logic implemented yet, since there's nothing to book against). A second new Netlify form, `rental-inquiry`, handles simple instrument-rental requests (not a payment flow). Photo: `img/music-room.jpg`. Homepage's two-track section became a three-card "Three ways to work with Kieran" section (`track-grid` CSS now supports 3 columns).
  - **Action needed in Netlify dashboard:** add **Form submission notifications** for the new `lesson-inquiry` and `rental-inquiry` forms (Site → Forms → Form submission notifications), same as the existing `booking` form has. Not something git/deploy can do — must be done manually once, same one-time step as the original Forms setup.
- [ ] **Group Lessons / Workshops booking logic** — deferred until Kieran has real class dates to list. Will need: a per-listing date + location field, and JS (or a small backend) to auto-switch advance vs. walk-up pricing at the 24-hour-before-event mark.
- [ ] **More gallery photos** — Kieran is gathering additional photos/material to add to `gallery.html` over time.
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
