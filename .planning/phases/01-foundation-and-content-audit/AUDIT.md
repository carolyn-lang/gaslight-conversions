# Gaslight Conversions — Site Audit

**Audited:** 2026-02-23
**Auditor:** Claude (automated crawl) + Carolyn (photo/copy quality review)

---

## Domain Status

| Field | Value |
|-------|-------|
| Domain | gaslightconversions.com |
| Registry Expiry Date | 2026-12-20 |
| Registrar | Amazon Registrar, Inc. |
| Name Servers | NS-1270.AWSDNS-30.ORG, NS-1673.AWSDNS-17.CO.UK, NS-278.AWSDNS-34.COM, NS-712.AWSDNS-25.NET |
| Auto-renew | [Yes] |
| Creation Date | 1999-12-20 |
| Last Updated | 2025-11-15 |

**Action needed if expiry within 60 days of project completion:** Renew immediately.

Note: Expiry is 2026-12-20 — approximately 10 months from audit date (2026-02-23). No immediate risk, but confirm auto-renew is enabled in Amazon Registrar console.

---

## URL Inventory (MIGR-01)

All 10 known URLs verified live (HTTP 200) on 2026-02-23. No additional URLs found beyond the known navigation.

| Old URL | HTTP Status | Page Purpose | Confirmed New URL |
|---------|-------------|-------------|-------------------|
| `/` | 200 | Home | `/` |
| `/kits.html` | 200 | Conversion kits catalog (LED, Low Voltage, High Voltage) | `/products` |
| `/bulbs.html` | 200 | Replacement bulbs (LV and HV) | `/products` |
| `/components.html` | 200 | Gaslight components (base, eye, transformer, fixtures, lantern) | `/products` |
| `/mail_order.html` | 200 | Mail order form (being dropped) | `/products` |
| `/benefits.html` | 200 | Benefits page (v2 candidate or drop) | `/` |
| `/guide.html` | 200 | Installation guides (links to PDF guides) | `/install-guides` |
| `/faq.html` | 200 | FAQ | `/faq` |
| `/sitemap.html` | 200 | HTML sitemap (dropping — replaced by XML sitemap in Phase 5) | `/` |
| `/contact_us.html` | 200 | Contact form | `/contact` |

**Additional URLs discovered in crawl (not in navigation):**
None found. All pages link only to the 10 known URLs above (plus `index.html` which is the same as `/`).

---

## Content Quality Audit (MIGR-03)

> Status values: `usable` | `needs-update` | `must-replace`
> Claude: fills photo URLs and file sizes. Carolyn: fills all Status columns at checkpoint.

**Important context on all product images:** All product photos are GIF format with file sizes under 10KB. This is extremely small for modern product photography — typical modern product images are 50KB–500KB. These images were likely created for the 1999–2000 era web when small file sizes were a priority. Quality review by Carolyn required to determine usability.

---

### /kits.html — Conversion Kits

**Photos found:**
| Image URL | File Size | Status |
|-----------|-----------|--------|
| `/images/gaslight_led_kit.gif` | 7,401 bytes | [usable] |
| `/images/gaslight_lv_kit.gif` | 6,348 bytes | [usable] |
| `/images/gaslight_hv_kit.gif` | 4,698 bytes | [usable] |

**Copy excerpt:**
> New LED Conversion Kit — New LED Bulbs feature 10 year life! High Power LED Fixture — 10 year lamp life, Constant current LED driver / photo electric control, and Low voltage transformer.
> Low Voltage Conversion Kit — be directly wired into any 1/2" knockout, conveniently plugged into any outlet. Complete Kit includes: two bulb inverted fixture, 2-12 volt bulbs, transformer, and photo electric.
> High Voltage Conversion Kit — kit easily secures to any gas lamp or electrical fixture. Kit includes: two bulb fixture, 2-15w bulbs, photo electric eye and detailed step-by-step instructions.

**Copy status:** [Usable]

---

