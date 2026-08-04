# LoyaltyScan landing page — setup guide

## 1. Push the files to your repo

In your local `LoyaltyScan-Website` repo folder:

```
git add index.html CNAME
git commit -m "Add landing page"
git push origin main
```

(If your default branch is called `master` instead of `main`, use that instead.)

## 2. Turn on GitHub Pages

1. Go to your repo on GitHub → **Settings** → **Pages**.
2. Under "Build and deployment", set **Source** to `Deploy from a branch`.
3. Branch: `main`, folder: `/ (root)`. Save.
4. Under "Custom domain", enter `loyaltyscan.app` and save (this also writes the CNAME file — you've already got it committed, which is fine, it'll match).

## 3. Point your domain at GitHub Pages

Go to wherever you bought `loyaltyscan.app` (registrar's DNS settings) and add these records:

**For the apex domain (loyaltyscan.app):**

| Type | Name | Value |
|------|------|-------|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

**If you also want `www.loyaltyscan.app` to work:**

| Type | Name | Value |
|------|------|-------|
| CNAME | www | `<your-github-username>.github.io` |

DNS changes can take anywhere from a few minutes to a few hours to propagate.

## 4. Enable HTTPS

Once GitHub detects the DNS is pointing correctly (back in Settings → Pages), tick **Enforce HTTPS**. This may take a little while to become available after DNS propagates — just check back.

## 5. Set up Google Analytics 4

1. Go to [analytics.google.com](https://analytics.google.com) → **Admin** → **Create Property**.
2. Name it "LoyaltyScan", set your timezone/currency, and create a **Web** data stream with URL `https://loyaltyscan.app`.
3. Copy the **Measurement ID** it gives you (looks like `G-XXXXXXXXXX`).
4. Open `index.html` and replace **both** occurrences of `G-XXXXXXXXXX` with your real ID (one in the `<script src=...>` tag, one in `gtag('config', ...)`).
5. Commit and push the change.

## What's already wired in

- **Automatic page view tracking** via the GA4 snippet.
- **Outbound click tracking**: clicking either store badge fires a `click_store_badge` GA4 event with a `store` parameter (`app_store` or `play_store`), so you can see landing-page-visit → store-click conversion in GA4 under Reports → Engagement → Events.
- **Referrer tracking is automatic** — GA4 records where each visitor came from without extra setup.

## Tracking where traffic comes from

Whenever you share the link (Reddit, forums, socials, etc.), add UTM parameters so GA4 can break it down by source:

```
https://loyaltyscan.app/?utm_source=reddit&utm_medium=post&utm_campaign=loyaltyscan_launch
```

These show up automatically under **Reports → Acquisition → Traffic acquisition** in GA4 — no extra code needed.
