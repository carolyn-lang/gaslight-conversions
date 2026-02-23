# Phase 1: Foundation and Content Audit - Research

**Researched:** 2026-02-23
**Domain:** Netlify site setup, web crawling/URL inventory, content audit, domain management
**Confidence:** HIGH

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

**New URL Structure**
- Product catalog: `/products`
- Individual product pages: `/products/[slug]` (e.g., `/products/low-voltage-kit`)
- Installation guides: one page per kit at `/install-guides/[slug]`
- Supporting content: flat top-level — `/about`, `/faq`, `/contact`, `/install-guides`
- Home page: `/` (not used as the catalog)

**Redirect Strategy**
- Old site uses flat `.html` URLs (confirmed: e.g., `/components.html`)
- Full crawl in this phase will discover all old URLs
- Redirects configured in Netlify `_redirects` file, go live at DNS cutover (not before)
- Mail Order page (`/mail-order.html` or similar) → `/products`
- HTML Sitemap page → `/` (home) or simply drop it
- Product pages: old `.html` slugs → new `/products/[slug]` equivalents
- Guides, FAQ, Contact: old `.html` slugs → new flat equivalents

### Claude's Discretion
- Exact Netlify project/team setup steps
- Whether to use the Netlify CLI or web UI for initial staging setup
- Content audit doc format (spreadsheet, Markdown table, etc.)

### Deferred Ideas (OUT OF SCOPE)
None — discussion stayed within phase scope.
</user_constraints>

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|-----------------|
| MIGR-01 | Old site URLs are crawled and inventoried before any build work begins | Screaming Frog free tier (≤500 URLs) handles site; site has ~10 pages total — well within limit. CSV export built-in. |
| MIGR-02 | All old site URLs have 301 redirects configured to new URL structure before DNS cutover | Netlify `_redirects` file syntax documented; redirect table can be drafted in this phase and placed in file during Phase 2 scaffold. |
| MIGR-03 | Existing product photos and copy are audited for quality before design begins | Content audit Markdown table or spreadsheet with usable/needs-update/must-replace status for each asset. |
</phase_requirements>

---

## Summary

Phase 1 is a discovery and setup phase — no code is written, no pages are built. The work falls into three parallel tracks: (1) spin up a Netlify staging site connected to the GitHub repo so CI/CD is live from day one, (2) crawl and fully inventory every URL on the current gaslightconversions.com site, and (3) audit every product photo and written content block for quality.

The current site is small: exactly 10 pages, all flat `.html` files. Screaming Frog's free tier (500-URL limit) more than covers it. The full redirect map can be drafted by hand from the crawl export — there is no ambiguity about scale here. Netlify setup is straightforward via either the web UI or CLI; the recommended path for a first-time setup is the web UI because it handles GitHub OAuth and deploy key installation automatically. The Netlify CLI is better for day-to-day use after the site exists.

Content audit output does not need to be a formal spreadsheet. A Markdown table in a `.planning/` document is sufficient given the small site size, and it stays in version control alongside the planning documents. Domain status should be checked via ICANN Lookup (the authoritative free tool) and documented in the same session.

**Primary recommendation:** Set up Netlify via web UI (import from GitHub repo), crawl gaslightconversions.com with Screaming Frog free, document redirect map and content audit status in a single Markdown file in `.planning/phases/01-foundation-and-content-audit/`.

---

## Known Site Inventory (Pre-Crawl)

Fetched directly from gaslightconversions.com during research. Site has exactly 10 pages:

| Old URL | Page Purpose | Confirmed New URL |
|---------|-------------|-------------------|
| `/index.html` (or `/`) | Home | `/` |
| `/kits.html` | Conversion kits catalog | `/products` (or individual `/products/[slug]`) |
| `/bulbs.html` | Replacement bulbs | `/products` (or `/products/replacement-bulbs`) |
| `/components.html` | Gaslight components | `/products` (or `/products/components`) |
| `/mail_order.html` | Mail order form | `/products` (dropping form — replacing with PayPal) |
| `/benefits.html` | Benefits of conversion | TBD — v2 or drop (out of scope for v1) |
| `/guide.html` | Installation guides | `/install-guides` |
| `/faq.html` | FAQ | `/faq` |
| `/sitemap.html` | HTML sitemap nav page | `/` (drop — will have XML sitemap instead) |
| `/contact_us.html` | Contact form | `/contact` |

**Note:** The crawl in this phase will confirm all URLs including any not linked from the sitemap (image files, CSS, JS, etc.). The table above covers the navigable HTML pages.

---

