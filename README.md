# Portfolio Timeline

Interactive horizontal portfolio timeline built with GSAP ScrollTrigger.
Cloudflare Pages deployment with Zero Trust admin protection.

---

## Project structure

```
timeline/
├── index.html          ← Public timeline (view only)
├── admin/
│   └── index.html      ← Editor (Zero Trust gated)
├── assets/
│   └── css/
│       └── shared.css  ← Design system / shared styles
├── _headers            ← Cloudflare security & cache headers
├── _redirects          ← Cloudflare Pages routing
└── README.md
```

---

## Deploy to Cloudflare Pages

### 1. Push to GitHub

Create a new repo and push this folder as the root:

```bash
git init
git add .
git commit -m "Initial timeline"
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

### 2. Connect to Cloudflare Pages

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages** → **Create application** → **Pages**
2. Connect your GitHub repo
3. Build settings:
   - **Framework preset:** None
   - **Build command:** _(leave blank)_
   - **Build output directory:** `/` (root)
4. Deploy

### 3. Custom domain (optional)

In Cloudflare Pages → Custom domains → add your domain.

---

## Protect /admin with Zero Trust

1. Go to [one.dash.cloudflare.com](https://one.dash.cloudflare.com) → **Access** → **Applications** → **Add an application**
2. Choose **Self-hosted**
3. Settings:
   - **Application name:** Timeline Admin
   - **Application domain:** `your-domain.com/admin`
   - **Session duration:** as needed
4. Create a **policy** — e.g. Allow → Emails → `your@email.com`
5. Save. Cloudflare now gates `/admin` before the page loads.

No auth UI needed — Zero Trust handles everything at the network edge.

---

## Managing content

### Via admin panel (`/admin`)

- **Add entry:** click Add in the sidebar
- **Edit:** click any entry in the sidebar list
- **Reorder:** drag entries up/down in the sidebar
- **Media:** paste Cloudinary URLs directly into each media item's URL field
- **Save:** click "Save Entry" — persists to browser `localStorage`
- **Export:** downloads `timeline-data.json` (keep a backup)
- **Import:** paste or load JSON to bulk-replace all entries

### Data storage

Data lives in `localStorage` under key `portfolio_timeline_v1`.

To make edits permanent across devices/browsers, **export JSON after editing** and either:
- Paste it into the `DEFAULT_ENTRIES` array in `index.html` and `admin/index.html`, or
- Host the JSON file and fetch it at runtime (future enhancement)

---

## Cloudinary URLs

In the admin, paste full Cloudinary URLs including any transformation params:

```
https://res.cloudinary.com/YOUR_CLOUD/image/upload/w_1200,q_auto,f_auto/your-image.jpg
```

Recommended transformations: `w_1200,q_auto,f_auto` for images.

---

## Media types supported

| Type     | URL format example |
|----------|--------------------|
| Image    | Any direct image URL (Cloudinary, etc.) |
| YouTube  | `https://www.youtube.com/watch?v=XXXXX` |
| Vimeo    | `https://vimeo.com/XXXXX` |
| Video    | Direct `.mp4` URL |

---

## WCAG / Accessibility

- Keyboard navigation: `←` `→` arrows to move through timeline, `Enter` to open, `Esc` to close
- Focus trap in lightbox dialog
- `aria-live` progress counter
- Skip link to main content
- Reduced motion: all GSAP animations disabled when `prefers-reduced-motion: reduce` is set
- All images require alt text (enforced in admin form)

---

## Dark / light mode

Toggle in the header. Preference saved to `localStorage` under `portfolio_theme`.

---

## Customising copy

Edit the intro panel copy directly in `index.html` — search for `intro-panel__heading` to find it.

Change the logo text by searching for `Portfolio` in the `<header>` of each file.
