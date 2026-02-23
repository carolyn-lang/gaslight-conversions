# Architecture Patterns

**Domain:** Small static product/e-commerce site (under 10 products, no backend)
**Project:** Gaslight Conversions
**Researched:** 2026-02-23
**Confidence:** HIGH (well-established patterns for this site class, no novel technology required)

---

## Recommended Architecture

**Verdict:** Fully-static site with file-based content, no API layer, PayPal JS SDK embedded per product page.

This is the simplest architecture that satisfies all requirements. There is no server, no database, no CMS, and no build-time data fetching from external APIs. Products are authored as Markdown or structured data files. Pages are generated at build time. The only runtime JavaScript is PayPal's checkout SDK, which handles the full payment flow externally.

```
Browser
  └── Static HTML/CSS/JS (served from CDN or static host)
        ├── Layout (shared shell: nav, header, footer)
        ├── Pages (home, catalog, product, about, FAQ, contact, guides)
        └── PayPal JS SDK (loaded per product page, points to PayPal servers)
              └── PayPal.com (handles all checkout, payment, confirmation)
```

---

## Component Boundaries

| Component | Responsibility | Communicates With |
|-----------|---------------|-------------------|
| Layout shell | Shared nav, header, footer, global styles, meta tags | All pages consume it |
| Home page | Hero, value proposition, product teaser grid, CTA | Links to catalog, about |
| Product catalog page | Grid of all products with image, name, short description, price | Links to individual product pages |
| Product detail page | Full description, photos, specs, PayPal buy button | PayPal JS SDK (outbound) |
| About page | Company origin story, family business narrative | None |
| Benefits/Why Convert page | Rationale for gas-to-electric conversion, value prop | Links to catalog |
| Installation guides page(s) | How-to content, step-by-step instructions | May link to products |
| FAQ page | Common customer questions and answers | None |
| Contact page | HTML form (no backend) | Form submission service (Netlify Forms, Formspree, or mailto) |
| PayPal Buy Button | Per-product checkout trigger, hosted SDK | PayPal.com (external) |
| Design tokens / global styles | Colors (orange, navy), typography, spacing scale | All pages/components |

### Boundary Rules

- **Layout shell is purely presentational.** It has no business logic, no data fetching.
- **Product data does not live in page files.** It lives in a structured data file (JSON, YAML, or Markdown frontmatter) and is consumed by the catalog and product pages at build time. This makes product updates a single-file edit.
- **PayPal is external.** The site embeds a script tag + button HTML that PayPal provides. No payment data touches the site's code or hosting.
- **Contact form submission is handled externally.** Options: Netlify Forms (zero-config if hosted on Netlify), Formspree (free tier sufficient), or a `mailto:` href fallback. No server-side email handler needed.

---

## Data Flow

### Product Display Flow (build time)
```
Product data file (JSON/YAML/Markdown)
  → Build tool reads at build time
  → Generates static catalog page (all products listed)
  → Generates static product detail pages (one per product)
  → HTML output contains product info, photo references, PayPal button code
```

### Checkout Flow (runtime, all external)
```
User clicks "Buy Now" on product detail page
  → PayPal JS SDK initializes (loaded from PayPal CDN)
  → PayPal popup/redirect opens (hosted on PayPal.com)
  → User completes payment on PayPal
  → PayPal sends confirmation to buyer's email
  → PayPal sends notification to seller's PayPal account
  → User returns to site (optional return URL)
```
**The site receives no payment data and handles no transaction state.** This is the correct model and a security/compliance advantage.

### Contact Form Flow (runtime)
```
User submits contact form
  → POST to form submission service (Netlify/Formspree) OR mailto: link
  → Service delivers email to Carolyn
  → No data stored on site infrastructure
```

### Asset Flow (images)
```
Source images in /public/images/ (or /src/assets/ for build-time optimization)
  → Build tool processes/optimizes (if using Astro's Image component)
  → Output as optimized WebP/AVIF with fallbacks
  → Served from static host / CDN
```

---

## Recommended File/Folder Structure

Using Astro (recommended framework for this site class):

