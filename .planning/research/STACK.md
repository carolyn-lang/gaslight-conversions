# Technology Stack

**Project:** Gaslight Conversions
**Researched:** 2026-02-23
**Confidence note:** All tool access (WebSearch, WebFetch, Context7) was blocked during this research session. Findings are based on training data (cutoff August 2025). Confidence levels are honest about this limitation. Before starting Phase 1, verify current Astro and Tailwind versions.

---

## Recommended Stack

### Core Framework

| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| Astro | 5.x | Static site generator / page framework | Purpose-built for content-first sites. Ships zero JS by default — ideal for a product catalog where performance and SEO matter. No server needed. Supports `.astro` component format (HTML-first) that gives Carolyn full markup control. First-class TypeScript. Active ecosystem. Output is plain HTML/CSS that deploys anywhere. |

**Why Astro over alternatives:**

- **Not Next.js:** Next.js is a React framework optimized for apps. This site has no dynamic data, no auth, no API routes that need a server. Next.js adds unnecessary complexity and runtime overhead (React hydration) for a brochure/product catalog site. Overkill.
- **Not Gatsby:** Declining ecosystem as of 2024-2025. Netlify (steward) has reduced investment. The GraphQL data layer adds complexity with no benefit at this scale.
- **Not plain HTML/CSS:** Valid option for this scale, but Astro gives layout components, reusable product card templates, and a build pipeline without adding framework complexity. Carolyn benefits from component reuse across 10 product pages without copy-paste HTML.
- **Not SvelteKit:** Strong option but Astro is simpler for a content site. SvelteKit shines when you need reactive UI. This site doesn't.
- **Not Hugo/Jekyll:** Ruby/Go ecosystems add toolchain friction. Astro's JS/TS ecosystem is more familiar to modern developers.

**Confidence:** MEDIUM — Astro 5.x was current at August 2025. Version number may have advanced. Core recommendation confidence is HIGH (Astro is the right tool for this use case).

---

### Styling

| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| Tailwind CSS | 4.x | Utility-first CSS | Tailwind 4.0 was released early 2025 with a rewritten engine (Oxide, Rust-based). Significantly faster. CSS-first configuration — design tokens live in CSS, not tailwind.config.js. Ideal for a mid-century design system: you define the orange/navy palette once, use utilities everywhere. Carolyn gets full visual control without fighting a component library's design opinions. |

**Why Tailwind over alternatives:**

- **Not Bootstrap:** Bootstrap's grid and components impose visual patterns that fight the custom mid-century aesthetic. You spend time overriding, not designing.
- **Not a CSS-in-JS library:** Doesn't integrate well with Astro's zero-JS-by-default model.
- **Not vanilla CSS modules:** Valid, but Tailwind's utility approach is faster for a developer with a clear design vision. The palette and spacing scale can be set once in CSS variables (Tailwind 4 approach) and used consistently.
- **Not Open Props:** Good library, but Tailwind has better tooling, purging, and developer familiarity.

**Confidence:** MEDIUM — Tailwind 4.x was released in early 2025. Verify current patch version before starting. Core recommendation is HIGH confidence.

---

### Deployment / Hosting

| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| Netlify (or Vercel) | — | Static site hosting + CDN | Both offer free tiers suitable for a low-traffic product site. Astro builds to static HTML. Drop it on either platform: git push deploys, global CDN, automatic HTTPS. No server to manage. Netlify has slightly simpler form handling (relevant for the contact form). |

**Netlify vs Vercel for this project:**

Choose Netlify if:
- You want the contact form handled by Netlify Forms (no backend code required — just add `netlify` attribute to the `<form>` tag)
- You prefer a simpler deployment dashboard

Choose Vercel if:
- Carolyn already has a Vercel account
- You anticipate needing server-side features later (easy to add Astro SSR endpoints)

**Recommendation: Netlify** — the contact form feature alone saves writing a serverless function or integrating a third-party form service.

**Confidence:** HIGH — Both platforms are well-established and stable. Free tiers are reliable for low-traffic sites.

---

### Supporting Libraries

| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| `@paypal/paypal-js` | latest | PayPal JS SDK loader | Use to load PayPal's Smart Payment Buttons (now called "Pay Later" / "PayPal Checkout") programmatically. Handles async SDK loading cleanly. |
| Astro Image (built-in) | (Astro core) | Responsive image optimization | Astro's `<Image>` component — converts product photos to modern formats (WebP/AVIF), generates srcsets, lazy loads. Zero config. |
| Fontsource | latest | Self-hosted web fonts | Serve fonts from your own domain instead of Google Fonts CDN. Better privacy, no external dependency, works with Astro's asset pipeline. Pick fonts that evoke mid-century character (see notes). |

---

## PayPal Integration Approach

**Confidence:** MEDIUM (based on PayPal docs as of mid-2025; PayPal has changed their checkout products multiple times — verify before implementing)

**Recommended approach: PayPal Smart Payment Buttons (v2 SDK)**

PayPal's current standard embed for small sellers is the JavaScript SDK with hosted buttons. Each product page includes a PayPal button that opens PayPal's hosted checkout flow.

**How it works in Astro:**