### /bulbs.html — Replacement Bulbs

**Photos found:**
| Image URL | File Size | Status |
|-----------|-----------|--------|
| `/images/lv_bulbs.gif` | 4,549 bytes | [usable] |
| `/images/15w_hv_bulbs.gif` | 3,015 bytes | [usable] |
| `/images/25w_hv_bulbs.gif` | 2,206 bytes | [usable] |

**Copy excerpt:**
> Pack of 12V low voltage frosted bulbs. (illumination equal to a 30 Watt light bulb)
> High Voltage 15 Watt Bulbs — pack of 15 watt high voltage frosted bulbs.
> Pack of 25 watt high voltage bulbs.

**Copy status:** [Usable]

---

### /components.html — Gaslight Components

**Photos found:**
| Image URL | File Size | Status |
|-----------|-----------|--------|
| `/images/gaslight_base.gif` | 4,575 bytes | [usable] |
| `/images/electric_eye01.gif` | 5,514 bytes | [usable] |
| `/images/transformer01.gif` | 4,196 bytes | [usable] |
| `/images/lv_fixture.gif` | 3,177 bytes | [usable] |
| `/images/lantern01.gif` | 3,025 bytes | [usable] |
| `/images/hv_fixture.gif` | 3,222 bytes | [usable] |

**Copy excerpt:**
> Slides over any pole and turns your gaslight into an antique treasure. At dusk, off at dawn! This 2 wire photo electric eye mounts to any pole.
> Low Voltage Inverted Bulb Fixture — 2 bulb fixture duplicates the appearance and illumination of a double mantle gaslight.
> Aluminum Lantern includes: 4 Glass Panes and Eagle Mount. (No internal)
> High Voltage Inverted Bulb Fixture — inverted 2 bulb fixture mounts to any gas lamp or electrical fixture.

**Copy status:** [Usable]

---

### /guide.html — Installation Guide

**Copy excerpt:**
> Outdoor Lighting Fixtures — Cast Aluminum lantern includes: 4 Glass Panes and Eagle Mount. (No internal) Works with low voltage inverted bulb fixture. Slides over most light poles for an authentic gaslight appearance.
> For a copy of our installation instructions, please view our printable installation guides: HIGH VOLTAGE — 115 VOLT UNIT WITH PHOTO ELECTRIC. LOW VOLTAGE — WITH TRANSFORMER AND PHOTO ELECTRIC.
> (Guides are Legal 8.5 x 14 BUT will print on Letter size)

**Copy status:** [usable]

---

## Redirect Map Summary (MIGR-02)

See `public/_redirects` for the full file. Decisions per CONTEXT.md:
- `/kits.html`, `/bulbs.html`, `/components.html`, `/mail_order.html` → `/products`
- `/guide.html` → `/install-guides`
- `/faq.html` → `/faq`
- `/contact_us.html` → `/contact`
- `/benefits.html` → `/` (v2 candidate, no v1 equivalent)
- `/sitemap.html` → `/` (HTML sitemap replaced by XML sitemap in Phase 5)
- `/index.html` → `/` (belt-and-suspenders)

---

## Notes and Observations

1. **Old site is a late-1990s/early-2000s table-layout site** built with Dreamweaver (evident from JS patterns in HTML). All images are GIFs sized for dial-up era (under 10KB each). Very likely all product photos will need replacement.

2. **Product image count:**
   - kits.html: 3 product images
   - bulbs.html: 3 product images
   - components.html: 6 product images
   - Total: 12 product photos to evaluate

3. **No additional URLs discovered** beyond the 10 known pages. No product subpages (e.g., `/kit-1.html`) found — the catalog structure is flat.

4. **Domain registered since 1999** — long-standing domain with good SEO history. The 301 redirects in Phase 5 will preserve link equity.

5. **Name servers are AWS Route 53** (AWSDNS-*) — domain is managed through Amazon Registrar / Route 53. DNS cutover in Phase 5 will happen here.
