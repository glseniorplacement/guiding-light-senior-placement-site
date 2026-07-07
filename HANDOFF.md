# Guiding Light Senior Placement — Website Handoff

Welcome. This document tells you everything you need to take ownership of the website: what's here, what to change, and how to keep it running. No prior coding experience required.

---

## 1. What you're getting

A complete, working website for **Guiding Light Senior Placement, LLC** that runs on GitHub Pages (free hosting). It's currently live at:

**https://boostbar9.github.io/guiding-light-senior-placement-site/**

Once you point your domain at it (see `DOMAIN-SETUP.md`), it will also load at your custom URL (e.g. `https://glseniorplacement.com`).

### Site structure

| File | What it is |
|---|---|
| `index.html` | The main home page (Hero, About, Services, Testimonials, Contact, FAQ preview, etc.). Everything — CSS, images, fonts — is bundled inside this one file so it never breaks. |
| `faq.html` | Full Frequently Asked Questions page |
| `areas.html` | Service areas / cities covered |
| `tour-checklist.html` | Free downloadable tour checklist page |
| `privacy.html` | Privacy policy |
| `thanks.html` | Confirmation page shown after someone submits the contact form |
| `404.html` | Friendly "page not found" page (auto-redirects to home after 3 seconds) |
| `README.md` | Short project readme |
| `HANDOFF.md` | This file |
| `DOMAIN-SETUP.md` | Step-by-step guide to connect your custom domain |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is (leave it alone) |
| `index-clean.html` | Older lightweight version — keep as a reference, do not delete |
| `index.html.bak` | Pre-fix backup of the main page — safe to delete once you're confident everything works |

---

## 2. Things you MUST update before real use

These are placeholders or unverified items. **Do not launch marketing to real customers until these are fixed.**

### 🔴 Critical — customer-facing accuracy

