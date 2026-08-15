# Shop Setup — One-Time Steps

Everything code-related is already built and pushed. Three things only you can do
(they need your Netlify/PayPal login), each a few minutes:

## 1. Link this GitHub repo to your Netlify site

1. Go to https://app.netlify.com/projects/costumecorena
2. **Site configuration → Build & deploy → Continuous deployment → Link repository**
   (or "Link site to Git" if you see that instead)
3. Choose GitHub → authorize if asked → select **corenag74/costumecorena-website**
4. Branch to deploy: `main`. Build command: leave blank. Publish directory: `.` (just a dot).
5. Save. Netlify will redeploy from this repo — your site should look identical, plus
   the new **Shop** link in the nav.

## 2. Turn on the dashboard login (Netlify Identity + Git Gateway)

1. Same site in Netlify → **Site configuration → Identity → Enable Identity**
2. Under Identity → **Registration**, set it to **Invite only** (so random people can't
   sign up to your dashboard).
3. Under Identity → **Services → Git Gateway → Enable Git Gateway**.
4. Still in Identity, click **Invite users**, enter your own email, and accept the
   invite email it sends you (it'll drop you on costumecorena.com to set a password).

This is all inside your existing free Netlify plan — Identity's free tier covers up to
1,000 users and you'll only ever have one (you).

Once that's done, go to **costumecorena.com/admin** and log in. You'll see two
sections: **Shop Products** and **Store Settings**.

## 3. Turn on PayPal checkout

1. Create a free account at https://developer.paypal.com if you don't have one
   (use the same email as your regular PayPal, or a new PayPal Business account —
   either works, business is recommended so payouts land in a business balance).
2. Dashboard → **Apps & Credentials** → switch to **Live** (not Sandbox) → **Create App**.
3. Copy the **Client ID** it gives you (a long string starting with something like `A...`).
4. Go to **costumecorena.com/admin → Store Settings → Payment Settings**, paste it
   into **PayPal Client ID**, save/publish.

That's it — every product with a price and a photo will now show a working PayPal
"Buy Now" button that charges that exact price, no per-product setup needed on
PayPal's side. Until you paste a Client ID, products show a **"Contact to Purchase"**
button instead, so nothing is ever unbuyable.

> **Note on Google Pay:** true Google Pay support needs a payment gateway (PayPal or
> Stripe) with Google Pay explicitly enabled on the account, which usually requires a
> short PayPal business verification. The buttons above already give buyers PayPal
> balance, bank transfer, and card checkout (no PayPal account required to pay by
> card). If you specifically want the Google Pay button to show up too, say the word
> and we can look at enabling it on your PayPal Business account once it's set up, or
> switch to Stripe (which supports Google Pay out of the box).

## Adding / editing / removing products day-to-day

1. Go to **costumecorena.com/admin**, log in.
2. **Shop Products → Product Catalog.**
3. Click into the list to add a new item, or edit/delete an existing one.
4. Each product needs: an ID (e.g. `leather-armor-01`, no spaces), a name, a price,
   an optional description, at least one photo (drag & drop or upload), and an
   "In Stock" toggle (turn it off to hide the Buy button without deleting the item).
5. Hit **Publish**. The live site rebuilds automatically in under a minute.
