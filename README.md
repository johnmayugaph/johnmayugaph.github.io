# John Paul Mayuga — Portfolio

Single-page portfolio. Static HTML, no framework, no build step, no dependencies.
Everything is in `index.html` — markup, CSS, JavaScript, the three feedback screenshots,
and the CV PDF, all embedded. Nothing loads from a server you control.

## Contents

| File | Purpose |
|---|---|
| `index.html` | The entire site |
| `404.html` | Not-found page, served automatically by GitHub Pages |
| `favicon.svg` | Browser tab icon |
| `robots.txt` | Search engine directives |
| `sitemap.xml` | Single-URL sitemap |
| `.nojekyll` | Stops GitHub Pages running Jekyll over the files |

Deploy **only these files**. Anything else you drop in the repo is publicly
downloadable by URL — don't add your CV, invoices or working documents.

---

## Deploy

Target address: **https://johnmayugaph.github.io/** — already written into
`index.html`, `robots.txt` and `sitemap.xml`.

### Step 0 — the account has to exist first

As of 1 Sep 2026 the username `johnmayugaph` was **not registered**. Nothing below
works until it is, and until then that URL returns GitHub's "There isn't a GitHub
Pages site here" 404.

- Signed-in browser currently: `johnmayugaph-bot` (a different account).
- `johnmayuga` (no `ph`) belongs to someone else — not available.

Register `johnmayugaph` at <https://github.com/signup>, or rename an existing
account under **Settings → Account → Change username**. Renaming is instant and
keeps your repos, but old links to the previous username break.

### Step 1 — create the repository

1. Signed in as `johnmayugaph`, go to <https://github.com/new>.
2. Repository name: **`johnmayugaph.github.io`** — exactly that, including the
   `.github.io`. It must equal the account name, or GitHub publishes to
   `/repo-name/` instead of the root and the canonical URL in `index.html`
   points at a 404.
3. Visibility: **Public**. GitHub Pages needs public on free accounts.
4. Leave "Add a README file" **unticked** — this bundle has its own.

### Step 2 — upload

1. On the empty repo page click **uploading an existing file**.
2. Unzip the bundle and drag in all **seven** files:
   `index.html`, `404.html`, `favicon.svg`, `robots.txt`, `sitemap.xml`,
   `README.md`, `.nojekyll`.
3. They must land at the top level, not inside a folder. If you drag the *folder*
   in, GitHub keeps the nesting and the site won't serve.
4. `.nojekyll` is a zero-byte hidden file. macOS Finder hides it until you press
   <kbd>Cmd</kbd>+<kbd>Shift</kbd>+<kbd>.</kbd>; Windows Explorer needs
   *View → Show → Hidden items*. Without it GitHub runs Jekyll over the site.
5. Commit to `main`.

### Step 3 — turn Pages on

**Settings → Pages** → *Build and deployment* → Source: **Deploy from a branch**,
branch `main`, folder `/ (root)` → **Save**.

### Step 4 — check it

Wait one to three minutes, then open <https://johnmayugaph.github.io/>.

If it still 404s:

| Symptom | Cause |
|---|---|
| "There isn't a GitHub Pages site here" | Pages never enabled (Step 3), or repo is private, or the account doesn't exist yet. |
| Site loads at `/johnmayugaph.github.io/` | You nested the files in a folder. Move them to the root. |
| Raw markdown or a broken layout | `.nojekyll` missing. |
| Nothing at all after 5 min | Check the repo's **Actions** tab — `pages-build-deployment` shows whether the publish ran and why it failed. |

Browsers cache 404s hard. Test in a private window, or hard-reload with
<kbd>Cmd/Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>R</kbd>.

---

---

## Updating the site

Edit `index.html` in GitHub's web editor (pencil icon) and commit, or upload a
replacement file. GitHub Pages redeploys within a minute or two to the same URL.

## Adding a custom domain later

Buy the domain, then in **Settings → Pages** enter it under *Custom domain*.
GitHub writes a `CNAME` file into the repo and issues an HTTPS certificate once
your DNS resolves. Point an `A` record at GitHub's IPs, or a `CNAME` record at
`johnmayugaph.github.io`. The `.github.io` address keeps working alongside it.

---

## Editing the content

Everything is data-driven near the top of the `<script>` block in `index.html`:

| Variable | Controls |
|---|---|
| `BRANDS` | The 44 brand records |
| `AGENCY` | Engagement tier, status and role per brand |
| `DOMAINS` | Brand name to domain — adding one makes that brand's real logo appear |
| `JOURNEY` | The resume timeline, including the Read more duties |
| `EDUCATION` | The education card |
| `FEEDBACK` | The three shout-out cards and their screenshots |
| `CONNECTIONS` | The platform stack grid |
| `PROOF` | The four proof cards |
| `FAQ` | The accordion |

### Accent colour

Three numbers at the top of the stylesheet drive the violet used across the page:

```css
--v:#7C3AED;    /* primary  */
--v-400:#A78BFA; /* light    */
--c-400:#22D3EE; /* gradient */
```

## Notes

- Brand logos are fetched at page load from Google's favicon service using the
  domains in `DOMAINS`. Brands without a verified domain fall back to their
  initials. The page needs an internet connection for logos to render.
- The contact form and buttons are `mailto:` links. There is no server, so
  nothing is stored or sent in the background.
- The file is around 395 KB, mostly the embedded screenshots and CV. That is fine
  for GitHub Pages, which has a 1 GB repo limit and a 100 MB per-file limit.


---

## QA fixes applied (1 Sep 2026)

| Fix | What it was |
|---|---|
| `.blobs{overflow:hidden}` | Decorative blur circles sat outside their sections, so every page had a horizontal scrollbar — 40px on desktop, 339px on a 375px phone. |
| Desktop nav breakpoint 768px → 1024px | The seven-item nav switched on at 768px but needs ~990px, so the header ran off the screen on iPad portrait. |
| `.cta-row .btn` max-width + small-screen sizing | The `johnmayuga.ph@gmail.com` button was 418px wide and broke layout on any phone under 400px. |
| Removed duplicate `<meta name="description">` | Two competing descriptions; search engines pick one at random. |
| CV download shim | Native `<a download>` still used here on GitHub Pages; the shim only activates inside the claude.ai artifact viewer, which blocks page-initiated downloads. |

Verified clean at 320 / 360 / 375 / 390 / 414 / 480 / 540 / 640 / 700 / 768 / 834 / 900 /
1024 / 1180 / 1280 / 1440 / 1600 / 1920 px. No JavaScript errors.

### Still open (not fixed — your call)

- **No `og:image`.** `twitter:card` is set to `summary_large_image` but there's no image
  to show, so LinkedIn/Slack/X previews render as a bare text card. Add a 1200×630 PNG
  as `og.png`, then add `<meta property="og:image" content="https://johnmayugaph.github.io/og.png">`
  and the matching `twitter:image`.
- **Brand logos need the open internet.** They come from Google's favicon service. Where
  that's blocked (corporate networks, the claude.ai artifact viewer) every brand falls back
  to its initials. The fallback is clean — this is a note, not a defect.
- **The sticky bottom bar is always on.** On a phone it covers the hero's "See the work"
  button. Consider revealing it only after the hero scrolls past.
- **No skip-to-content link**, and 79 decorative SVGs lack `aria-hidden="true"`. Minor
  accessibility polish; everything else (landmarks, heading order, focus rings,
  `aria-expanded`, `role="dialog"`, reduced-motion) already checks out.
