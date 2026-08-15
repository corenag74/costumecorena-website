# Shop Build — Project Log

Running record of the shop/dashboard build for costumecorena.com: what was built, why,
what's still open, and what to do next. Keep this updated as things change.

---

## What was requested

Add a page to costumecorena.com where Corena can list handmade costume pieces for
sale, accept PayPal (and ideally Google Pay), and — as the catalog grows — manage
products (add/edit/remove) herself without needing a developer each time.

## What was built

**Repo:** [github.com/corenag74/costumecorena-website](https://github.com/corenag74/costumecorena-website)
(this repo). Deployed to the existing Netlify site `costumecorena` (site ID
`6d1836cc-f294-4055-8276-a549357593fb`), now linked via GitHub → continuous deployment
(previously the site was deployed by drag-and-drop with no Git history).

- **`shop.html`** — new Shop page, matches the existing site's dark purple/lime
  design. Renders product cards from `data/products.json`: photo(s) with a
  thumbnail strip for multi-angle items, name, price, description, in-stock badge,
  and a Buy button.
- **PayPal checkout** — dynamic, via PayPal's JS SDK (Smart Payment Buttons). The
  button reads each product's price at checkout time and creates the order
  client-side — **no per-product setup on PayPal's side ever needed**, so the
  catalog can grow through the dashboard alone. Until a PayPal Client ID is
  configured, products show a **"Contact to Purchase"** mailto link instead, so
  nothing is ever unbuyable.
- **`/admin` dashboard** — Decap CMS (open-source, free), backed by **Netlify
  Identity + Git Gateway**. Every dashboard edit becomes a git commit to this repo,
  which auto-redeploys the live site in under a minute. Two collections:
  - **Shop Products** (`data/products.json`) — add/edit/delete products: name,
    price, description, photos, and an **In Stock / Available** toggle (turn off
    to mark a one-off piece as sold without deleting it — shows a "Sold / Not
    Available" badge and hides the Buy button; delete instead if you'd rather it
    disappear entirely).
  - **Store Settings** (`data/settings.json`) — PayPal Client ID + currency.
- **`index.html`** — added a "Shop" nav link + the standard Netlify Identity
  redirect snippet (handles the invite-acceptance flow landing back on `/admin/`).
- **`netlify.toml`** — static site, no build step (`publish = "."`, empty build
  command).

Nothing here required Netlify Functions, Blobs, or any paid tier — everything runs
on Netlify's free plan plus Decap CMS (free/open-source) and PayPal's JS SDK (free,
just standard PayPal transaction fees on sales).

## Current status (as of this writing)

- ✅ Shop page live at `costumecorena.com/shop.html`, "Shop" in the main nav.
- ✅ GitHub repo linked to the Netlify site; pushes to `main` auto-deploy.
- ✅ Netlify Identity + Git Gateway enabled and working; **dashboard login
  confirmed working** — Corena is logged into `costumecorena.com/admin` and can
  edit `data/products.json` / `data/settings.json`.
- ✅ DNS fixed for `costumecorena.com` at Network Solutions — root domain now uses
  **A records** (`52.52.192.191`, `13.52.188.95`, Netlify's load balancer IPs)
  instead of the old apex CNAME, which was silently blocking all email to
  `@costumecorena.com` addresses (CNAME and MX can't coexist at the same DNS name).
  `www` was also cleaned up (had a stale "under construction" A record conflicting
  with the real one).
- ✅ **Missing MX/SPF records added by a Network Solutions live support agent**
  (self-service DNS panel + automatic email-forwarding provisioning never created
  these; needed a human on their end). Propagation quoted at 4-6 hours — not yet
  independently re-confirmed as fully resolved end-to-end (i.e. a real test email
  actually landing in the inbox), so treat as "likely fixed, verify with a real
  test send."
- ✅ **Fixed a second, unrelated blocker: Netlify build failures.** After Git
  Gateway was enabled, every subsequent push started failing with "Build blocked:
  unrecognized Git contributor" (Netlify's free-plan restriction on private repos
  — though this repo shows as public, the restriction still triggered). This also
  explained the intermittent `/admin` "Git Gateway backend is not returning valid
  settings" errors. **Fix:** Netlify → Team → **Git Contributors** → Edit settings
  → connect/link the GitHub account there (separate from the earlier Git Gateway
  GitHub OAuth authorization — this is a distinct "recognized contributor" link).
  Confirmed fixed: retried deploy published successfully, `/admin` login now
  working. **If this error reappears** (e.g. after a new commit from a different
  author identity), the fix is the same: Netlify → Team members → Git Contributors.
- ⏳ **PayPal Client ID not yet set.** Blocked on Corena's PayPal Business account
  verification (see below). Shop currently shows "Contact to Purchase" as a
  result — functional, just not automated checkout yet.
- ⏳ **Products not yet added.** `data/products.json` is intentionally empty — no
  placeholder/fake products were seeded. ~289 raw product photos exist locally at
  `D:\costumes\products\JPEG` (multiple angles per item, originally unlabeled);
  Corena is renaming/organizing them before they get added through the dashboard.

## Outstanding items / next steps

1. **Finish photo renaming**, then add real products via `/admin` (name, price,
   description per item).
2. **Send a real test email** to `pay@costumecorena.com` and/or
   `info@costumecorena.com` once the 4-6 hour propagation window has passed, to
   confirm the MX/SPF fix actually resolved delivery end-to-end.
3. **PayPal verification**: Corena's PayPal account (`corenag@gmail.com`) needs to
   finish Business-account verification before a Live Client ID can be generated at
   developer.paypal.com → Apps & Credentials. Was stuck on an unconfirmed
   `pay@costumecorena.com` business email tied to the (now-fixed) DNS/MX issue —
   worth rechecking whether verification unblocks now that email is working.
4. Once PayPal Client ID is in hand: paste into `/admin → Store Settings → Payment
   Settings`, publish. No code changes or redeploy needed.
5. **Optional later:** true Google Pay support needs a gateway with Google Pay
   explicitly enabled (PayPal Business, once verified, or Stripe) — current PayPal
   buttons already support card/bank/PayPal-balance checkout without a PayPal
   account on the buyer's side.

## Key reference

- Netlify site: `costumecorena` → https://costumecorena.com
- GitHub repo: https://github.com/corenag74/costumecorena-website
- Setup steps (already completed, kept for reference): [`README-SHOP-SETUP.md`](README-SHOP-SETUP.md)
- Product photos (not in repo): `D:\costumes\products\JPEG` (local machine)
