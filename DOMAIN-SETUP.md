# Custom Domain Setup — Step by Step

This guide walks you through pointing a custom domain (like `glseniorplacement.com`) at your GitHub Pages website. **Total time: 15 minutes of active work, then 1–24 hours of DNS propagation.**

---

## Before you start — what you need

- Access to the GitHub repo (as owner or with write access)
- A registered domain name. If you don't have one yet, buy from [Namecheap](https://namecheap.com), [Cloudflare Registrar](https://www.cloudflare.com/products/registrar/), or [Porkbun](https://porkbun.com) — all ~$10–13/year and reputable. Avoid GoDaddy upsells.
- Login access to your domain registrar (where you bought the domain)

---

## Step 1 — Decide: root domain, subdomain, or both?

| Option | Example | Recommended? |
|---|---|---|
| Root (apex) domain | `glseniorplacement.com` | ✅ Yes — cleanest |
| `www` subdomain | `www.glseniorplacement.com` | ✅ Yes — set as redirect to root |
| Other subdomain | `home.glseniorplacement.com` | Only if root is used elsewhere |

**Best practice:** point BOTH `glseniorplacement.com` AND `www.glseniorplacement.com` at the site, with `www` redirecting to the root. GitHub Pages handles this automatically once configured.

---

## Step 2 — Add a `CNAME` file to the repo

GitHub Pages needs to know which domain to accept traffic for.

### Easiest way (browser):

1. Go to your repo on GitHub.
2. Click **Add file** → **Create new file**.
3. Name it exactly `CNAME` (all caps, no extension).
4. In the file body, type your domain **without `https://` or `www`**:
   ```
   glseniorplacement.com
   ```
5. Scroll down, click **Commit new file**.

That's it — the file is created in `main` branch.

---

## Step 3 — Configure DNS at your registrar

Log into your domain registrar (Namecheap, GoDaddy, Cloudflare, etc.) and open the DNS management panel for your domain. You'll add **five records total: four A records + one CNAME.**

### 3a. Add four `A` records for the root domain

These point `glseniorplacement.com` (no `www`) at GitHub's servers.

| Type | Host / Name | Value | TTL |
|---|---|---|---|
| A | `@` | `185.199.108.153` | Automatic / 3600 |
| A | `@` | `185.199.109.153` | Automatic / 3600 |
| A | `@` | `185.199.110.153` | Automatic / 3600 |
| A | `@` | `185.199.111.153` | Automatic / 3600 |

> `@` means "the root domain itself" — most registrars accept `@`. If yours doesn't, leave the host blank or type the full domain.

### 3b. Add one `CNAME` record for `www`

This makes `www.glseniorplacement.com` redirect to the site as well.

| Type | Host / Name | Value | TTL |
|---|---|---|---|
| CNAME | `www` | `boostbar9.github.io` | Automatic / 3600 |

> Replace `boostbar9` with the new GitHub username if the repo has been transferred to a different account. The value is `<username>.github.io` (no path, no trailing slash).

### 3c. Remove any conflicting records

If there are existing `A` or `CNAME` records pointing `@` or `www` at some other IP (a parked page from your registrar, an old host, etc.), **delete them.** Only the GitHub records should remain.

---

## Step 4 — Configure GitHub Pages

1. Go to your repo → **Settings** → **Pages** (left sidebar).
2. Under **Custom domain**, enter `glseniorplacement.com` and click **Save**.
3. GitHub will run a DNS check. You may see a warning like "DNS check in progress" — that's normal for the first few hours.
4. Once the check succeeds (green checkmark), tick the **Enforce HTTPS** box. This gives your site a free SSL certificate — the little padlock in the browser bar. **Do not skip this.**

---

## Step 5 — Wait and verify

