# Domain Pitfalls

**Domain:** Small e-commerce site rebuild (static/modern stack, PayPal, under 10 SKUs)
**Project:** Gaslight Conversions — gaslightconversions.com
**Researched:** 2026-02-23
**Confidence:** MEDIUM — based on established domain knowledge; live source verification was unavailable during this research session. Findings are well-established patterns, not speculative.

---

## Critical Pitfalls

Mistakes that cause rewrites, lost revenue, or search ranking damage.

---

### Pitfall 1: Failing to Audit and Redirect All Existing URLs Before Launch

**What goes wrong:** The old site has URLs that are indexed by Google and bookmarked by repeat customers. When the new site launches with different URL structure (e.g., `/products/gaslight-base-unit.html` becomes `/products/gaslight-base-unit`), every old URL returns a 404. Google drops the page rankings accumulated over years. Customers who bookmarked product pages hit dead ends.

**Why it happens:** Rebuilders focus on the new site structure and forget the old site has an existing link graph. The assumption is "it's a small site, it won't matter" — but even 10 pages of indexed content carry SEO equity worth preserving.

**Consequences:**
- Search rankings drop for terms like "gas lamp to electric conversion kit" that the site may have held for years
- Inbound links from forums, home improvement sites, or commercial supplier directories break silently
- Google Search Console surfaces crawl errors, but only after the damage is done

**Prevention:**
1. Before touching the new site, crawl the old site with a tool (Screaming Frog free tier handles small sites) and export all URLs
2. Map every old URL to its new equivalent in a redirect table
3. Implement 301 redirects at the server/host level (Netlify `_redirects` file, Vercel `vercel.json`, or `.htaccess` on Apache)
4. After launch, verify redirects are working before DNS cutover completes

**Detection (warning signs):**
- You realize the old site used `.html` extensions and the new one doesn't, with no redirect plan
- No one has crawled the old site before work began
- The deploy plan jumps straight from "build" to "DNS swap" without a redirect verification step

**Phase mapping:** Address in the pre-launch / deployment phase. Redirect table should be built during content migration, not after.

---

### Pitfall 2: PayPal Button Code Is Tied to Deprecated Button API

**What goes wrong:** The old site uses PayPal's legacy "Payments Standard" button code generated from the PayPal merchant dashboard circa 2005–2015. This is `<form>` tag-based HTML with a `https://www.paypal.com/cgi-bin/webscr` action. PayPal has been deprecating Payments Standard in favor of PayPal Checkout (JS SDK). If the old button code is copied directly into the new site, it may work today but could break without warning when PayPal fully sunsets the endpoint.

**Why it happens:** "It worked before, just copy it" is the path of least resistance. The merchant dashboard still generates this code for accounts that were using it historically.

**Consequences:**
- Checkout silently stops working after a PayPal API deprecation
- Revenue stops; the owner may not notice for days if traffic is low
- Fixing it mid-flight is stressful and requires touching live production code

**Prevention:**
1. Log into the PayPal merchant dashboard and check which button generation API is being used
2. If the existing buttons are legacy Payments Standard (`webscr`), rebuild them using the current PayPal Checkout JS SDK (`https://www.paypal.com/sdk/js`) during the rebuild — not after
3. Test each product's buy button in PayPal's sandbox environment before going live
4. Confirm the merchant account email on every button matches the active PayPal account

**Detection (warning signs):**
- Old site's button HTML contains `<form action="https://www.paypal.com/cgi-bin/webscr">`
- No one has tested checkout on the new site since copying over the button code
- Button HTML was copy-pasted without checking PayPal's current developer docs

**Phase mapping:** Address during the product pages phase. Every buy button should be re-generated or re-implemented from current SDK, not copied from old source.

---

### Pitfall 3: Losing the Domain's Search Authority During DNS Cutover

**What goes wrong:** The domain gaslightconversions.com has accumulated authority over many years (likely since the late 1990s). If the cutover is done carelessly — such as parking the domain, letting it lapse briefly, or pointing it to a new host before the new site is ready — Google can interpret this as a site change and re-evaluate the domain's trust score.

**Why it happens:** Domain registration renewals get missed. DNS propagation is misunderstood. A gap where the domain returns errors or a blank page signals abandonment.

**Consequences:**
- Years of domain age and backlink equity devalued
- Rankings for niche terms like "gas lamp conversion kit" take months to recover
- The site re-enters a "sandbox" period where rankings temporarily drop after major changes

**Prevention:**
1. Ensure domain registration is paid and current well before the rebuild begins
2. Keep the old site live until the new site is fully deployed, tested, and redirect-verified
3. Use a staging URL (e.g., `staging.gaslightconversions.com` or a host preview URL) for development — never the live domain
4. DNS cutover should be the very last step, not done mid-build

