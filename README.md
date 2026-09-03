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
| `AREA` / `AREA_ORDER` | The drawer tabs. A `cro` key exists but no brand has `cro` data yet, so the CRO tab is not rendered anywhere. Add `cro:[["Task title","What you did"],…]` to a brand's `s` object and the tab appears for that brand automatically. |
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
- `index.html` is around 450 KB, mostly the embedded screenshots and the CV PDF.
  It gzips to roughly 264 KB over the wire. That is fine for GitHub Pages, which
  has a 1 GB repo limit and a 100 MB per-file limit.
- The CV is embedded as a base64 `data:` URI, so it ships with the page and needs
  no separate request. To replace it: run the new PDF through
  `qpdf --object-streams=generate --recompress-flate --compression-level=9 --linearize`
  first (it took the current one from 136 KB to 99 KB with no visible change), then
  swap the base64 payload in the `$("#cvBtn").href` assignment.


---

## QA fixes applied

### Round 1 — pre-deploy (1 Sep 2026)

| Fix | What it was |
|---|---|
| `.blobs{overflow:hidden}` | Decorative blur circles sat outside their sections, so every page had a horizontal scrollbar — 40px on desktop, 339px on a 375px phone. |
| Desktop nav breakpoint 768px → 1024px | The seven-item nav switched on at 768px but needs ~990px, so the header ran off the screen on iPad portrait. |
| `.cta-row .btn` max-width + small-screen sizing | The `johnmayuga.ph@gmail.com` button was 418px wide and broke layout on any phone under 400px. |
| Removed duplicate `<meta name="description">` | Two competing descriptions; search engines pick one at random. |
| CV download shim | Native `<a download>` is used here; the shim only activates inside the claude.ai artifact viewer, which blocks page-initiated downloads. |

### Round 2 — audited against the live site (1 Sep 2026)

| Fix | What it was |
|---|---|
| `--s-500` `#64748B` → `#7A8A9F` | **70 text elements failed WCAG AA.** 69 of them used this one token: 3.83:1 against the card background, needs 4.5:1. Now 5.18:1. |
| Footer copyright `--s-600` → `--s-500` | The remaining failure — 2.41:1, the worst on the page. |
| `og:image` + `twitter:image` + `og.png` | `twitter:card` claimed `summary_large_image` with no image, so every LinkedIn/Slack/X share rendered as a bare text card. `og.png` is a 1200×630 card built in the site's own type and palette. |
| `referrerpolicy="no-referrer"` on 143 logo images | Brand logos come from Google's favicon service — 77 requests that were handing Google the referring URL of every visitor. The logos still load; the referrer no longer leaks. |
| Sticky bar now reveals on scroll | It was permanently on, sitting on top of the hero's "See the work" button on phones. Now it appears once the hero scrolls away and hides again over the contact section. Implemented as a plain passive scroll listener that also re-syncs on `visibilitychange`, `pageshow`, `load` and `resize`. Note for future debugging: this was rewritten twice chasing bugs that did not exist. Programmatic `window.scrollTo()` from a browser-extension context does not fire scroll events on the page, so any automated test that scrolls that way will report the bar as broken when it is not. Verify with real wheel input, or by dispatching a `scroll` event explicitly. |
| Skip-to-content link | Keyboard and screen-reader users had to tab through the whole nav on every visit. |
| `aria-hidden="true"` on 79 decorative SVGs | Screen readers were announcing every icon as an unlabelled graphic. |
| JSON-LD `Person` schema | Gives Google structured facts — name, role, location, skills, GitHub — instead of guessing from prose. |

| `text-wrap: balance` on headings | Long headings stranded a word or two on their own last line — "Nine things I actually work / on." On phones this hit 7–9 headings per width. Balancing evens the lines out; body copy gets `text-wrap: pretty` for the same reason. Orphan count at 360–414px went from 7–9 to zero. |

| Hero H1 sized to two lines | "Shopify storefronts engineered for conversion," was wrapping to two lines on its own, making a three-line headline. The H1 now breaks out of the 1152px body column (`width:min(1560px,96vw)`, centred with a transform) and is sized `clamp(34px,4.1vw,68px)` so that first clause holds one line from 834px up. Below that it wraps naturally. Verified two lines at 834–2560px, no overflow at any of 20 widths. |

| Social descriptions say "8+ years" | `og:description` and `twitter:description` still said "Eight years" after the hero changed, so link previews understated the page. |
| Closed drawer removed from the tab order | The brand drawer sits off-screen at `translateX(103%)` when closed. `aria-hidden` hides it from screen readers but does **not** remove it from the tab order, so a keyboard user could tab into invisible controls. Now `visibility:hidden` when closed, restored on open with a zero-duration transition so the slide animation is unaffected. |
| Drawer given an accessible name | It had `role="dialog"` with nothing to announce. Now `aria-label="Engagement record"`. |
| Tap targets, header and footer nav | Nine footer links and ten header nav links were 16px tall, under the 24px minimum in WCAG 2.2 SC 2.5.8. Both now `min-height:24px`; the footer row gap was reduced so spacing is unchanged, and the header links were already centred in a 65px bar so nothing moved visually. Worth noting for future audits: the header set was missed on the first pass because the audit ran only at 390px, where the desktop nav is hidden. Tap targets are now checked at 13 widths from 320 to 1920px. |

| Mobile burger menu + flyout | Below 1024px there was no navigation at all — only the logo and Contact button, so the ten section links were unreachable without scrolling the whole page. Added a 44x44 burger opening a right-hand flyout that matches the brand drawer's motion. Closed state is `visibility:hidden` so it stays out of the tab order; focus moves in on open and returns to the burger on close; dismisses on Esc, scrim click, link click, and on resize past the desktop breakpoint so the page can never be left scroll-locked. Below 400px the header's Contact button is hidden — logo, Contact and burger cannot share a 320px bar without clipping, and the flyout carries "Get in touch" anyway. |

**Verified after the changes:** 0 of 555 text elements below WCAG AA (lowest ratio now 5.15:1),
no horizontal scroll at 320 / 360 / 375 / 390 / 414 / 480 / 540 / 640 / 700 / 768 / 834 / 900 /
1024 / 1180 / 1280 / 1440 / 1600 / 1920 px, 44 brand cards and all interactions intact,
no JavaScript errors.

**Live performance** (measured on GitHub Pages): 396 KB decoded / 218 KB over the wire,
TTFB 290 ms, DOMContentLoaded 543 ms, fully loaded 794 ms.

### Still open (deliberately not changed)

- **143 logo requests still go to Google.** `no-referrer` stops the leak, but the third-party
  dependency remains: on a network that blocks Google, or if the service changes, every brand
  falls back to its initials. Embedding the logos as data URIs would remove the dependency
  entirely at a cost of roughly 100–150 KB.
- **19 tap targets are under 24px.** Small icon buttons. Below the 24×24 WCAG 2.2 minimum,
  though all of them sit next to a larger labelled control.
