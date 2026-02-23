# Project Research Summary

**Project:** Gaslight Conversions
**Domain:** Small niche e-commerce — specialty hardware / gas-to-electric outdoor lighting conversion kits
**Researched:** 2026-02-23
**Confidence:** MEDIUM (architecture HIGH; stack/features/pitfalls MEDIUM due to tool access restrictions during research)

## Executive Summary

Gaslight Conversions is a small family business selling under 10 SKUs of a proprietary gas-to-electric lamp conversion kit. The site rebuild is fundamentally a content and credibility project, not a technology challenge. The correct approach is a fully-static site built with Astro 5.x and Tailwind CSS 4.x, deployed to Netlify, with PayPal buy buttons per product page and no backend. This is a well-understood site class with established patterns — nothing here requires novel technology. The complexity ceiling is low, which means the risk of over-engineering is actually higher than the risk of under-building.

The recommended approach keeps complexity ruthlessly low: products live as Markdown files with typed frontmatter (Astro Content Collections), pages are generated statically at build time, the PayPal checkout flow happens entirely on PayPal's servers, and the contact form is handled by Netlify Forms — zero backend code needed anywhere. Adding a new product means adding one file. Changing the design means editing design tokens. This is the right shape for a 1-developer site with 10 products and a business owner who can edit code directly.

The key risks are not technical — they are operational. The old site has accumulated years of domain authority and indexed URLs. Careless DNS cutover, missing 301 redirects, or letting the domain lapse during the rebuild can cost months of search ranking recovery. Separately, the existing PayPal button code may use a deprecated API endpoint that could silently stop working. And content — especially product photos — must be inventoried and deemed usable before design work begins, not after. These three risks, if ignored, can each individually cause launch failure or post-launch revenue loss.

---

## Key Findings

### Recommended Stack

Astro 5.x is the right framework for this site. It is purpose-built for content-first, performance-critical, zero-JS-by-default static sites. Component reuse across 10 product pages, file-based routing, and Content Collections for typed product data give Carolyn a maintainable codebase without the overhead of React, Next.js, or a headless CMS. Tailwind CSS 4.x (CSS-first configuration, Oxide engine) is the right styling layer — it lets the mid-century orange/navy design system live in CSS variables and be applied consistently with utilities. Netlify is the preferred host because Netlify Forms eliminates the need to write any backend code for the contact form.

**Core technologies:**
- **Astro 5.x:** Static site framework — zero-JS-by-default, file-based routing, Content Collections, ideal for product catalog at this scale
- **Tailwind CSS 4.x:** Utility-first CSS with CSS-first config — define the palette once, use it everywhere; no fighting a component library's design opinions
- **Netlify:** Static hosting with CDN, free tier sufficient, Netlify Forms handles contact submission without a backend
- **PayPal hosted buttons (static form embed):** MVP path — dad's existing PayPal dashboard generates the code; no JS SDK integration needed for launch; migrate to Smart Payment Buttons in v2 if improved UX is desired
- **Fontsource (self-hosted fonts):** DM Serif Display or Playfair Display for headings + DM Sans for body — mid-century editorial character without Google CDN dependency

See `.planning/research/STACK.md` for full rationale and alternatives considered.

### Expected Features

The MVP purchase funnel is: product catalog → product detail page → PayPal checkout. Everything else serves to build trust and reduce friction before that moment. The current site's most critical failure is not being mobile-responsive — that is a baseline requirement for launch, not a nice-to-have.

**Must have (table stakes):**
- Product catalog page — customers need to see all products at a glance
- Individual product pages — photos, description, specs, price, PayPal buy button
- Mobile-responsive design — the current site's biggest failure; majority of browsing is mobile
- PayPal buy button per product — the defined checkout path
- Contact form — hardware buyers always have pre-purchase questions ("will this fit my lamp?")
- About / origin story page — family business with proprietary invention; trust is a purchase barrier
- FAQ page — reduces the "will this work for me?" friction before purchase
- HTTPS / SSL — required for any commerce; handled at the hosting level

**Should have (differentiators):**
- Installation guide / how-to content — reduces "can I do this myself?" anxiety; builds pre-purchase confidence
- Benefits / "Why Convert?" page — addresses the "why not replace the whole fixture?" objection
- Product compatibility / fit guide — answers the #1 pre-purchase question on-site, reduces support burden
- Testimonials / customer use-case photos — social proof for a niche product is disproportionately powerful
- Vintage mid-century brand aesthetic — design IS a differentiator; a generic Bootstrap site would undermine the product's identity
- Commercial customer callout — HOAs, municipalities, property managers have different needs; even acknowledging them builds credibility

