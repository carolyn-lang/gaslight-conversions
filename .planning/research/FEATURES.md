# Feature Landscape

**Domain:** Small niche e-commerce — specialty hardware / home improvement conversion kits
**Project:** Gaslight Conversions (gas-to-electric outdoor lighting conversion kits)
**Researched:** 2026-02-23
**Confidence:** MEDIUM — based on established patterns for small niche product sites; no live competitor data fetched (tool access restricted during research session)

---

## Table Stakes

Features users expect. Missing = product feels incomplete or untrustworthy.

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| Product catalog page | Customers need to see all available products at a glance | Low | Under 10 products; a single page with cards/grid works perfectly |
| Individual product pages | Detailed view per product — the decision page | Low–Med | Needs photos, description, specs, price, buy button |
| High-quality product photos | Hardware customers need to verify what they're buying; trust signal | Low | Multiple angles; show the kit assembled and installed if possible |
| Pricing visible on product page | Customers won't call to ask price; hidden pricing = lost sale | Low | PayPal buy buttons can display price; also show it in plain text |
| PayPal buy button | The defined checkout path; customers expect a recognizable payment option | Low | Embedded per product page; PayPal's hosted button requires no server logic |
| Mobile-responsive design | Majority of browsing is mobile; homeowners use phones while standing at their lamp post | Med | Critical — current site is not responsive, this is a baseline requirement |
| Contact form or email | Customers have pre-purchase questions; hardware buyers especially | Low | Simple form or mailto link; they need to ask "will this fit my lamp?" |
| About / origin story page | Niche product from a family business — trust and credibility come from the story | Low | "Who are you and why should I trust this product?" is a real purchase barrier |
| Clear navigation | Users need to get from home to product to buy without confusion | Low | Simple nav: Home, Products, How It Works / FAQ, About, Contact |
| SSL / HTTPS | Required for any commerce; browsers actively warn on HTTP | Low | Hosting-level concern but must be addressed; without it users see security warnings |
| Legible typography and readable text | Product descriptions, installation notes — if text is hard to read, users leave | Low | Contrast ratios, font size, line length — especially on mobile |
| Page load speed | Slow sites lose customers; especially important for older demographics who may be less patient | Med | Optimize images, avoid heavy frameworks where not needed |

## Differentiators

Features that set this product/site apart from generic hardware stores or Amazon listings. Not expected, but valued — and achievable at small-site scale.

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| Installation guide / how-to content | Reduces "can I do this myself?" anxiety; builds confidence before purchase | Low–Med | Step-by-step text + photos; positions Gaslight as the expert, not just a vendor |
| Benefits / "Why Convert?" page | Addresses the primary customer objection: "why not just replace the whole fixture?" Educates on the value of preservation + modernization | Low | Emotional + practical arguments: aesthetics, cost savings, ease |
| Product compatibility / fit guide | "Will this work with my lamp?" is the #1 pre-purchase question for conversion kits; answering it on-site closes sales without requiring a phone call | Med | Could be a table of compatible fixture types, pole sizes, or a simple checklist |
| FAQ page | Reduces support burden; answers "does this work with 120V?", "is installation licensed?", "what's the warranty?" | Low | Especially valuable for dual audience (homeowners vs. commercial); different concerns |
| Business origin / invention story | Family business with a proprietary invention — this is a genuine differentiator vs. generic kit suppliers; story builds emotional connection and trust | Low | Dad invented this; lean into that authenticity |
| Commercial customer section or callout | Commercial buyers (municipalities, HOAs, property managers) have different needs: bulk pricing, professional install, invoicing | Med | Could be a simple callout on relevant pages or a dedicated "For Businesses" section; even acknowledging them signals you understand their context |
| Testimonials or use-case photos | Social proof for a niche product is powerful; "I installed this on my 1920s carriage lamp" is more convincing than specs alone | Low–Med | Even 3–5 real customer photos or quotes significantly increase conversion |
| Clear warranty / guarantee statement | Hardware buyers want to know what happens if it doesn't work | Low | Even a simple "satisfaction guaranteed" or return policy statement reduces purchase risk |
| Vintage/mid-century brand aesthetic | The orange + navy palette and classic character signal authenticity — this is a product for people who care about preserving period architecture, not modernizing it | Low | Design IS a differentiator here; a generic Bootstrap site would undermine the product's identity |

## Anti-Features

Features to explicitly NOT build at this scale. These add complexity without proportional value.

