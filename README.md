# magicthegathering.com.au

A single-page GitHub Pages site that greets visitors to **magicthegathering.com.au**, shows a
short message, and redirects them to **magicthegathering.com** after ~4 seconds. Traffic is
measured with Google Analytics 4.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The landing + redirect page (message, countdown, GA4, redirect). |
| `404.html` | Same logic, so any deep link / typo'd path also gets redirected. |
| `CNAME` | Tells GitHub Pages to serve the custom domain `magicthegathering.com.au`. |
| `README.md` | This file. |

## How the redirect works

1. **Normal (JS on):** a 4-second countdown, then a GA4 `redirect` event fires and the browser
   is sent to `magicthegathering.com`.
2. **No JS:** a `<meta http-equiv="refresh">` fallback fires at 6 seconds.
3. **Manual:** a visible "Click here if you're not redirected" link always works.

The 4-second delay is deliberate — it gives the Google Analytics beacon time to fire before the
browser navigates away.

## Analytics

- GA4 Measurement ID: **`G-XHTJXRNJMZ`** (already embedded in `index.html` and `404.html`).
- You get the full GA4 dashboard: real-time, sessions, geography, referrers, devices.
- Each landing sends the automatic `page_view` plus a custom **`redirect`** event, so you can
  count "people who landed and got forwarded" distinctly.
- **Where to look:** GA4 → *Reports → Realtime* (immediate), and *Reports → Engagement → Events*
  for the `redirect` event (can take 24–48h to populate the standard reports the first time).

## One-time setup

### 1. Push this repo to GitHub

```sh
git remote add origin https://github.com/Nedmarqs/magicthegathering-au.git
git branch -M main
git push -u origin main
```

### 2. Enable GitHub Pages

Repo → **Settings → Pages**:
- **Source:** Deploy from a branch
- **Branch:** `main` / `root`
- Save. GitHub will detect the `CNAME` file and set the custom domain to
  `magicthegathering.com.au`.

### 3. Point your domain's DNS at GitHub Pages

Because this is an **apex** domain (`magicthegathering.com.au`, no `www`), add these **A records**
at your DNS provider, all for the host `@` (root):

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

(Optional but recommended — also add these **AAAA records** for IPv6:)

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

DNS changes can take anywhere from a few minutes to a few hours to propagate.

### 4. Turn on HTTPS

Back in **Settings → Pages**, once the domain verifies, tick **Enforce HTTPS**.
(The checkbox may be greyed out until GitHub finishes issuing the certificate — this can take up
to ~24h after DNS resolves. Just check back.)

## Verifying it works

- **Locally:** open `index.html` in a browser → confirm the countdown runs, it redirects at 4s,
  and the fallback link works.
- **Live:** visit `https://magicthegathering.com.au` → you should be forwarded, and the visit
  should appear in GA4 **Realtime** within a few seconds.
- **No-JS:** disable JavaScript and reload → the meta-refresh fallback should still forward you.

## Changing things later

- **Redirect target:** edit the `DEST` constant in the `<script>` of both `index.html` and
  `404.html`.
- **Delay:** edit the `SECONDS` constant in both files (and the `content="6;..."` meta-refresh
  fallback, keeping it a couple seconds longer than `SECONDS`).
- **Message wording:** edit the `<h1>` / `<p>` text in both files.
