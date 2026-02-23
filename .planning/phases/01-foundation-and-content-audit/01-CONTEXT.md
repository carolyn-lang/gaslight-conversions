# Phase 1: Foundation and Content Audit - Context

**Gathered:** 2026-02-23
**Status:** Ready for planning

<domain>
## Phase Boundary

Set up staging environment on Netlify and audit the existing gaslightconversions.com site — crawl all URLs, inventory product photos and copy quality, and produce a 301 redirect map. No new pages are built in this phase. The output is a complete picture of what exists and what redirects are needed before build work begins.

</domain>

<decisions>
## Implementation Decisions

### New URL Structure

- Product catalog: `/products`
- Individual product pages: `/products/[slug]` (e.g., `/products/low-voltage-kit`)
- Installation guides: one page per kit at `/install-guides/[slug]`
- Supporting content: flat top-level — `/about`, `/faq`, `/contact`, `/install-guides`
- Home page: `/` (not used as the catalog)

### Redirect Strategy

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

</decisions>

<specifics>
## Specific Ideas

- Old site URL pattern confirmed: flat `.html` files (e.g., `gaslightconversions.com/components.html`)
- Crawl target: `https://www.gaslightconversions.com` — full site, all linked pages

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 01-foundation-and-content-audit*
*Context gathered: 2026-02-23*