1. **Contact form isn't wired to send you emails yet.**
   The form points to `https://formspree.io/f/YOUR_FORMSPREE_ID`. Right now, if someone fills it out, nothing happens. Fix:
   - Sign up free at [formspree.io](https://formspree.io) with `glseniorplacement@gmail.com`
   - Create a new form, name it "Guiding Light Contact"
   - Copy the form ID (looks like `xyzabc123` or `f/moqgevkl`)
   - In `index.html`, find `YOUR_FORMSPREE_ID` and replace it with your ID
   - Test by submitting the form yourself
   - Alternative: use [Basin](https://usebasin.com), [Getform](https://getform.io), or a Netlify redirect. Any form service that accepts a plain POST works — just update the `action=` URL.

2. **Verify the phone number.** The site shows **(747) 222-6399**. That's a Los Angeles area code, which is unusual for a Camarillo/Ventura County business. Confirm this is correct and answered. If wrong, search `index.html` for `747` and replace all four instances (`href="tel:...`, displayed text in header, hero, and footer).

3. **Verify business address, LLC status, and licensing claims.**
   Currently shown: `317 W Ventura Blvd #1037, Camarillo, CA 93010`. Confirm this is your actual mailing address, not just a UPS/PO box you no longer use. Also confirm any "licensed," "insured," or "certified" language matches what your LLC actually holds today.

4. **Replace the testimonials.** The three testimonials on the site (Maria L., James T., Anna K.) are placeholders written for launch. Swap them for real families' words with **written permission**. Keep the disclaimer note about compensated/edited testimonials if any were shortened.

5. **Replace the founder photo.** The current "founder" image is a generic silhouette. Add a real headshot for credibility. Save it as `founder.jpg` in the repo and update the `<img>` tag in the About section — or, easier, use the base64 encoder trick (see section 4 below).

### 🟡 Nice to have

6. **Real logo file.** A logo is embedded as base64 inside `index.html`. If you want a crisper logo across social shares and search results, save a PNG version as `logo.png` in the repo root and update references.
7. **Social share image (`og-image.jpg`).** When someone shares your link on Facebook/LinkedIn/iMessage, the preview image comes from a `<meta property="og:image">` tag. Create a 1200×630 JPG called `og-image.jpg`, put it in the repo, and update that meta tag.
8. **Google Business Profile.** Create/claim a free listing at [business.google.com](https://business.google.com). Free, huge SEO impact for local search.
9. **Camarillo Senior Resource Guide listing.** As of the 2026 edition, the business does not appear in the guide. Contact them to be added — it's a common referral source in Ventura County.

---

## 3. How to edit the site (three options)

### Option A — Edit directly on GitHub (easiest, no software)

1. Log in at [github.com](https://github.com) as `boostbar9` (or whoever owns the repo — see section 5).
2. Open [the repo](https://github.com/boostbar9/guiding-light-senior-placement-site).
3. Click any file (e.g. `index.html`).
4. Click the pencil icon (top right) to edit.
5. Make changes.
6. Scroll down, write a short "commit message" ("Updated phone number"), and click "Commit changes."
7. GitHub Pages will rebuild in ~30 seconds. Refresh the live site to see it.

**Good for:** small text changes — phone number, hours, testimonials, adding a paragraph.

### Option B — Edit locally with VS Code (recommended for anything bigger)

1. Install [VS Code](https://code.visualstudio.com) (free).
2. Install [GitHub Desktop](https://desktop.github.com) (free).
3. In GitHub Desktop, clone the repo `boostbar9/guiding-light-senior-placement-site`.
4. Open the folder in VS Code.
5. Edit files. Preview by opening `index.html` in your browser (double-click).
6. In GitHub Desktop, write a commit message and click "Commit to main," then "Push origin."

### Option C — Hire someone / use AI

For any substantial redesign, paste `index.html` into ChatGPT / Claude / Perplexity Computer and ask for the change you want. All three can edit HTML/CSS fluently. You can also hire a freelancer on [Upwork](https://upwork.com) or [Fiverr](https://fiverr.com) — this is a standard static site, any web dev can work on it.

---

## 4. Common tasks — quick recipes

**Change the phone number:**
Search `index.html` for `747` — appears in header, hero button, footer, and JSON-LD schema. Replace all instances. Then search the other pages (`faq.html`, `areas.html`, etc.) the same way.

**Change hours or address:**
Same as above — search for the current value in `index.html` and replace. Address appears in the footer, contact section, and the JSON-LD `<script type="application/ld+json">` block near the top.

**Add a new testimonial:**
In `index.html`, search for `class="testimonial"` — you'll find the existing three. Copy one entire `<blockquote>...</blockquote>` block, paste below it, and edit the text and attribution.

**Update the founder photo:**
1. Save your headshot as a JPG, ~800px wide.
2. Convert it to base64 at [base64.guru/converter/encode/image](https://base64.guru/converter/encode/image).
3. In `index.html`, find the current founder `<img src="data:image/...">` tag and replace the whole `data:image/...` string with your new one.
   OR — save the file as `founder.jpg` in the repo and change `src="data:image/..."` to `src="founder.jpg"`.

**Change colors / fonts:**
Everything is inside a single `<style>` block near the top of `index.html`. Look for CSS custom properties (they start with `--`): `--color-primary`, `--color-accent`, `--font-heading`, etc. Change the values, save, refresh.

---

## 5. Taking ownership of the GitHub repo

Right now the repo lives under **boostbar9** (developer/friend's account). To take full ownership:

### Path 1 — Transfer the repo to your own GitHub account

1. Create a free account at [github.com](https://github.com) using `glseniorplacement@gmail.com`.
2. Ask boostbar9 to go to the repo → **Settings** → scroll to bottom → **Transfer ownership** → enter your new username.
3. Accept the transfer email.
4. Your new URL becomes `https://YOURUSERNAME.github.io/guiding-light-senior-placement-site/` (temporarily) until the custom domain is set up.
5. Re-enable GitHub Pages: **Settings** → **Pages** → source: `main` branch, folder: `/ (root)` → Save.

### Path 2 — Fork / re-upload under your own account

1. Create your GitHub account.
2. Download the repo as a ZIP from the current URL: `https://github.com/boostbar9/guiding-light-senior-placement-site` → green "Code" button → "Download ZIP."
3. On your new GitHub account, create a new repo (also named `guiding-light-senior-placement-site`, public).
4. Upload the unzipped files via the "uploading an existing file" link.
5. Enable Pages: **Settings** → **Pages** → source `main` / root.

**Recommendation:** Path 1 preserves the commit history and is faster.

---

## 6. Domain setup

See **`DOMAIN-SETUP.md`** in this same folder for a step-by-step guide to point `glseniorplacement.com` (or any domain you buy) at this GitHub Pages site.

---

## 7. Hosting alternatives (if you outgrow GitHub Pages)

GitHub Pages is free and reliable for static sites like this. But if you ever want to:
- Add a contact form that emails you *without* a third party like Formspree,
- Add password-protected areas,
- Add a booking/scheduling backend,

…then move to one of these (all have free tiers, all support this exact repo out of the box):

| Host | Best for | Free tier |
|---|---|---|
| [Netlify](https://netlify.com) | Built-in forms, easy custom domain | Yes |
| [Vercel](https://vercel.com) | Speed, easy custom domain | Yes |
| [Cloudflare Pages](https://pages.cloudflare.com) | Fastest globally, best DDoS protection | Yes |

All three: sign up → "New site from Git" → pick this repo → deploy. Takes 3 minutes.

---

## 8. Cost expectations

| Item | Cost |
|---|---|
| GitHub account | Free |
| GitHub Pages hosting | Free |
| Formspree form (50 submissions/mo) | Free |
| Formspree paid (unlimited) | $10/mo |
| Domain name (yearly) | $10–15 |
| **Total to run this site** | **$0/mo (or $10 if form volume is high) + ~$12/year for domain** |

---

## 9. Who to call for help

- **Domain / DNS problems:** the company you bought the domain from (GoDaddy, Namecheap, Google Domains, etc.). They all have free chat/phone support.
- **GitHub Pages issues:** [GitHub Docs — Pages](https://docs.github.com/pages) or the GitHub community forum.
- **Formspree issues:** [help.formspree.io](https://help.formspree.io).
- **Broad website help:** any web developer can pick this up. It's plain HTML/CSS/JS — no exotic framework.

---

## 10. Recent changes made during handoff prep (July 2026)

- Removed a duplicate embedded page block (site loads noticeably faster now)
- Fixed a broken "click to navigate" behavior that could send visitors to blank pages
- Corrected malformed structured data (helps Google understand the business)
- Rewrote the contact form to use a standard form service instead of a broken Netlify config
- Fixed the animated stats strip that briefly flashed incorrect numbers
- Improved the testimonials disclaimer wording
- Built real standalone pages for FAQ, service areas, privacy, tour checklist, and thanks (so direct links like `/faq.html` work)
- Added a friendly 404 page

Backup of the pre-fix `index.html` is preserved as `index.html.bak` — safe to delete once you're satisfied the new version works.

---

Good luck. This is a solid foundation. Keep it simple, keep it accurate, and answer your phone.