```
gaslight-conversions/
├── public/
│   ├── images/
│   │   └── products/           # Raw product photos
│   ├── favicon.ico
│   └── robots.txt
├── src/
│   ├── layouts/
│   │   └── BaseLayout.astro    # Shell: nav, header, footer, meta, global CSS import
│   ├── pages/
│   │   ├── index.astro         # Home page
│   │   ├── catalog.astro       # All products grid
│   │   ├── products/
│   │   │   ├── [slug].astro    # Dynamic route → generates one page per product
│   │   ├── about.astro
│   │   ├── why-convert.astro   # Benefits page
│   │   ├── guides/
│   │   │   └── index.astro     # Installation guides landing
│   │   ├── faq.astro
│   │   └── contact.astro
│   ├── components/
│   │   ├── layout/
│   │   │   ├── Nav.astro
│   │   │   ├── Footer.astro
│   │   │   └── SiteHeader.astro
│   │   ├── products/
│   │   │   ├── ProductCard.astro    # Used in catalog grid
│   │   │   └── PayPalButton.astro  # Encapsulates PayPal embed code
│   │   └── ui/
│   │       ├── Button.astro
│   │       └── Hero.astro
│   ├── content/
│   │   ├── products/           # One .md or .json file per product
│   │   │   ├── outdoor-lighting-fixture.md
│   │   │   ├── low-voltage-kit.md
│   │   │   └── gaslight-base-unit.md
│   │   └── guides/             # One .md file per installation guide
│   └── styles/
│       └── global.css          # Design tokens, base reset, typography
├── astro.config.mjs
├── package.json
└── tsconfig.json
```

### Key Structure Decisions

- **`src/content/products/`** — Each product is a Markdown file with frontmatter (name, slug, price, description, PayPal button ID, image path). This is the single source of truth. Adding/changing a product means editing one file.
- **`src/pages/products/[slug].astro`** — Astro's file-based dynamic routing generates one static HTML page per product at build time. No server needed.
- **`src/components/products/PayPalButton.astro`** — Isolates all PayPal SDK code. If PayPal's embed approach changes, one file to update.
- **`public/images/`** vs **`src/assets/`** — Use `src/assets/` for product images if using Astro's built-in image optimization (`<Image>` component). Use `public/` for images that should not be transformed.

---

## Patterns to Follow

### Pattern 1: Content Collections for Products

**What:** Astro Content Collections — a typed, schema-validated way to manage structured Markdown/JSON content at build time.

**When:** Any time you have a set of similar items (products, guides) that share a schema.

**Why:** Validates frontmatter at build time. Auto-generates slugs. Provides typed data in templates. Adding a product is a one-file operation.

**Example product file (`src/content/products/low-voltage-kit.md`):**
```markdown
---
title: "Low Voltage Conversion Kit"
slug: "low-voltage-kit"
price: 49.95
description: "Converts standard gaslight fixtures to low-voltage electric operation."
image: "/images/products/low-voltage-kit.jpg"
paypal_button_id: "XXXXXXXXXXXXXXX"
featured: true
---

Full product description in Markdown here. Supports rich content,
bullet lists for specs, etc.
```

**Example schema (`src/content/config.ts`):**
```typescript
import { defineCollection, z } from 'astro:content';

const products = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    slug: z.string(),
    price: z.number(),
    description: z.string(),
    image: z.string(),
    paypal_button_id: z.string(),
    featured: z.boolean().default(false),
  }),
});

export const collections = { products };
```

### Pattern 2: PayPal Hosted Button Embed

**What:** Use PayPal's "Hosted Button" (generated in the PayPal merchant dashboard). PayPal provides an HTML snippet with a form POST or JavaScript SDK. Embed per product page.

**Why this approach over PayPal JS SDK Smart Buttons:** Hosted buttons are simpler, require no JavaScript configuration per product, and are fully managed in the PayPal dashboard. For a developer-maintained site with under 10 products, this is the right tradeoff. Smart Buttons require more JS setup but offer better UX; either works here.

**Example PayPalButton.astro:**
```astro
---
interface Props {
  buttonId: string;
  productName: string;
}
const { buttonId, productName } = Astro.props;
---

<form action="https://www.paypal.com/cgi-bin/webscr" method="post" target="_top">
  <input type="hidden" name="cmd" value="_s-xclick" />
  <input type="hidden" name="hosted_button_id" value={buttonId} />
  <input type="hidden" name="currency_code" value="USD" />
  <button type="submit" class="paypal-btn">
    Buy {productName} via PayPal
  </button>
</form>
```

### Pattern 3: Single Layout Shell

**What:** All pages use one `BaseLayout.astro` that accepts page-specific title/description via props.

**Why:** Ensures consistent nav, footer, and `<head>` metadata without duplication. SEO meta changes happen in one place.

**Example:**
```astro
---
// BaseLayout.astro
interface Props {
  title: string;
  description?: string;
}
const { title, description = "Gaslight Conversions — Gas to electric lamp conversion kits." } = Astro.props;
---
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>{title} | Gaslight Conversions</title>
    <meta name="description" content={description} />
    <link rel="stylesheet" href="/styles/global.css" />
  </head>
  <body>
    <Nav />
    <main><slot /></main>
    <Footer />
  </body>
</html>
```

---

## Anti-Patterns to Avoid

### Anti-Pattern 1: Hardcoding Product Data in Page Files

**What:** Writing product titles, prices, and descriptions directly inside `.astro` page files instead of in content files.