## Standard Stack

This phase is tooling-only. No npm packages are installed. The deliverables are: a Netlify site, a crawl export file, a Markdown audit document.

### Core Tools

| Tool | Version | Purpose | Why Standard |
|------|---------|---------|-------------|
| Netlify Web UI | N/A | Create site, connect GitHub repo, get staging URL | Official first-time setup path; handles OAuth, deploy keys, webhook automatically |
| Screaming Frog SEO Spider | Free tier | Crawl all URLs, export CSV | Industry-standard crawler; free tier covers 500 URLs (site has 10); exports to CSV |
| ICANN Lookup | N/A (web tool) | Check domain registration and expiry | Official authoritative source for domain registry data |

### Supporting Tools

| Tool | Version | Purpose | When to Use |
|------|---------|---------|-------------|
| Netlify CLI (`netlify-cli`) | Latest | Link local repo to Netlify site after initial setup | Use after web UI creates the site; needed for local `netlify dev` in later phases |
| LibreCrawl | Latest (open-source) | Free unlimited crawl alternative | Use if Screaming Frog download is blocked or unavailable |
| Google Search Console | N/A | Secondary URL inventory check | Use to cross-reference — may surface indexed URLs not in navigation |

### Alternatives Considered

| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| Screaming Frog (free) | `wget --spider` or `curl` scripts | Screaming Frog exports clean CSV; command-line approach requires post-processing |
| Netlify Web UI (first setup) | Netlify CLI `netlify init` | CLI is fine but requires Node.js and a terminal; web UI is faster for one-time setup |
| ICANN Lookup | Registrar dashboard login | ICANN is always authoritative; registrar login requires credentials on hand |

**Installation (for later phases — not needed in Phase 1):**
```bash
npm install -g netlify-cli
```

---

## Architecture Patterns

### Deliverables Structure

```
.planning/phases/01-foundation-and-content-audit/
├── 01-CONTEXT.md          # Already exists
├── 01-RESEARCH.md         # This file
├── 01-PLAN.md             # Created by planner
└── AUDIT.md               # Created during execution: URL inventory + content audit + domain status

public/
└── _redirects             # Created during execution: 301 redirect rules (not active until DNS cutover)
```

### Pattern 1: Netlify Site Setup via Web UI

**What:** Connect GitHub repo to Netlify through the web UI to create a staging site with a `*.netlify.app` URL.
**When to use:** First-time site creation — handles GitHub OAuth and deploy key in one flow.

**Steps:**
1. Go to app.netlify.com → "Add new project" → "Import an existing project"
2. Connect GitHub, select the Gaslight repo
3. Netlify auto-detects Astro; confirm build command (`astro build`) and publish directory (`dist`)
4. Click "Deploy site"
5. Customize the site name (e.g., `gaslight-conversions`) to get `https://gaslight-conversions.netlify.app`

**Note on timing:** The repo must exist on GitHub before this step. If the repo doesn't exist yet, create it first (empty is fine).

### Pattern 2: Netlify `_redirects` File

**What:** A plain text file in the publish directory that tells Netlify how to redirect old URLs.
**When to use:** All permanent (301) redirects for old `.html` URLs to new slug-based URLs.

```
# Source: https://docs.netlify.com/manage/routing/redirects/overview/
# Place at: public/_redirects (Astro copies public/ contents to dist/)

# Product pages
/kits.html            /products           301
/bulbs.html           /products           301
/components.html      /products           301
/mail_order.html      /products           301

# Content pages
/guide.html           /install-guides     301
/faq.html             /faq                301
/contact_us.html      /contact            301

# Drop pages
/sitemap.html         /                   301
/benefits.html        /                   301

# Home (belt-and-suspenders)
/index.html           /                   301
```

**Rules for `_redirects`:**
- One rule per line: `[old-path]  [new-path]  [status-code]`
- Default status code is `301` (permanent) — include it explicitly for clarity
- Rules are processed top-to-bottom; first match wins
- File lives in the Astro `public/` directory so it gets copied to `dist/` on build
- Paths are case-sensitive

### Pattern 3: Content Audit Markdown Table

**What:** A Markdown table documenting each page's content quality.
**When to use:** When site is small enough that a spreadsheet is overkill — 10 pages fits cleanly in a table.

```markdown
## Content Audit

| Page | URL | Photos | Copy | Status | Notes |
|------|-----|--------|------|--------|-------|
| Home | /index.html | n/a | Medium | needs-update | Generic intro, no clear value prop |
| Kits | /kits.html | 3 photos | Good | usable | Photos are small, may need reshot |
| ... | ... | ... | ... | ... | ... |

**Status values:** `usable` | `needs-update` | `must-replace`
```