**Detection (warning signs):**
- Domain renewal is coming up during the rebuild window
- Development is happening on the live domain with "under construction" pages
- No staging environment exists

**Phase mapping:** Address in project setup/infrastructure phase. Staging environment and domain health check should be the first deliverables.

---

### Pitfall 4: Rebuilding the Design Without Auditing What Content Actually Exists

**What goes wrong:** The rebuilder designs and builds the new site based on assumptions about what content exists, then discovers the actual content is missing, outdated, or of unusable quality. Product photos are low-resolution JPEGs from 2001. Installation guide text is buried in a PDF that can't be edited. The "About" page has no story worth telling.

**Why it happens:** Design work starts before content inventory. The classic "we'll fill in the real content later" mistake — except for a small family business, "later" may never arrive.

**Consequences:**
- Launch is blocked waiting on content that hasn't been created yet
- Product pages go live with placeholder text or missing photos, hurting conversion
- The modern design is undercut by the old content it's wrapping

**Prevention:**
1. Before any design work, inventory all existing content: pages, product descriptions, photos, guides, copy
2. Flag every asset that needs to be remade (new product photos, rewritten descriptions)
3. Agree on who produces missing content and by when — this is often the longest lead-time item
4. Product photos in particular: source hi-res versions or plan a reshoot before design decisions are locked

**Detection (warning signs):**
- No content inventory has been done
- Product photos haven't been evaluated for quality yet
- "We'll write the descriptions later" has been said

**Phase mapping:** Address before design begins. Content audit is a prerequisite to design, not a parallel track.

---

## Moderate Pitfalls

---

### Pitfall 5: Mobile Layout That "Passes" But Doesn't Convert

**What goes wrong:** The site is technically responsive — it doesn't break on mobile — but the mobile experience is poor. Navigation is hard to tap, product images are small, and PayPal buttons are too close to the edge of the screen. The site passes a "mobile-friendly" Google test but customers on phones abandon before purchasing.

**Why it happens:** Responsive design is often tested by resizing a desktop browser, not on real devices. Tap target sizes, thumb reach zones, and reading flow on small screens aren't the same as a scaled-down desktop layout.

**Prevention:**
- Test on real phones (iPhone and Android), not just browser DevTools
- PayPal buy buttons should be large, full-width on mobile with at least 44px tap height
- Navigation should work with one thumb; avoid hover-dependent menus
- Test the complete purchase flow on mobile before launch, not just visual appearance

**Detection (warning signs):**
- Mobile testing has only been done in browser DevTools
- Buy buttons haven't been tested on a physical phone
- No one has completed a test purchase on mobile

**Phase mapping:** Mobile testing should be a formal checklist item in the QA/pre-launch phase.

---

### Pitfall 6: Page Titles and Meta Descriptions Reset to Defaults

**What goes wrong:** The old site, even with its late-90s HTML, likely has page titles that have been indexed and are serving as the click text in Google results. When the new site launches with generic titles like "Products | Gaslight Conversions" instead of "Gas Lamp to Electric Conversion Kits | Gaslight Conversions," click-through rates from search drop.

**Why it happens:** SEO metadata is a detail-level concern that gets deprioritized during design and build. Developers set up the template first and leave metadata as a "content task."

**Prevention:**
- Audit the old site's existing page titles and meta descriptions before rebuilding
- Carry forward any titles that are already performing (check Google Search Console if the account is claimed)
- Every product page and content page should have a unique, descriptive title tag before launch

**Detection (warning signs):**
- No one has checked what Google currently shows as the title for key pages
- The new site's page titles are all following a generic template without page-specific copy

**Phase mapping:** Address during content migration. SEO metadata is content work, not an afterthought.

---

### Pitfall 7: Contact Form Sends to an Email That Isn't Monitored

**What goes wrong:** The contact form is built and works, but form submissions go to an email address that isn't actively checked. Customer inquiries about products or orders go unanswered for days.

**Why it happens:** The form is wired to a legacy email or a catch-all that gets filtered to spam. No one verified the delivery during setup.

**Prevention:**
- Confirm the destination email address before launch and send a test submission
- Use a simple, well-maintained transactional email service (Formspree, Netlify Forms, or similar) rather than direct SMTP
- Set up a notification rule so the email doesn't land silently in a folder

**Detection (warning signs):**
- No test submission has been sent from the live form
- The destination email hasn't been confirmed with the business owner

**Phase mapping:** Address in the contact/forms phase. Form delivery verification is a launch blocker.

---

### Pitfall 8: PayPal Sandbox vs. Live Credentials Confusion at Launch