**Why bad:** Adding or changing a product requires editing template files, not content files. Mixing data and presentation makes future updates error-prone. Risk of inconsistency between catalog page and product detail page.

**Instead:** All product data lives in `src/content/products/`. Pages consume it via `getCollection('products')`.

---

### Anti-Pattern 2: One HTML File Per Product (no templating)

**What:** Creating `outdoor-fixture.html`, `low-voltage-kit.html`, etc. as separate, manually-maintained files.

**Why bad:** Changing the product page layout requires editing every file. N products = N times the maintenance risk. This is the old-site problem.

**Instead:** One `[slug].astro` template + one content file per product.

---

### Anti-Pattern 3: Storing PayPal Button HTML in Multiple Places

**What:** Copying the PayPal form snippet into each product page directly.

**Why bad:** If PayPal's hosted button endpoint changes, or the form needs an additional field, you must update every product page.

**Instead:** Isolate it in `PayPalButton.astro` and pass `buttonId` as a prop.

---

### Anti-Pattern 4: Building a Backend for the Contact Form

**What:** Adding a Node/Express server or serverless function to handle form submissions and send email.

**Why bad:** Adds infrastructure, credentials management, and maintenance overhead for a feature Formspree or Netlify Forms handles for free at this scale.

**Instead:** Use Netlify Forms (if hosting on Netlify) or Formspree. Zero backend required.

---

## Suggested Build Order

Dependencies between components determine the correct sequence.

```
1. Foundation (no dependencies)
   └── Design tokens, global CSS, color system, typography

2. Layout shell (depends on: design tokens)
   └── BaseLayout, Nav, Footer

3. Product data (depends on: nothing — pure content)
   └── Content collection schema + product .md files

4. Core product pages (depends on: layout, product data)
   ├── Catalog page (product grid)
   └── Product detail page template + PayPalButton component

5. Supporting content pages (depends on: layout only)
   ├── Home page (hero, product teaser, value prop)
   ├── About page
   ├── Benefits/Why Convert page
   └── FAQ page

6. Functional pages (depends on: layout + external service)
   ├── Contact page (depends on: Formspree/Netlify Forms setup)
   └── Installation guides (depends on: guide content files)

7. Polish and integration
   ├── Image optimization (product photos)
   ├── SEO metadata per page
   └── Mobile responsiveness pass
```

**Rationale for this order:**

- Design tokens first because every component depends on them. Starting with ad-hoc color values leads to inconsistency.
- Layout shell before any pages because pages cannot be previewed without it.
- Product data before product pages because the template needs data to render against during development.
- Core product/commerce pages before marketing pages — they are the site's primary purpose and contain the most complexity (PayPal integration).
- Contact page later because it depends on an external service decision (Netlify vs Formspree) that can be deferred.

---

## Scalability Considerations

This site does not need to scale in the traditional sense. All concerns are about maintainability at the "1 developer, ~10 products" scale.

| Concern | At current scale (< 10 products) | If ever expanding |
|---------|-----------------------------------|-------------------|
| Adding a product | Add one .md file, add product photos | Same process — no architecture change needed up to ~100 products |
| Changing design | Edit design tokens + layout shell | Same — centralized |
| Changing PayPal button | Edit PayPalButton.astro, update button ID in product .md | Same |
| Contact form volume | Free tier of Formspree/Netlify Forms sufficient | Upgrade plan or add backend |
| Image management | Manual file drop + reference in .md | Consider a media library if photos grow substantially |

---

## Hosting Architecture

**Recommended:** Netlify or Vercel (static hosting with CDN, free tier sufficient).

```
Git repo (GitHub/GitLab)
  → Push to main triggers build
  → Astro builds static HTML/CSS/JS
  → Output deployed to CDN edge nodes
  → Custom domain (gaslightconversions.com) pointed via DNS
  → HTTPS automatic (Let's Encrypt)
  → Netlify Forms handles contact form (if using Netlify)
```

No server processes run post-deploy. The entire site is flat files served from a CDN. This is both the simplest and most performant option for this use case.

---

## Sources

- Astro project structure and Content Collections: training knowledge (HIGH confidence — stable, well-documented API since Astro 2.0)
- PayPal hosted button embed: training knowledge (HIGH confidence — longstanding PayPal merchant feature, form POST to paypal.com/cgi-bin/webscr)
- Static site hosting patterns (Netlify/Vercel): training knowledge (HIGH confidence — industry standard)
- File-based routing in Astro: training knowledge (HIGH confidence — core Astro feature, unchanged)

**Note:** External research tools were unavailable during this session. All findings are based on training knowledge of well-established, stable patterns in the static site ecosystem. Confidence is HIGH for this site class because nothing here is novel — these patterns have been standard for 3+ years.