### Anti-Patterns to Avoid

- **Setting up DNS/domain transfer in Phase 1:** The `_redirects` file is drafted in this phase but goes live at DNS cutover (Phase 5 or later). Do not touch DNS settings yet.
- **Creating the Netlify site without connecting to GitHub:** If you drag-and-drop files instead of connecting to git, you lose automatic deploy previews for future work.
- **Writing redirect rules before the full crawl is complete:** The crawl may discover URLs not in the navigation (old product detail pages, image paths). Write the `_redirects` file after the crawl, not before.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| URL crawling | Custom wget/curl script | Screaming Frog (free) | Handles JS rendering, follows redirects correctly, exports clean CSV with status codes |
| Domain expiry check | Manual registrar research | ICANN Lookup (`lookup.icann.org`) | Authoritative registry data; always current |
| 301 redirect rules | Custom server middleware | Netlify `_redirects` file | Built into Netlify's CDN layer; zero config beyond the text file |

**Key insight:** This phase is entirely process and tooling. The outputs are documents and one config file. Nothing should be built from scratch.

---

## Common Pitfalls

### Pitfall 1: Incomplete Crawl Misses Non-Linked Pages

**What goes wrong:** The crawl only visits pages reachable from the homepage. Old product subpages or image files with direct URLs are missed and don't get redirects.
**Why it happens:** Old sites often have pages that exist but aren't linked from the main navigation.
**How to avoid:** In Screaming Frog, also check the "Images" and "External Links" tabs. Use Google Search Console (if available) to cross-reference indexed URLs.
**Warning signs:** After launch, 404 errors in Netlify analytics for `.html` URLs you didn't redirect.

### Pitfall 2: Redirect File in Wrong Location

**What goes wrong:** `_redirects` file gets placed in the project root, not in `public/`. Astro does not copy root files to `dist/` — so the file never reaches Netlify.
**Why it happens:** Astro's build output directory is `dist/`. Netlify only reads `_redirects` from the publish directory.
**How to avoid:** Place `_redirects` in `public/` — Astro copies all `public/` contents to `dist/` verbatim.
**Warning signs:** Redirects defined in the file but 404s still returned on old URLs after deploy.

### Pitfall 3: Netlify Site Created Without GitHub Connection

**What goes wrong:** Site is drag-and-dropped rather than connected to the Git repo. Future code pushes don't auto-deploy.
**Why it happens:** Drag-and-drop is the fastest path shown on the Netlify homepage.
**How to avoid:** Always use "Import an existing project" → select GitHub repo for the initial setup.
**Warning signs:** No "Deploys" tab auto-updating when you push to `main`.

### Pitfall 4: Domain Expiry Not Documented

**What goes wrong:** The domain expires during the multi-phase rebuild. The new site never goes live and the old site goes down.
**Why it happens:** Long-running projects lose track of renewal dates.
**How to avoid:** Look up `gaslightconversions.com` on ICANN Lookup (`lookup.icann.org`), record the "Registry Expiry Date" and the registrar name in the audit document.
**Warning signs:** If the expiry is within 60 days of expected project completion, renew immediately.

### Pitfall 5: Redirect Drafted for `/benefits.html` Without a Destination

**What goes wrong:** `/benefits.html` has no clear v1 equivalent — it's a deferred-to-v2 page. A redirect to `/` is acceptable, but silently dropping it without a rule causes a 404.
**Why it happens:** The new URL structure doesn't include a `/benefits` route in v1.
**How to avoid:** Redirect `/benefits.html` → `/` (home) as a safe fallback. Document the decision in the audit.

---

## Code Examples

### Netlify `_redirects` File (Complete Draft)

```
# Source: https://docs.netlify.com/manage/routing/redirects/overview/
# File location: public/_redirects
# Goes live at DNS cutover only

# --- Product pages (all map to catalog until individual pages exist) ---
/kits.html            /products           301
/bulbs.html           /products           301
/components.html      /products           301
/mail_order.html      /products           301

# --- Content pages ---
/guide.html           /install-guides     301
/faq.html             /faq                301
/contact_us.html      /contact            301

# --- Drop pages ---
/sitemap.html         /                   301
/benefits.html        /                   301

# --- Home (belt-and-suspenders) ---
/index.html           /                   301
```

### ICANN WHOIS Lookup

