# Roadmap: Gaslight Conversions

## Overview

This roadmap rebuilds gaslightconversions.com on a modern static stack (Astro + Tailwind + Netlify) while protecting years of accumulated SEO authority. The phases follow a strict dependency order: pre-build audit before design, design system before components, core commerce before supporting content, and a dedicated launch verification phase that treats SEO and payment verification as first-class deliverables rather than afterthoughts.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [ ] **Phase 1: Foundation and Content Audit** - Staging environment up, old site crawled, all existing content inventoried before design begins
- [ ] **Phase 2: Design System** - Mid-century design tokens, base layout shell, and responsive grid that all subsequent pages inherit
- [ ] **Phase 3: Product Catalog and Commerce** - Complete purchase funnel — catalog, product detail pages, PayPal buy buttons, optimized images
- [ ] **Phase 4: Supporting Content Pages** - Installation guides, contact form, and all trust-building pages completing the site
- [ ] **Phase 5: SEO, Verification, and Launch** - 301 redirects live, metadata complete, PayPal live credentials verified, DNS cutover

## Phase Details

### Phase 1: Foundation and Content Audit
**Goal**: The project has a staging environment and a complete picture of what exists before any build work begins
**Depends on**: Nothing (first phase)
**Requirements**: MIGR-01, MIGR-02, MIGR-03
**Success Criteria** (what must be TRUE):
  1. A staging URL on Netlify is live and accessible — the new site can be built and previewed without touching the live domain
  2. Every URL on the current gaslightconversions.com site is inventoried in a crawl export, and a draft 301 redirect table maps old URLs to new URL structure
  3. Every product photo and written content block from the old site has been reviewed and marked as usable, needs-update, or must-be-replaced
  4. The domain registration status is confirmed and renewal date is documented (no surprise lapse during the rebuild)
**Plans**: 2 plans

Plans:
- [ ] 01-01-PLAN.md — Astro 5.x project scaffold + Netlify staging site connected to GitHub
- [ ] 01-02-PLAN.md — URL crawl inventory, content quality audit, and draft _redirects file

### Phase 2: Design System
**Goal**: A complete design foundation exists that every subsequent page and component can inherit without rework
**Depends on**: Phase 1
**Requirements**: DSNG-01, DSNG-02
**Success Criteria** (what must be TRUE):
  1. The site renders correctly and is fully usable at mobile, tablet, and desktop widths — no horizontal scroll, no overlapping elements, no unreadable text at any breakpoint
  2. The orange and navy mid-century palette, typographic scale, and spacing system are defined as design tokens and applied consistently across the base layout
  3. Every page shares a consistent nav and footer via BaseLayout.astro — no duplicated markup
  4. The visual direction reads as mid-century and warm on first impression, not as a generic modern Bootstrap site
**Plans**: TBD

### Phase 3: Product Catalog and Commerce
**Goal**: A customer can find every product, understand it, and buy it — entirely on the new site
**Depends on**: Phase 2
**Requirements**: PROD-01, PROD-02, PROD-03, DSNG-03
**Success Criteria** (what must be TRUE):
  1. A customer can view all products on a single catalog page — each product shows a photo, name, and price at a glance
  2. A customer can navigate to a dedicated product detail page showing full description, multiple photos, specs, and price
  3. A customer can initiate a PayPal purchase directly from the product detail page using an embedded buy button — the button uses a current PayPal API (not the deprecated webscr endpoint)
  4. Product images are visually consistent, high-quality, and load fast (WebP format, appropriately sized)
  5. The purchase flow is fully functional in PayPal sandbox — ready for live credential swap in Phase 5
**Plans**: TBD

### Phase 4: Supporting Content Pages
**Goal**: Every trust-building and support page exists, and a customer inquiry can reach the business owner
**Depends on**: Phase 3
**Requirements**: CONT-01, CONT-02
**Success Criteria** (what must be TRUE):
  1. A customer can access step-by-step installation instructions for each conversion kit without leaving the site
  2. A customer can submit an inquiry via a contact form and the message is delivered to the business owner's email via Netlify Forms — no backend code required
  3. The home page clearly communicates what Gaslight Conversions sells, who it's for, and how to buy
**Plans**: TBD

### Phase 5: SEO, Verification, and Launch
**Goal**: The new site is live, the old site's SEO authority is fully preserved, and real purchases can complete
**Depends on**: Phase 4
**Requirements**: DSNG-04
**Success Criteria** (what must be TRUE):
  1. Every URL from the Phase 1 crawl inventory returns a 301 redirect to the correct new URL — verified in production, not just locally
  2. Every page has a unique, descriptive title tag and meta description — no two pages share the same metadata
  3. An XML sitemap is generated and submitted to Google Search Console — property ownership verified
  4. At least one real test purchase completes end-to-end with live PayPal credentials (not sandbox) and money moves correctly
  5. The new site is live on gaslightconversions.com via DNS cutover — the old site is retired
**Plans**: TBD

## Progress

**Execution Order:**
Phases execute in numeric order: 1 → 2 → 3 → 4 → 5

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Foundation and Content Audit | 1/2 | In Progress|  |
| 2. Design System | 0/TBD | Not started | - |
| 3. Product Catalog and Commerce | 0/TBD | Not started | - |
| 4. Supporting Content Pages | 0/TBD | Not started | - |
| 5. SEO, Verification, and Launch | 0/TBD | Not started | - |
