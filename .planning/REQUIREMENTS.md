# Requirements: Gaslight Conversions

**Defined:** 2026-02-23
**Core Value:** Customers can find the right kit, understand how it works, and buy it with confidence — on any device.

## v1 Requirements

### Migration & Pre-Launch Prep

- [ ] **MIGR-01**: Old site URLs are crawled and inventoried before any build work begins
- [ ] **MIGR-02**: All old site URLs have 301 redirects configured to new URL structure before DNS cutover
- [ ] **MIGR-03**: Existing product photos and copy are audited for quality before design begins

### Products

- [ ] **PROD-01**: User can browse all conversion kits, fixtures, and components on a product catalog page
- [ ] **PROD-02**: User can view a dedicated product detail page with description and photos for each product
- [ ] **PROD-03**: User can purchase a product directly from its detail page via a PayPal buy button

### Content

- [ ] **CONT-01**: User can access step-by-step installation guides for each conversion kit
- [ ] **CONT-02**: User can submit an inquiry via a contact form that delivers to the business owner

### Design & Foundation

- [ ] **DSNG-01**: Site renders correctly and is fully usable on mobile phones and tablets
- [ ] **DSNG-02**: Site uses a mid-century design system (orange + navy palette, classic typography, period feel)
- [ ] **DSNG-03**: Product images are updated, optimized, and visually consistent across the site
- [ ] **DSNG-04**: Each page has a unique title, meta description, and a sitemap is generated for search engines

## v2 Requirements

### Content

- **V2-CONT-01**: About / origin story page — who invented this and why
- **V2-CONT-02**: Benefits / "Why Convert?" page — reasons to switch from gas to electric
- **V2-CONT-03**: FAQ page answering common customer questions

### Products

- **V2-PROD-01**: Product filtering by category (kits vs fixtures vs components)

## Out of Scope

| Feature | Reason |
|---------|--------|
| Shopping cart / multi-item checkout | PayPal handles this via individual buy buttons — no cart needed at this scale |
| CMS / admin panel | Carolyn manages updates directly in code |
| Mail order form | Replacing with online-only purchasing via PayPal |
| Sitemap page (old-style) | XML sitemap for search engines replaces the old HTML sitemap nav page |
| User accounts / login | Not needed for a small product catalog |
| Real-time chat / live support | Unnecessary complexity; contact form handles inquiries |
| Product reviews / ratings | Out of scope for v1 |

## Traceability

Which phases cover which requirements. Updated during roadmap creation.

| Requirement | Phase | Status |
|-------------|-------|--------|
| MIGR-01 | Phase 1 | Pending |
| MIGR-02 | Phase 1 | Pending |
| MIGR-03 | Phase 1 | Pending |
| PROD-01 | Phase 3 | Pending |
| PROD-02 | Phase 3 | Pending |
| PROD-03 | Phase 3 | Pending |
| CONT-01 | Phase 4 | Pending |
| CONT-02 | Phase 4 | Pending |
| DSNG-01 | Phase 2 | Pending |
| DSNG-02 | Phase 2 | Pending |
| DSNG-03 | Phase 3 | Pending |
| DSNG-04 | Phase 5 | Pending |

**Coverage:**
- v1 requirements: 12 total
- Mapped to phases: 12
- Unmapped: 0 ✓

---
*Requirements defined: 2026-02-23*
*Last updated: 2026-02-23 after roadmap creation*