Visit: `https://lookup.icann.org/en/lookup`
Enter: `gaslightconversions.com`
Record:
- Registry Expiry Date
- Registrar name
- Registrar IANA ID
- Name servers (will be needed for DNS cutover in a later phase)

### Screaming Frog Export Steps

1. Download and install Screaming Frog SEO Spider (free tier, no license required for ≤500 URLs)
2. Enter `https://www.gaslightconversions.com` in the URL field
3. Click Start
4. When crawl completes, go to: Bulk Export → All Export → All Inlinks
5. Also export: Reports → Crawl Overview
6. Save CSV to `.planning/phases/01-foundation-and-content-audit/crawl-export.csv`

---

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| `netlify.toml` `[[redirects]]` blocks | `public/_redirects` flat file | Both still supported | Flat file is simpler for a short list; TOML is better for complex conditions |
| Screaming Frog as paid-only tool | Free tier for ≤500 URLs | Long-standing | Small sites (like this one) can use it without paying |
| Manual WHOIS via registrar | ICANN Lookup (lookup.icann.org) | Stable | ICANN is always authoritative; registrar UIs vary |

**Note:** Both `_redirects` and `netlify.toml` are current and supported. For this project's simple 10-URL redirect table, `_redirects` is preferred for readability.

---

## Open Questions

1. **Does gaslightconversions.com have product sub-pages not linked from navigation?**
   - What we know: Navigation has 10 pages. Old e-commerce sites sometimes have direct-link product pages.
   - What's unclear: Whether there are additional `.html` files accessible only via direct URL or old Google index entries.
   - Recommendation: The crawl will answer this. Also check Google Search Console if Carolyn has access, or run a `site:gaslightconversions.com` query in Google to see indexed pages.

2. **Who is the domain registrar?**
   - What we know: Domain is `gaslightconversions.com`. Registrar unknown until WHOIS lookup.
   - What's unclear: Whether auto-renewal is enabled, when it expires.
   - Recommendation: First task of this phase — check ICANN Lookup and document result.

3. **Quality of existing product photos**
   - What we know: Photos exist on `/kits.html`, `/bulbs.html`, `/components.html`. Resolution unknown.
   - What's unclear: Whether photos are high enough resolution for a modern responsive design, or whether new photography is needed before Phase 3.
   - Recommendation: Content audit task will answer this. The MIGR-03 requirement gates Phase 3 design on this answer.

4. **Does the GitHub repo exist yet?**
   - What we know: Project is planned on Astro 5.x + Tailwind 4.x + Netlify.
   - What's unclear: Whether a GitHub repo has been created for the project.
   - Recommendation: Check for existing repo before scheduling Netlify setup task. If no repo exists, repo creation is a prerequisite task.

---

## Sources

### Primary (HIGH confidence)
- [Netlify Docs: Redirects overview](https://docs.netlify.com/manage/routing/redirects/overview/) — `_redirects` syntax, file placement, processing order
- [Netlify Docs: Add new site](https://docs.netlify.com/welcome/add-new-site/) — three setup pathways, URL format, site naming
- [Netlify Docs: Create deploys](https://docs.netlify.com/deploy/create-deploys/) — CLI vs web UI options
- [gaslightconversions.com](https://www.gaslightconversions.com) — live site fetched directly; 10 pages confirmed
- [gaslightconversions.com/sitemap.html](https://www.gaslightconversions.com/sitemap.html) — complete URL list confirmed

### Secondary (MEDIUM confidence)
- [Screaming Frog FAQ](https://www.screamingfrog.co.uk/seo-spider/faq/) — free tier 500 URL limit confirmed; CSV export available in free tier
- [ICANN Lookup](https://lookup.icann.org/) — authoritative domain registry lookup tool
- [Netlify CLI docs](https://docs.netlify.com/api-and-cli-guides/cli-guides/get-started-with-cli/) — `netlify init` and `netlify link` commands

### Tertiary (LOW confidence)
- [LibreCrawl](https://librecrawl.com/) — open-source alternative to Screaming Frog; unverified feature completeness

---

## Metadata

**Confidence breakdown:**
- Known site inventory: HIGH — fetched live from gaslightconversions.com during research
- Netlify setup: HIGH — verified against official Netlify docs
- Redirect syntax: HIGH — verified against official Netlify docs
- Crawl tooling: HIGH — Screaming Frog free tier confirmed for small sites
- Content audit format: HIGH — Markdown table is appropriate for 10-page site
- Domain check: HIGH — ICANN Lookup is the authoritative source

**Research date:** 2026-02-23
**Valid until:** 2026-03-25 (stable domain; Netlify docs rarely change for core features)