**Defer (v2+):**
- Shopping cart / multi-item checkout — PayPal handles this natively; added complexity for minimal benefit at under 10 SKUs
- User accounts / login — no repeat-purchase logic that requires account state
- Live chat, newsletter, analytics dashboard, wholesale portal — complexity without proportional benefit at this scale

See `.planning/research/FEATURES.md` for full feature landscape and anti-features.

### Architecture Approach

The architecture is a fully-static site with no API layer, no backend, and no database. Products are authored as Markdown files with typed frontmatter (Astro Content Collections). Astro generates static HTML at build time — one page per product from a single `[slug].astro` template. PayPal checkout happens entirely on PayPal's servers. The contact form is handled by Netlify Forms. The entire deployed artifact is flat files on a CDN.

**Major components:**
1. **Design tokens / global CSS** — color system (orange, navy), typography, spacing; the foundation every other component depends on
2. **BaseLayout.astro** — shared nav, footer, `<head>` metadata shell; all pages consume it
3. **Content Collections (src/content/products/)** — one .md file per product; single source of truth for title, price, description, image, PayPal button ID
4. **Product catalog page + [slug].astro template** — Astro generates static pages at build time from product data; PayPalButton.astro encapsulates all PayPal embed code
5. **Supporting content pages** — Home, About, Why Convert, FAQ, Installation guides; layout-only dependencies
6. **Contact page** — HTML form + Netlify Forms; no backend required
7. **Netlify hosting + CDN** — git push deploys, HTTPS automatic, Netlify Forms active

**Key patterns to follow:**
- Content Collections with Zod schema validation for products — typed frontmatter, auto-generated slugs, build-time error catching
- Single PayPalButton.astro component — isolates all PayPal embed code; one file to update if PayPal's API changes
- Single BaseLayout.astro — consistent nav/footer/metadata without duplication

