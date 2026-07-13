# Sparkefy Website (`sparkefy.app`)

Production-ready **zero-dependency static site** for Sparkefy — the on-demand home services marketplace app.

This folder is the **canonical public site** for:

| App Store Connect field | URL |
|-------------------------|-----|
| **Marketing URL** | `https://sparkefy.app/` |
| **Support URL** | `https://sparkefy.app/support.html` |
| **Privacy Policy URL** | `https://sparkefy.app/privacy.html` |

Also includes **Terms of Service** at `https://sparkefy.app/terms.html` and an Apple App Site Association file for future Universal Links.

> **Note:** This is a new deployable package (`sparkefy-website/`). It does not replace or edit `~/sparkefy-landing/` or `~/Sparkefy-Support/`. After you deploy this site to `sparkefy.app`, point App Store Connect (and optionally iOS `Config.supportURL`) here.

---

## File tree

```
sparkefy-website/
├── index.html          # Marketing splash + download CTAs
├── privacy.html        # Full Privacy Policy (App Store Privacy URL)
├── terms.html          # Terms of Service
├── support.html        # Support + contact (App Store Support URL)
├── README.md
├── assets/
│   ├── logo.png              # Official App Icon (1024px, source of truth)
│   ├── logo-180.png
│   ├── apple-touch-icon.png
│   ├── favicon.png
│   └── logo.svg              # Stylized SVG fallback (approx)
└── .well-known/
    └── apple-app-site-association.json
```

No build step. No npm. No framework.

---

## Design system

| Token | Value |
|-------|--------|
| Primary (brand) | `#0F766E` teal |
| Accent (CTAs) | `#F59E0B` amber |
| Neutrals | Tailwind slate scale + white |
| CSS | Tailwind CDN + `tailwind.config` in each page |
| Theme | Light/dark toggle with `localStorage` |
| Typography | System sans stack |

The iOS app splash historically used green `#4AE080`. This marketing site uses the teal/amber system above for a premium home-services look. You can realign colors later if desired.

---

## Local preview

```bash
cd ~/sparkefy-website
python3 -m http.server 8080
```

Open [http://localhost:8080](http://localhost:8080).

Alternatively:

```bash
npx serve .
```

Or open `index.html` directly in a browser (`file://` works for browsing; use a local server to verify `.well-known` paths cleanly).

---

## Deploy to `sparkefy.app`

Any static host works. Recommended: **Vercel**, **Netlify**, or **Cloudflare Pages**.

### General steps

1. Create a Git repo with this folder as the **site root** (or set the host’s root directory to `sparkefy-website`).
2. Import the project on your host; deploy on push.
3. Add custom domain `sparkefy.app` (and optionally `www.sparkefy.app` → apex redirect).
4. Confirm HTTPS is active.
5. Paste the App Store Connect URLs from the table above.

### DNS (registrar)

Point the domain at your host using their instructions (usually A/ALIAS for apex and CNAME for `www`). DNS changes can take time to propagate.

### Apple App Site Association (AASA)

File path in this repo:

```
.well-known/apple-app-site-association.json
```

**Before Universal Links work in production:**

1. Replace `TEAMID` in the JSON with your Apple Developer **Team ID**.
2. Confirm bundle ID: `com.logangale.V3Sparkefy`.
3. Serve the file with `Content-Type: application/json` (and preferably **no** redirects).
4. In Xcode, add Associated Domains entitlement: `applinks:sparkefy.app`.
5. Validate with Apple’s CDN after deploy (can take time to refresh).

#### Optional host headers / rewrites

**Netlify** (`netlify.toml` example — add only if you want it):

```toml
[[headers]]
  for = "/.well-known/apple-app-site-association"
  [headers.values]
    Content-Type = "application/json"

[[headers]]
  for = "/.well-known/apple-app-site-association.json"
  [headers.values]
    Content-Type = "application/json"

[[redirects]]
  from = "/support"
  to = "/support.html"
  status = 200

[[redirects]]
  from = "/privacy"
  to = "/privacy.html"
  status = 200

[[redirects]]
  from = "/terms"
  to = "/terms.html"
  status = 200
```

**Vercel** (`vercel.json` example):

```json
{
  "headers": [
    {
      "source": "/.well-known/apple-app-site-association",
      "headers": [{ "key": "Content-Type", "value": "application/json" }]
    },
    {
      "source": "/.well-known/apple-app-site-association.json",
      "headers": [{ "key": "Content-Type", "value": "application/json" }]
    }
  ],
  "rewrites": [
    { "source": "/support", "destination": "/support.html" },
    { "source": "/privacy", "destination": "/privacy.html" },
    { "source": "/terms", "destination": "/terms.html" }
  ]
}
```

Note: Apple often expects AASA at `/.well-known/apple-app-site-association` **without** `.json`. You can copy or rewrite the file to that path on the host. The JSON body is the same either way.

---

## After App Store approval — replace placeholder download links

Search the site for:

```text
https://apps.apple.com/app/id000000000
```

Replace **every** occurrence with your real App Store URL, for example:

```text
https://apps.apple.com/us/app/sparkefy/idXXXXXXXXX
```

Locations: nav Download buttons, hero badge, final CTA, footer, mobile menu (all four HTML pages that include the badge).

---

## App Store Connect checklist

- [ ] Site live on HTTPS at `sparkefy.app`
- [ ] Marketing URL → `https://sparkefy.app/`
- [ ] Support URL → `https://sparkefy.app/support.html`
- [ ] Privacy Policy URL → `https://sparkefy.app/privacy.html`
- [ ] Support page shows real email (`sparkefysupport@gmail.com`) and mailto works
- [ ] Privacy page is substantive (not a blank stub)
- [ ] (Optional) Terms linked from app / listing as needed
- [ ] (Optional) Update iOS `Config.supportURL` to `https://sparkefy.app/support.html`
- [ ] (Optional) AASA Team ID + Associated Domains when enabling Universal Links
- [ ] App Store badge hrefs updated after the app goes live

---

## Product facts baked into copy

| Fact | Value |
|------|--------|
| Support email | `sparkefysupport@gmail.com` |
| Bundle ID | `com.logangale.V3Sparkefy` |
| Payments | Stripe |
| Platform fee | 20% of service base; tips 100% to provider |
| Booking window | 30 days for new bookings |
| Dual role | Customer + provider |
| Governing law (Terms) | Florida, USA |

---

## Legal disclaimer

Privacy and Terms pages are structured for **App Store submission and early product launch**. They are **not legal advice**. Have counsel review before fundraising, multi-state expansion, or enterprise contracts if you need formal coverage.

---

## License / ownership

© 2026 Sparkefy. All rights reserved.  
Site content prepared for the Sparkefy product and domain `sparkefy.app`.
