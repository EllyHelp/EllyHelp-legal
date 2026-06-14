# Ellyhelp Legal Site — How to deploy

This folder contains a small, self-contained static website with three legal pages:

- `/` — landing page linking to the docs
- `/privacy` — Privacy Policy
- `/terms` — Terms of Service
- `/delete-account` — Account & Data Deletion
- `/404` — friendly "page not found" page

No build step needed. No JavaScript. No external dependencies. Just open the HTML files in a browser and they work — which means any static host will serve them.

## What to do with the "TODO BEFORE PUBLISHING" dates

The three legal documents currently say `[TO BE FILLED IN BEFORE PUBLISHING]` at the top. **Before you make the site public,** open each HTML file in a text editor and replace that bracketed text with the actual date (e.g. `14 June 2026`).

Files to update:
- `privacy/index.html`
- `terms/index.html`
- `delete-account/index.html`

Search for `TO BE FILLED IN BEFORE PUBLISHING` in each. There's exactly one occurrence per file.

## Option 1 — GitHub Pages (free, recommended)

1. Create a free GitHub account at github.com if you don't have one.
2. Create a new repository called `ellyhelp-legal` (public).
3. Upload **all the files in this folder** to the repository — preserving the folder structure (`privacy/`, `terms/`, `delete-account/` should all be top-level directories in the repo).
4. In the repo, go to **Settings → Pages**.
5. Under "Source", select "Deploy from a branch" and pick `main` branch, `/ (root)` folder.
6. Save. GitHub gives you a URL like `https://yourusername.github.io/ellyhelp-legal/`.
7. Test: open `https://yourusername.github.io/ellyhelp-legal/privacy/` — you should see the privacy policy.

That's it. You can now use those URLs in the app:
- `https://yourusername.github.io/ellyhelp-legal/privacy`
- `https://yourusername.github.io/ellyhelp-legal/terms`
- `https://yourusername.github.io/ellyhelp-legal/delete-account`

### Adding a custom domain (optional, ~£10/year)

Once you've bought a domain like `ellyhelp.app` from Namecheap:

1. In your GitHub repo: **Settings → Pages → Custom domain** → enter `ellyhelp.app`.
2. In Namecheap: **Domain List → Manage → Advanced DNS**.
3. Add four A records for the apex domain (`@` host), all pointing to the GitHub Pages IPs:
   - `185.199.108.153`
   - `185.199.109.153`
   - `185.199.110.153`
   - `185.199.111.153`
4. Add a CNAME record: host `www`, value `yourusername.github.io.` (note the trailing dot).
5. Back in GitHub Pages settings, tick **Enforce HTTPS**.
6. Wait 1-24 hours for DNS to propagate.

After that, your URLs become:
- `https://ellyhelp.app/privacy`
- `https://ellyhelp.app/terms`
- `https://ellyhelp.app/delete-account`

## Option 2 — Vercel (free, slightly nicer admin)

1. Sign up at vercel.com (free tier is plenty).
2. Push these files to a GitHub repo (same as Option 1 step 1-3).
3. In Vercel: **Add new project → Import the repo**.
4. Vercel auto-detects it as a static site. Click Deploy.
5. You get a URL like `https://ellyhelp-legal.vercel.app/`.
6. To add a custom domain: **Project → Settings → Domains** → add `ellyhelp.app` → Vercel walks you through the DNS records.

Vercel's main advantage is they handle custom-domain DNS more smoothly than GitHub Pages.

## Option 3 — Netlify (also free)

Same as Vercel, different brand. Drag-and-drop this entire folder onto **app.netlify.com/drop** and you get an instant URL like `https://random-name-123.netlify.app/`. Custom domain costs the same as elsewhere (~£10/year for the domain itself, Netlify hosting is free).

## After hosting — update the app

Once you have working URLs (whether from GitHub Pages, Vercel, or with a custom domain), open the Ellyhelp app source:

```
app/(tabs)/settings.tsx
```

Find these two lines near the top of the `SettingsScreen` function:

```
const PRIVACY_URL = "https://ellyhelp.app/privacy"; // TODO: update before launch
const TERMS_URL = "https://ellyhelp.app/terms";    // TODO: update before launch
```

Replace them with your real URLs and rebuild the app. The Settings tab's "Privacy Policy" and "Terms of Service" links will then open the live pages.

## What I deliberately didn't add

- **No analytics tracking** (no Google Analytics, Plausible, etc.) — pages must load fast and not track visitors of a privacy policy, which would be ironic
- **No web fonts** — system fonts only, so pages load instantly without privacy concerns about font loading
- **No JavaScript** — pure HTML/CSS, works in any browser including text-only ones used by accessibility tools
- **No third-party CSS frameworks** — all styles inline so there's nothing to break in deployment

This is intentional. Legal pages should be boring, fast, accessible, and trustworthy — not flashy.

## Testing locally before you deploy

You can test the site on your Mac before uploading anywhere:

```
cd path/to/this/folder
python3 -m http.server 8000
```

Then visit `http://localhost:8000/` in your browser. Click around — make sure the navigation works between pages, dark mode looks right (try changing your system to dark mode), and the content reads correctly. The 404 page won't auto-fire on a local server but you can manually visit `http://localhost:8000/404.html` to preview it.