**What goes wrong:** The site is built using PayPal sandbox credentials for testing. At launch, the sandbox credentials are left in place (or worse: the site ships to production with test mode active). Real purchases appear to succeed from the customer's perspective but no money changes hands. Or the inverse: live credentials are used during development and real charges are processed during testing.

**Why it happens:** The distinction between sandbox and live PayPal environments is easy to lose track of when juggling a build. Button code generated from the sandbox dashboard looks identical to live button code.

**Prevention:**
- Maintain a clear `.env` variable or configuration flag that controls sandbox vs. live mode
- Add a visible banner or indicator in the development environment that says "SANDBOX MODE"
- Create a pre-launch checklist item: "Verify PayPal buttons use live merchant account, not sandbox"
- Do one final test purchase with a real (small-amount) transaction before announcing the site is live

**Detection (warning signs):**
- Sandbox and live PayPal credentials are not tracked separately in configuration
- No one has done a real-money end-to-end test

**Phase mapping:** PayPal configuration management should be established in the product pages phase. Sandbox/live switchover is a launch checklist item.

---

## Minor Pitfalls

---

### Pitfall 9: Forgetting to Claim Google Search Console for the New Site Configuration

**What goes wrong:** The old site may or may not have had Google Search Console set up. After the rebuild, changes like new sitemap structure, updated robots.txt, or redirect chains benefit from being submitted directly to Google via Search Console. Without it, re-indexing is passive and slow.

**Prevention:**
- Claim the property in Google Search Console (if not already done)
- Submit the new XML sitemap after launch
- Monitor for crawl errors in the first 30 days post-launch

**Phase mapping:** Post-launch / launch checklist.

---

### Pitfall 10: Hardcoded Product Prices in Multiple Places

**What goes wrong:** Product prices are hardcoded both in the visible page copy and in the PayPal button's price parameter. When a price changes, the display price is updated but the PayPal button still charges the old amount (or vice versa). This creates real customer disputes.

**Prevention:**
- When building product pages, establish a single source of truth for price — either in a data file, a component variable, or at minimum a clearly commented section
- PayPal buy buttons encode the price in the button HTML; this must be updated in sync with any displayed price change
- Document this clearly in a "how to update a product" guide for Carolyn

**Phase mapping:** Address during product pages phase. Build with maintainability in mind from the start.

---

### Pitfall 11: Images Not Optimized, Causing Slow Load on Mobile

**What goes wrong:** Product photos are uploaded at full camera resolution (3–5MB each). The site technically works but loads slowly on mobile networks. Google's Core Web Vitals score suffers, which affects search ranking.

**Prevention:**
- All images should be exported at appropriate web resolution (1200px max width for product images)
- Use modern formats (WebP with JPEG fallback) where the build toolchain supports it
- Compress images before committing to the repo — don't rely on the browser to do this

**Phase mapping:** Address during product pages phase when images are first added.

---

## Phase-Specific Warnings

| Phase Topic | Likely Pitfall | Mitigation |
|-------------|---------------|------------|
| Project setup / infra | Domain lapses or DNS gaps during rebuild | Audit domain registration; set up staging URL immediately |
| Content migration | Content inventory skipped; design blocked later | Audit all existing pages and photos before design begins |
| Product pages | PayPal legacy button code copied forward | Regenerate buttons from current SDK; test in sandbox first |
| Product pages | Hardcoded prices in two places | Establish single source of truth for price in each product |
| Product pages | Images too large | Compress and resize before committing |
| Design / CSS | Mobile layout not tested on real devices | Physical device testing before sign-off |
| SEO setup | Old URLs not redirected | Build redirect table from old-site crawl; implement 301s |
| SEO setup | Page titles reset to generic | Audit old titles; write unique titles per page before launch |
| Forms | Contact form email not verified | Send test submission to confirm delivery |
| Pre-launch QA | PayPal sandbox credentials left in production | Checklist item: verify live credentials, do real test purchase |
| Post-launch | Google not notified of new sitemap | Submit sitemap via Search Console within first week |

---

## Sources

Note: Live web source verification was unavailable during this research session. Findings are drawn from:
- Established Google Search Console documentation patterns (301 redirect behavior, sitemap submission, Search Console crawl errors)
- PayPal developer documentation patterns (Payments Standard deprecation timeline, JS SDK migration)
- Core Web Vitals / Google Lighthouse criteria for image optimization and mobile usability
- Well-documented industry patterns for site migration SEO (confidence: MEDIUM — these are widely agreed-upon patterns, not speculative)

Recommend verifying PayPal's current deprecation status for Payments Standard at https://developer.paypal.com/docs/ before the product pages phase begins, as deprecation timelines shift.