| Anti-Feature | Why Avoid | What to Do Instead |
|--------------|-----------|-------------------|
| Shopping cart / multi-item checkout | Under 10 products, low transaction volume; PayPal buy buttons handle per-item purchase; cart adds complexity for zero benefit at this scale | One PayPal buy button per product; PayPal's cart is available if customer wants to buy multiple items |
| User accounts / login | No repeat-purchase catalog logic that requires account state; adds auth complexity, security surface, GDPR concerns | No accounts; contact form for post-purchase support |
| CMS / admin panel | Carolyn edits code directly; a CMS adds a dependency, a login surface, and maintenance burden | Static HTML or template files Carolyn can edit in a text editor or Git |
| Inventory management | Under 10 SKUs, no real-time inventory pressure; PayPal does not require stock tracking integration | Manual updates when a product goes out of stock |
| Product reviews / ratings system | Requires a database, moderation, spam prevention; too heavy for this scale | Curated testimonials/photos added manually as static content |
| Live chat | Adds third-party script weight and an ongoing support obligation; not staffed for real-time response | Contact form with clear response time expectation ("We respond within 1 business day") |
| Newsletter / email marketing | Requires list management, CAN-SPAM compliance, ongoing content; not the core business goal | About page + contact form captures interest; add later if business scales |
| Search functionality | Under 10 products; navigation + catalog page is sufficient; adding search requires indexing logic | Well-organized catalog page with clear product names makes search unnecessary |
| Related products / upsell engine | Requires logic, data, possibly JS framework overhead; at sub-10 products, the catalog IS the upsell | Link to catalog from product pages ("See all products") |
| Analytics dashboard / reporting | Out of scope; Carolyn doesn't need a custom dashboard | Embed Google Analytics or Plausible snippet; view data in their own UI |
| Wholesale / bulk ordering portal | Different UX/pricing logic; adds complexity | Simple "Contact us for bulk orders" callout for commercial customers |

---

## Feature Dependencies

```
Product catalog page
  → Individual product pages (catalog links to detail)
      → PayPal buy button (lives on product page)
      → Product photos (appear on product page)
      → Compatibility / fit guide (can live on or link from product page)

Benefits / "Why Convert?" page
  → About / origin story (both build trust; can cross-link)

FAQ page
  → Contact form (FAQ reduces inbound; contact catches what FAQ misses)

Installation guide
  → Product pages (link from product to relevant guide)

Mobile-responsive design
  → All pages (not a feature but a constraint on all feature implementations)
```

---

## MVP Recommendation

Build these first — they cover the full purchase funnel:

1. **Product catalog page** — users need to see what exists
2. **Individual product pages** with photos, description, specs, PayPal buy button — the decision and purchase moment
3. **Mobile-responsive design** — the current site's biggest failure; fix this at the foundation
4. **Contact form** — pre-purchase questions are the main support need for a hardware product
5. **About / origin story page** — family business with a proprietary invention; trust is a purchase factor
6. **How It Works / FAQ page** — reduces friction; answers the "will this work for me?" question

Add in second pass (low complexity, high value):
- **Benefits / "Why Convert?" page** — content marketing that closes the "why bother" objection
- **Installation guide content** — reduces post-purchase support and builds confidence pre-purchase
- **Compatibility / fit guide** — reduces the #1 pre-purchase question

Defer (or skip entirely):
- **Commercial customer section** — acknowledge them in FAQ or contact page first; build dedicated content only if commercial inquiries are significant
- **Testimonials** — seed with any existing customer feedback; do not block launch on this

---

## Audience-Specific Notes

### Homeowners
- Primary concern: "Can I install this myself, or do I need an electrician?"
- Secondary concern: "Will it look right on my existing fixture?"
- Key content: Installation difficulty level, compatibility info, photos of installed product

### Commercial Customers (HOAs, municipalities, property managers)
- Primary concern: "Can I order in volume? Who handles installation?"
- Secondary concern: "Is this a reputable supplier for a professional project?"
- Key content: About / origin story (legitimacy signal), contact form ("ask us about bulk orders"), business longevity signal

---

## Sources

- Project context: `/Users/carolyn/Gaslight/.planning/PROJECT.md`
- Domain knowledge: Established patterns for small niche product sites, specialty hardware e-commerce, and PayPal buy button integration — based on training data (confidence: MEDIUM; web verification was unavailable during this research session)
- Note: Competitor analysis of live comparable sites (e.g., small specialty lighting vendors, gas lamp conversion suppliers) was not conducted due to tool access restrictions. Recommend spot-checking 2–3 comparable sites before finalizing feature scope.