1. Load the PayPal JS SDK via `<script>` tag (or via `@paypal/paypal-js` npm package) in the product page's `<head>` or as a client-side `<script>` in the `.astro` component.
2. Each product page renders a `<div id="paypal-button-container">` in the correct position.
3. A small inline `<script>` (Astro's `<script>` tag) calls `paypal.Buttons({ ... }).render('#paypal-button-container')` with that product's price and item name.

**Alternative: PayPal-hosted "Buy Now" buttons (legacy)**

PayPal's merchant portal generates static HTML snippets with embedded `<form>` tags that POST to PayPal. No JavaScript SDK required. These are simpler but less customizable visually. If the dad's existing site uses these, they can be dropped directly into Astro templates. This is the path of least resistance for an initial launch.

**Recommendation:** Start with the static PayPal-generated button HTML (legacy `<form>` embed) for MVP. It requires no JavaScript framework integration, is proven, and the dad already knows how to manage PayPal order settings. Migrate to Smart Payment Buttons in a later phase if better UX (animated buttons, Pay Later options) is desired.

---

## Font Recommendation for Mid-Century Aesthetic

| Font | Source | Role | Why |
|------|--------|------|-----|
| DM Serif Display or Playfair Display | Google Fonts / Fontsource | Headings | Serif typefaces with editorial character — evoke the warmth and weight of 1950s-60s signage without being costume-y |
| Inter or DM Sans | Google Fonts / Fontsource | Body / UI | Clean, neutral sans-serif that lets the serif headings lead. Legible at small sizes. |

**Not recommended:** Retro/novelty fonts (too on-the-nose), system font stacks alone (no character), Futura knockoffs (overused).

---

## What NOT to Use

| Technology | Why Not |
|------------|---------|
| WordPress / WooCommerce | Admin overhead, plugin maintenance, security patching — all unnecessary for 10 products. Carolyn edits code directly; a PHP CMS adds no value and significant complexity. |
| Shopify | Monthly fees, locked ecosystem, limited design control. Overkill for under 10 SKUs with PayPal already working. |
| Contentful / Sanity / any headless CMS | No content editors on this project. Carolyn codes directly. A CMS adds indirection and API dependency for zero benefit. |
| React / Vue / Svelte SPA | Single-page apps are for apps. This is a marketing/catalog site. SPAs add JS bundle weight, complexity, and hurt SEO without meaningful UX improvement for static content. |
| Next.js | Server-side React framework. Correct for dynamic apps; wrong tool for a static product catalog. Forces React component model on every page. |
| Gatsby | Diminished ecosystem, GraphQL data layer complexity for a use case that needs neither. |
| Bootstrap | Enforces Bootstrap's visual language; fighting it for a custom mid-century aesthetic creates more work than starting with Tailwind utilities. |
| Emotion / styled-components | CSS-in-JS doesn't fit Astro's HTML-first model or zero-JS default. |
| Stripe / Square | Changing payment processor is explicitly out of scope. PayPal only. |

---

## Alternatives Considered

| Category | Recommended | Alternative | Why Not |
|----------|-------------|-------------|---------|
| Framework | Astro 5.x | SvelteKit | SvelteKit excellent but adds reactivity overhead for a content site; Astro simpler |
| Framework | Astro 5.x | Next.js 15 | Server-oriented React app framework; wrong shape for static catalog |
| Framework | Astro 5.x | Plain HTML + Vite | Valid for 10 pages; Astro adds layout components and build pipeline worth the small overhead |
| CSS | Tailwind 4.x | Vanilla CSS | Tailwind faster for a design system with a defined palette; no overhead for Carolyn who knows Tailwind |
| Hosting | Netlify | Vercel | Both excellent; Netlify Forms tip the balance for the contact form requirement |
| PayPal | Static form buttons | Smart Payment Buttons JS | Smart buttons better UX long-term; static buttons simpler for MVP launch |
| Fonts | Fontsource | Google Fonts CDN | Fontsource self-hosts, removes Google CDN dependency, better privacy |

---

## Installation

```bash
# Scaffold Astro project
npm create astro@latest gaslight-conversions

# During scaffold wizard:
# - Template: Empty
# - TypeScript: Yes (strict)
# - Install deps: Yes

# Add Tailwind CSS integration
npx astro add tailwind

# Add Fontsource fonts (example — confirm font choice first)
npm install @fontsource/playfair-display @fontsource/dm-sans

# Optional: PayPal JS SDK loader
npm install @paypal/paypal-js
```

---

## Project Structure (Astro)

```
src/
  components/
    ProductCard.astro       # Reusable product card
    PayPalButton.astro      # Wrapper for PayPal embed per product
    Nav.astro
    Footer.astro
  layouts/
    BaseLayout.astro        # HTML shell, meta tags, fonts
    ProductLayout.astro     # Product page layout with sidebar/gallery
  pages/
    index.astro             # Homepage
    products/
      index.astro           # Product catalog
      [slug].astro          # Individual product pages
    about.astro
    how-it-works.astro
    faq.astro
    contact.astro
  styles/
    global.css              # Tailwind base + custom design tokens (colors, fonts)
  data/
    products.ts             # Product data (name, price, description, PayPal button ID)
public/
  images/                   # Product photos
```

**Why this structure:** Products live in `src/data/products.ts` (a TypeScript array of objects). Each product has a slug, name, price, description, and PayPal button ID. Astro's static generation loops over this array to generate individual product pages. Adding a new product = adding one object to the array. No CMS, no database.

---

## Sources

- Astro documentation: https://docs.astro.build (not verified in this session — tool access blocked)
- Tailwind CSS v4 announcement: https://tailwindcss.com/blog/tailwindcss-v4 (not verified in this session)
- PayPal developer documentation: https://developer.paypal.com/docs/checkout/ (not verified in this session)
- Training data (cutoff August 2025) — MEDIUM confidence on version numbers, HIGH confidence on architectural recommendations

**NOTE FOR ROADMAP:** Before Phase 1 kickoff, verify:
1. Current Astro version (was 5.x as of mid-2025)
2. Tailwind 4.x compatibility with Astro's official integration (`@astrojs/tailwind` or direct PostCSS approach)
3. PayPal's current recommended embed method — their product names and SDKs change frequently