**Anti-patterns to avoid:**
- Hardcoding product data in page files (mixing data and presentation)
- One HTML file per product (the old site's problem — maintenance nightmare)
- Copying PayPal button HTML into multiple places instead of a shared component
- Building a backend for the contact form (Netlify Forms handles this for free)

See `.planning/research/ARCHITECTURE.md` for full component boundaries, data flow diagrams, and code examples.

### Critical Pitfalls

1. **Failing to redirect existing URLs before launch** — The old site has years of indexed URLs. Missing 301 redirects from old URL structure to new drops Google rankings that took years to accumulate. Mitigation: crawl the old site before touching anything; build a redirect table; implement via Netlify `_redirects`; verify before DNS cutover.

2. **Copying deprecated PayPal button code from the old site** — Old sites often use the legacy Payments Standard `webscr` form POST endpoint, which PayPal is deprecating. If copied forward, checkout could silently break post-launch. Mitigation: check which PayPal API the existing buttons use; regenerate from the current PayPal dashboard or JS SDK; test in sandbox before launch.

3. **DNS cutover without staging environment** — Doing development on the live domain risks domain authority, confuses Google, and creates no-rollback situations. Mitigation: set up staging URL on day one; keep old site live until new site is fully tested and verified; DNS cutover is the absolute last step.

4. **Starting design before content audit** — Building around placeholder content from a family business that may have low-resolution 2001-era product photos and no written copy is a launch blocker. Mitigation: inventory all existing content (photos, descriptions, pages) before design begins; identify every asset that needs to be remade and who produces it.

5. **PayPal sandbox credentials in production** — Sandbox and live PayPal environments look identical in code. Shipping with sandbox credentials means real purchases process but no money changes hands. Mitigation: establish clear environment configuration early; add "verify live PayPal credentials" as a hard launch checklist item; do one real test purchase before announcing the site is live.

See `.planning/research/PITFALLS.md` for the full pitfall catalog including moderate and minor pitfalls.

---

## Implications for Roadmap

Based on combined research, the architecture's build order and the pitfall risks together strongly suggest the following phase structure. The architecture research provides the dependency graph; the pitfalls research identifies which phases carry the most risk and what must happen before they begin.

### Phase 1: Project Foundation and Content Audit

**Rationale:** The two most critical pre-work items — establishing a staging environment and auditing existing content — must happen before any design or build work begins. These are blocking dependencies identified in pitfalls research, not optional tasks. Domain health check prevents SEO catastrophe. Content audit prevents design work being done around placeholder content.

**Delivers:** Staging URL active on Netlify; domain registration verified and renewed if needed; complete inventory of existing pages, product photos, and copy with quality assessment; list of content that must be created before launch; old site crawled and URL export in hand; redirect table drafted.

**Addresses:** Pitfall 1 (URL redirect planning), Pitfall 3 (staging environment), Pitfall 4 (content audit before design)

**Research flag:** Standard patterns — no deep research needed. Infrastructure setup and content auditing are straightforward tasks.

---

### Phase 2: Design System and Layout Foundation

**Rationale:** Design tokens are the dependency for every subsequent component. The architecture research is explicit: design tokens first, then layout shell, then pages. Building product cards before defining the color system leads to rework. This phase also locks in the mid-century brand aesthetic that is identified as a differentiator — design is not decoration here, it is product identity.

**Delivers:** Tailwind CSS 4.x configured with orange/navy palette and typographic scale; Fontsource fonts integrated (DM Serif Display + DM Sans); global.css with all design tokens; BaseLayout.astro with Nav, Footer, SiteHeader; responsive grid system; visual design validated against mid-century aesthetic intent.

**Addresses:** FEATURES.md differentiator (vintage mid-century brand aesthetic), ARCHITECTURE.md pattern (design tokens before all components), Pitfall 5 (mobile layout — establish responsive system from day one)

**Research flag:** Standard patterns — Astro + Tailwind 4.x integration is well-documented. Verify current `@astrojs/tailwind` integration approach before starting (STACK.md noted this as a version-check item).

---

### Phase 3: Product Data and Core Commerce Pages

**Rationale:** This is the site's primary purpose — the product catalog and individual product pages are the purchase funnel. They are also the most architecturally complex: Content Collections schema, dynamic routing, PayPal button integration, image optimization, and the mobile buy button experience all land here. This phase must be completed and thoroughly tested before supporting content pages are built, because it validates the core architecture.

**Delivers:** Content Collections schema and config; all product .md files authored with final copy and verified photos; catalog page (product grid); product detail page template ([slug].astro); PayPalButton.astro component using current PayPal embed approach (not legacy `webscr` if deprecated); product images optimized (WebP, appropriate dimensions); PayPal sandbox testing completed; mobile buy button experience verified on real devices.

**Addresses:** FEATURES.md table stakes (product catalog, product pages, PayPal buy button, high-quality photos, pricing visible), Pitfall 2 (PayPal deprecated API), Pitfall 8 (sandbox vs. live credentials), Pitfall 10 (hardcoded prices in multiple places), Pitfall 11 (image optimization)

**Research flag:** Needs verification before starting — verify PayPal's current recommended embed method at developer.paypal.com before implementing. STACK.md and PITFALLS.md both flag this as a high-risk area where PayPal has changed approaches multiple times.

---

### Phase 4: Supporting Content Pages

**Rationale:** Trust-building content (About, Why Convert, FAQ, Installation guides) is high-value and low-complexity. These pages depend only on the layout shell and product link targets established in Phases 2–3. Grouping them together makes sense because they share the same development pattern and the same content authorship workflow.

**Delivers:** Home page (hero, product teaser, value prop, CTA); About page (origin story, family business narrative); Benefits / Why Convert page; FAQ page (homeowner and commercial audiences); Installation guides page(s); Contact page with Netlify Forms integration and verified email delivery.

**Addresses:** FEATURES.md table stakes (About page, FAQ, contact form), FEATURES.md differentiators (benefits page, installation guides, commercial customer acknowledgment), Pitfall 7 (contact form email verified)

**Research flag:** Standard patterns — static content pages with Astro are well-documented. Netlify Forms integration is zero-config. No research needed.

---

### Phase 5: SEO, Polish, and Pre-Launch Verification

**Rationale:** SEO metadata, redirect implementation, mobile QA, and launch verification are distinct from build tasks but are launch blockers. Grouping them as a final phase ensures they are treated as deliverables, not afterthoughts. The pitfalls research identifies three SEO risks (URL redirects, page title reset, Search Console) that are individually capable of causing significant post-launch damage if skipped.

**Delivers:** All 301 redirects implemented in Netlify `_redirects` and verified; unique page titles and meta descriptions on every page, informed by old site audit; XML sitemap generated; Google Search Console property claimed and sitemap submitted; Lighthouse / Core Web Vitals baseline; mobile testing on real devices (complete purchase flow); PayPal live credentials verified (sandbox-to-live switch); one real test purchase completed; DNS cutover executed; post-launch monitoring checklist.

**Addresses:** Pitfall 1 (URL redirects implemented), Pitfall 3 (DNS cutover last), Pitfall 5 (real-device mobile testing), Pitfall 6 (page titles audited), Pitfall 8 (live PayPal credentials), Pitfall 9 (Search Console)

**Research flag:** Standard patterns — 301 redirects via Netlify _redirects, sitemap submission via Search Console are well-documented processes. No research needed.

---

### Phase Ordering Rationale

- **Content audit before design** is non-negotiable based on pitfall research — designing around missing or unusable content is a launch blocker that surfaces at the worst possible time.
- **Design tokens before components** is a hard architectural dependency — every component inherits from the design system.
- **Core commerce pages before supporting content** reflects the site's purpose — the purchase funnel is the primary deliverable; marketing content is secondary.
- **SEO and verification as a final dedicated phase** prevents these tasks from being squeezed out by feature work — they are documented launch blockers, not polish items.
- **Staging environment on day one** is a risk mitigation prerequisite that protects years of accumulated domain authority.

### Research Flags

Phases needing deeper research or verification before starting:
- **Phase 3 (Product Commerce Pages):** Verify PayPal's current recommended embed method before implementing. STACK.md and PITFALLS.md both flag this — PayPal has changed their checkout products and deprecated Payments Standard. Check developer.paypal.com before touching any PayPal code.
- **Phase 2 (Design System):** Verify current Astro + Tailwind 4.x integration approach. STACK.md notes Astro 5.x and Tailwind 4.x versions as items to confirm before starting, as the official `@astrojs/tailwind` integration approach may differ from Tailwind 3.x patterns.

Phases with standard patterns (no phase research needed):
- **Phase 1 (Foundation/Audit):** Infrastructure setup and content inventorying — well-understood operational tasks.
- **Phase 4 (Content Pages):** Static content pages in Astro — fully documented, no novel patterns.
- **Phase 5 (SEO/Launch):** 301 redirects, sitemap submission, device testing — industry-standard checklist items.

---

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Stack | MEDIUM | Core recommendations (Astro, Tailwind, Netlify) are HIGH confidence. Version numbers (Astro 5.x, Tailwind 4.x) are MEDIUM — web verification was unavailable; confirm before starting Phase 2. |
| Features | MEDIUM | Based on established small-product-site patterns. No live competitor analysis was conducted. Recommend spot-checking 2–3 comparable specialty hardware sites before finalizing feature scope. |
| Architecture | HIGH | Well-established, stable patterns for static sites with Astro Content Collections and PayPal embeds. These patterns have been standard for 3+ years and do not depend on current API verification. |
| Pitfalls | MEDIUM | Well-documented patterns from SEO and site migration knowledge. The PayPal deprecation risk (Pitfall 2) specifically needs live verification — deprecation timelines shift and cannot be confirmed from training data alone. |

**Overall confidence:** MEDIUM — high enough to proceed to roadmap planning with confidence in the architecture and approach; low enough that Phase 2 and Phase 3 should begin with live verification of specific version/API questions called out in STACK.md and PITFALLS.md.

### Gaps to Address

- **PayPal API status:** Whether the legacy `webscr` Payments Standard endpoint is deprecated/sunset as of 2026 cannot be confirmed from training data. Verify at developer.paypal.com before Phase 3. This is the highest-priority gap — it affects what code gets written.
- **Astro + Tailwind 4 integration:** The `@astrojs/tailwind` integration's compatibility with Tailwind 4.x CSS-first configuration should be verified before Phase 2. Tailwind 4 changed the configuration approach significantly from v3.
- **Existing content quality:** The actual state of Gaslight's existing product photos, written copy, and page content is unknown until Phase 1 audit. This gap determines how much content production work precedes design.
- **Competitor spot-check:** No live competitor analysis was conducted. Before finalizing features, briefly review 2–3 comparable small specialty hardware sites to validate feature assumptions and identify any gaps.
- **Google Search Console access:** Whether the current gaslightconversions.com domain has an active Search Console property is unknown. Claiming it (if not already done) should happen in Phase 1, as it provides baseline ranking data before the rebuild begins.

---

## Sources

### Primary (HIGH confidence)
- Astro Content Collections API — training knowledge of well-documented stable API (since Astro 2.0)
- Netlify Forms and static hosting patterns — industry-standard, highly stable
- Static site architecture patterns — well-documented, 3+ years established

### Secondary (MEDIUM confidence)
- Astro 5.x version number — training data (cutoff August 2025); verify current version
- Tailwind CSS 4.x — announced early 2025; CSS-first config and Oxide engine confirmed; integration details need verification
- PayPal hosted button embed patterns — training knowledge; deprecation status needs live verification
- Small niche e-commerce feature patterns — established domain knowledge; no live competitor verification conducted

### Tertiary (needs validation)
- PayPal Payments Standard (`webscr`) deprecation timeline — flagged as uncertain; verify at developer.paypal.com before Phase 3
- Astro 5.x + Tailwind 4.x official integration (`@astrojs/tailwind`) — verify compatibility before Phase 2

---

*Research completed: 2026-02-23*
*Ready for roadmap: yes*
