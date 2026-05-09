# HMB-NeSDiN — current deployment state & maintenance guide

**Live URL:** https://hmb-nesdin.org
**Email:** info@hmb-nesdin.org
**Hosting:** Netlify (free) · **DNS:** Netlify DNS · **Email:** Zoho Mail (free tier) · **Domain registrar:** Cloudflare

---

## What's live and working

- ✅ **Website** at https://hmb-nesdin.org with HMB-NeSDiN branding, custom logo, and the full Leadership section (5 Coordinators + Executive Committee placeholders + Advisory Board placeholders)
- ✅ **www.hmb-nesdin.org** redirects automatically to the apex
- ✅ **hmbns.org and www.hmbns.org** redirect automatically to hmb-nesdin.org (legacy domain preserved)
- ✅ **Free SSL certificate** via Let's Encrypt (managed by Netlify)
- ✅ **Email inbox** at info@hmb-nesdin.org via Zoho Mail (https://mail.zoho.com)
- ✅ **Membership form** submissions deliver to info@hmb-nesdin.org via Netlify Forms

## What's still pending

- 🔲 **DKIM record** in Netlify DNS — improves outgoing-mail deliverability so replies don't land in spam. Generated via Zoho Admin → Domains → Email Configuration → DKIM → Add Selector (`zmail`)
- 🔲 **DMARC record** (optional polish) — single TXT record at `_dmarc` with value `v=DMARC1; p=quarantine; rua=mailto:info@hmb-nesdin.org; pct=100; adkim=s; aspf=s`
- 🔲 **Coordinator photos** — save the 5 photos as `danda.jpg`, `nirajan.jpg`, `sudip.jpg`, `sushil.jpg`, `bishnu.jpg` into a `photos/` subfolder next to `index.html`. The HTML already references them with graceful fallback to initials, so missing files don't break anything.
- 🔲 **Real names** for Executive Committee + Advisory Board roles as they're confirmed

---

## Files in this folder

| File | Purpose |
|------|---------|
| `index.html` | The website — single self-contained page |
| `logo.svg` | Custom HMB-NeSDiN logo (mountains, sun, constellation) |
| `favicon.svg` | Browser-tab icon |
| `netlify.toml` | Netlify hosting config + security headers |
| `DEPLOY.md` | This guide |
| `golden-community-survey.html` | Separate project — bilingual survey for an elderly community in Kathmandu |
| `photos/` *(create when you have files)* | Coordinator photos (`.jpg`, lowercase, exact names below) |

---

## How to redeploy after editing the website

The setup uses Netlify's drag-and-drop deploy (no Git required).

1. Make any edits you want to `index.html` (or any other file).
2. Open **https://app.netlify.com** → click your `hmb-nesdin.org` project.
3. Click the **Deploys** tab in the left sidebar.
4. Scroll to the bottom of the Deploys page — there's a drag-and-drop zone.
5. Drag the **whole folder** containing `index.html`, `logo.svg`, `favicon.svg`, `netlify.toml`, and `photos/` (if it exists) onto the drop zone.
6. Wait ~10 seconds for "Deploy in progress" → "Published".
7. Hard-refresh https://hmb-nesdin.org (Cmd+Shift+R on Mac, Ctrl+Shift+R on Windows) to bypass browser cache.

---

## Adding coordinator photos

The HTML expects this folder structure:

```
your-folder/
  index.html
  logo.svg
  favicon.svg
  netlify.toml
  photos/
    danda.jpg
    nirajan.jpg
    sudip.jpg
    sushil.jpg
    bishnu.jpg
```

Filenames must be exactly lowercase as shown. Recommended specs: square crop (or close to it), at least 400×400 pixels, under 500 KB each. JPG or PNG both work, but the HTML references `.jpg` — so either save as `.jpg` or update the HTML to point at `.png`.

Once the `photos/` folder is in place, redeploy the whole folder and the cards will swap from "DC", "NS", etc. initials to actual photos.

---

## Adding more leadership members

Each leadership card in `index.html` is a `<div class="member">` block. To add a new person:

1. Open `index.html`, search for **"Coordinators"** or **"Executive Committee"** or **"Advisory Board"**.
2. Copy an existing card and edit:
   - The two-letter initials in the avatar div (e.g. `<div class="avatar">XX...`)
   - The `<h3>` name
   - The `<p class="role">` role
   - The `<p class="affil">` affiliation (HTML `<br/>` for line breaks)
3. If you want a photo: change `<img src="photos/XX.jpg" ...>` accordingly.
4. Save and redeploy.

---

## DKIM setup (5-min walkthrough for tomorrow)

1. Open https://mailadmin.zoho.com → **Domains** → click `hmb-nesdin.org`.
2. Click the **Email Configuration** tab → find the **DKIM** section.
3. Click **Add** (or **Add Selector**). Selector name: `zmail`. Domain auto-fills.
4. Zoho generates a key pair and shows you a **Hostname** (e.g., `zmail._domainkey`) and a long **Value** starting with `v=DKIM1; k=rsa; p=...`.
5. Open Netlify → **Domains** → `hmb-nesdin.org` → **DNS records** → **Add new record**:
   - Record type: `TXT`
   - Name: `zmail._domainkey` (just the prefix, Netlify appends the domain)
   - Value: paste the entire `v=DKIM1; k=rsa; p=...` string
   - TTL: 3600
   - Save.
6. Wait 1–5 minutes. Back in Zoho's DKIM page, click **Verify** → should turn green.

---

## Useful links

- **Netlify dashboard:** https://app.netlify.com
- **Netlify project (deploys, forms):** https://app.netlify.com/projects/nesdin-network
- **Cloudflare (domain registrar):** https://dash.cloudflare.com
- **Zoho Mail Admin Console:** https://mailadmin.zoho.com
- **Zoho Webmail (info@hmb-nesdin.org inbox):** https://mail.zoho.com

---

## Costs

- Domain `hmb-nesdin.org` at Cloudflare Registrar: ~$10/year
- Domain `hmbns.org` at Cloudflare Registrar (legacy redirect): ~$10/year (renew yearly to keep the redirect alive)
- Netlify hosting: $0
- Netlify Forms: $0 (up to 100 submissions/month)
- Zoho Mail: $0 (Forever Free Plan, 1 user, 5 GB)

**Total: ~$10–20/year**, depending on whether you keep the legacy hmbns.org domain.