- DNS changes propagate anywhere from **10 minutes to 24 hours**. Usually under 1 hour.
- Test progress with [dnschecker.org](https://dnschecker.org) — search your domain, pick `A` record type, see the values propagate globally.
- Once propagated, visit `https://glseniorplacement.com`. You should see your site with a secure padlock.
- Also test `https://www.glseniorplacement.com` — it should redirect to the root.

---

## Step-by-step for popular registrars

### Namecheap

1. Log in → **Domain List** → click **Manage** next to your domain.
2. Click **Advanced DNS** tab.
3. Delete any existing "URL Redirect" or default parking records.
4. Click **Add New Record** and add each of the 4 A records + 1 CNAME above.
   - For A records, "Host" = `@`, "Value" = the IP.
   - For CNAME, "Host" = `www`, "Value" = `boostbar9.github.io.` (note the trailing dot — Namecheap requires it).
5. Save (green checkmark next to each row).

### GoDaddy

1. Log in → **My Products** → **DNS** next to your domain.
2. Delete existing conflicting `A` or `CNAME` records for `@` and `www`.
3. **Add** → select `A` → Name `@`, Points to first IP, TTL 1 Hour. Repeat for all 4 IPs.
4. **Add** → select `CNAME` → Name `www`, Points to `boostbar9.github.io`, TTL 1 Hour.
5. Save.

### Cloudflare

1. Log in → select your domain → **DNS** tab.
2. Delete existing conflicting records.
3. **Add record** → Type `A`, Name `@`, IPv4 first GitHub IP, **Proxy status: DNS only (grey cloud, not orange)**. Repeat for the other 3 IPs.
4. **Add record** → Type `CNAME`, Name `www`, Target `boostbar9.github.io`, Proxy status: DNS only.
5. Save. Cloudflare propagates in ~1 minute.

> Cloudflare tip: leave the proxy **off** (grey cloud) at first. Once GitHub's HTTPS certificate is issued, you *can* turn the orange cloud on for Cloudflare's CDN — but the DNS-only setup works fine and avoids config complications.

### Google Domains (now Squarespace Domains)

Google sold Google Domains to Squarespace in 2023. Same steps apply — the interface is under **DNS** → **Custom records**.

---

## Troubleshooting

**"DNS check in progress" won't clear after 24 hours**
- Double-check the A records match GitHub's IPs exactly (they occasionally change — verify at [docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)).
- Make sure you deleted old conflicting records.
- Run `dig glseniorplacement.com` (Mac/Linux Terminal) or check [dnschecker.org](https://dnschecker.org) to confirm what the internet actually sees.

**Site loads but shows "Not Secure" in browser**
- Wait a few more hours — the SSL certificate takes time to issue after DNS resolves.
- Then tick **Enforce HTTPS** in GitHub Pages settings.
- If it's stuck for over 24 hours: in **Settings → Pages**, remove the custom domain, save, wait 60 seconds, add it back, save. This kicks GitHub to reissue the cert.

**404 or "page not found" error**
- Make sure the `CNAME` file (from Step 2) is committed to `main` branch and contains only the domain, no `https://`, no path.
- Make sure GitHub Pages source is set to `main` / `/(root)` in **Settings → Pages**.

**`www.glseniorplacement.com` works but `glseniorplacement.com` doesn't**
- The 4 A records for the root aren't set up correctly. Recheck Step 3a.

**The old GitHub URL (`boostbar9.github.io/...`) still works too**
- That's expected and fine. It'll continue to work forever as a backup.

---

## Optional: email at your domain

Owning `glseniorplacement.com` also means you can have email like `hello@glseniorplacement.com`. GitHub Pages does *not* provide email — you need a separate provider:

| Provider | Price | Notes |
|---|---|---|
| [Google Workspace](https://workspace.google.com) | $6/user/mo | Best if already using Gmail |
| [Fastmail](https://fastmail.com) | $3/user/mo | Privacy-focused, excellent |
| [ImprovMX](https://improvmx.com) | Free | Forwards `hello@yourdomain.com` → your Gmail, no separate inbox |

Setup involves adding `MX` records at your registrar — instructions from each provider are clear. **Adding MX records does not interfere with the A/CNAME records above** — they're separate record types for separate purposes.

---

## What "success" looks like

When you're done:

- ✅ `https://glseniorplacement.com` loads the site with a green padlock
- ✅ `https://www.glseniorplacement.com` redirects to the root
- ✅ `http://` (no S) auto-upgrades to `https://`
- ✅ The site loads on phone, tablet, and computer
- ✅ Contact form is wired to Formspree (or equivalent) and you've tested submitting it to yourself

You now own a professional website. Update it any time using the guide in `HANDOFF.md`.
