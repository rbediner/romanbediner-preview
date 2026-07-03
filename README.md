# romanbediner-preview

Staging preview deployment of the [romanbediner.com](https://romanbediner.com) website.

## Architecture

This repository holds the **published static build** of the Roman Bediner site,
served on the `staging-preview` branch as a preview mirror of the production
`romanbediner.com` site. It is a deploy artifact, not the site source: commits
are automated snapshots titled `Publish preview for staging@<sha>`, where the sha
identifies the source commit that produced the build.

- **Tech stack:** Plain static HTML, CSS, and vanilla JavaScript — no build
  framework or bundler in the published output. Hosted on **GitHub Pages** under
  the base path `/romanbediner-preview/` (`.nojekyll` disables Jekyll
  processing). `robots.txt` disallows all crawlers so the preview stays
  unindexed.
- **Directory layout:**
  - `index.html`, `404.html` — homepage and error page.
  - Per-page folders each containing an `index.html`: `about/`, `services/`,
    `connect/`, `insights/`, `resources/` (with sub-pages such as
    `agentic-ai-employees/`, `pasteflow/`, dashboard/framework summaries), and
    `framework/` (stage sub-pages: `signals/`, `opportunity/`, `design/`,
    `execution/`, `integration/`, `evolution/`), plus the
    `ai-enabled-operations-dashboard/` demo.
  - `styles/` — per-page stylesheets (`home.css`, `about.css`, `framework.css`,
    etc.) plus shared `site.css`.
  - `scripts/runtime/` — page-scoped vanilla-JS modules (shared
    `site-navigation.js`, `section-nav.js`, `ga4-bootstrap.js`, contact form via
    EmailJS, framework/resources analytics and carousels, dashboard modals, fleet
    diagram zoom).
  - `assets/` — favicons, logos, images, OG cards, and icon sets.
  - `sitemap.xml`, `robots.txt` — SEO/crawler metadata pointing at the canonical
    `romanbediner.com` URLs.
- **Security/CSP:** Pages ship a strict Content-Security-Policy that forbids
  inline scripts; all behavior lives in external `scripts/runtime/*.js` files.
- **Build / preview / deploy:** The site is built and published elsewhere; this
  branch receives the resulting static files. Preview it by opening `index.html`
  locally or via any static file server (asset links assume the
  `/romanbediner-preview/` base path). Pushing to `staging-preview` publishes it
  to GitHub Pages.

## Google Drive drift

This repository is checked out inside Google Drive and synced across machines. Google Drive creates conflict-copies (filenames ending in ` 2`, ` 3`, or ` (1)`) — including inside `.git` — which corrupt the repo. A guardrail auto-removes them:

- `scripts/clean-drive-drift.sh --fix` — remove conflict-copies then verify with `git fsck` (`--check` to only report).
- Runs automatically via git hooks (`pre-commit`, `post-merge`, `post-checkout`) and, for Claude, on session start via `.claude/settings.json`.
- Never commit a file whose name ends in ` 2`/` 3` — it is Google Drive junk, not a real file.
