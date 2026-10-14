# MobilePitStop: onboarding research (Nearside test client)

Started 14 Oct 2026. MobilePitStop is Gabriel's own business, taken on as a Nearside client to test onboarding (hq/ONBOARDING.md, Path B: it already has a decent website).

- **Package:** Full, without Google Ads. **£0** (own business), so no Stripe checkout.
- **Decided 14 Oct:** rebuild on `client-site-template` as a hidden preview (noindex, workers.dev), with **the same page addresses** as today. The domain only moves when Gabriel says so.
- **Hands off:** the blog routines, the Make scenarios (including the booking webhooks the questionnaires call) and their secrets. The rebuild keeps calling the same webhooks, unchanged.

Sources, all checked 14 Oct 2026: the live site (all 54 pages in the sitemap, plus `/`), the code in this repo, and the public Google listing (Maps, plus one live Places lookup with nothing stored except the place ID). The profile's secondary categories and services list are only visible in the dashboard: **waiting on Gabriel's screenshots**.

---

## 1. Business facts

| Fact | Website | Google profile | Status |
|---|---|---|---|
| Name | "MobilePitStop - Valeting & Detailing" (schema); Squarespace's own schema block says "MobilePitStop" | "MobilePitStop - Valeting & Detailing" | Match (drop the second schema block in the rebuild) |
| Phone | 07592 196929 | 07592 196929 | Match |
| Email | mobilepitstopvalet@gmail.com | — | OK |
| Address | Town only (Ellesmere Port, Cheshire) | A street address in Ellesmere Port, **shown publicly** | **Customers can't visit it** (Gabriel, 14 Oct). Google's rules: hide it and list the service area. Street addresses stay out of this public repo |
| Base town | Ellesmere Port, everywhere (copy, schema, "20 miles of Ellesmere Port") | Ellesmere Port | **Moving to Runcorn** (Gabriel, 14 Oct). Site, profile, blog and directories all change with it; the address stays hidden |
| Second base | "Our unit in Birkenhead" (window tinting, PPF, wraps, de-chrome, paint correction); About page also says "a fully equipped garage base" | Not on the profile | **Decide:** is the unit a real customer-facing address? If so, it may need its own mention or profile |
| Hours | Schema: Mon–Sat 06:00–22:00 | **Every day** 06:00–22:00 | **Every day** (Gabriel, 14 Oct): the site changes to match |
| Service area | "Roughly 20 miles of Ellesmere Port, travel free within it" | Not checked (dashboard) | Several town pages are well outside 20 miles (see §4) |
| Reviews | 5.0, 84 (from `window.MPS_REVIEWS`, header injection) | 5.0, 84 | Match |
| Primary category | — | **Valeting service** | Home page heading should be "…in Ellesmere Port" (§2) |
| Place ID | — | `ChIJ6Ujg3qTXJ0wRzLSz0LEaxtg` (Maps CID 15620201708594115788) | Use for the map and the reviews link |
| Website link | — | https://mobilepitstop.uk/ | OK |
| Booking link | — | Not checked (dashboard) | QA.md §3: should go to the same place as the site's Book buttons |
| Social | Instagram, Facebook, TikTok, YouTube (`sameAs`) | — | OK |

## 2. Google profile ↔ website map (ONBOARDING.md §5.1)

**Fill in from the dashboard screenshots** (Edit profile → Business category; Edit services).

| Profile | Name on the profile | Website page | Status |
|---|---|---|---|
| Primary category | Valeting service | `/` (home) | Home heading has no town ("…In Cheshire, Merseyside & Flintshire"). Rebuild: "Mobile valeting and detailing in Ellesmere Port" |
| Secondary categories | _pending_ | | |
| Services | _pending_ | | |

**Website services (each has its own page today), to match against the profile list:**

| Website service | Page |
|---|---|
| Maintenance Wash | `/maintenance-wash` |
| Exterior Detail | `/exterior-detail` |
| Interior Deep Clean | `/interior-deep-clean` |
| In & Out Deep Clean | `/in-out-deep-clean` |
| Protection Detail | `/protection-detail` |
| Enhancement Polish / Machine Polishing (and the paint correction quote) | `/machine-polishing` |
| Ceramic Coating | `/ceramic-coating` |
| Odour Removal Treatment | `/cigarette/milk/bio-odour-removal` |
| Window Tinting (unit) | `/window-tinting` |
| Detailing Maintenance Plan | `/car-detailing-maintenance-plan` |
| De-chrome (unit, by quote) | `/de-chrome`, not live (redirects to `/window-tinting`) |
| PPF, vinyl wraps (unit, by quote) | No page; mentioned in the home FAQ and schema |

Rules (§5.1): same names on both; add a profile category only if it's real work; a distinct service gets its own page (the Maps test). Recheck this table every month.

## 3. Services and prices (from the live questionnaire code, 14 Oct)

Sizes: 3-door / 5-door / 7-seater. The code is the source of truth; Acuity is the second (QA.md §3).

| Service | Price | Time (home FAQ) |
|---|---|---|
| Maintenance Wash | £60 / £70 / £80 | ~75 min |
| Exterior Detail | £70 / £80 / £90 | ~1½ h |
| Interior Deep Clean | £90 / £100 / £120 | 3–5 h |
| In & Out Deep Clean | £140 / £160 / £180 | 3–5 h |
| Protection Detail | £210 / £230 / £250 | 4–6 h |
| Enhancement Polish (1-stage, clay included) | £285 / £330 / £385 | a day |
| Multi-stage paint correction | from £600 / £650 / £700, by quote | — |
| Ceramic Coating (exterior prep + coating) | 1 yr £190 / £230 / £290 · 3 yr £270 / £310 / £370 · 7 yr £370 / £410 / £470 | most of a day |
| Odour Removal Treatment (4 stages) | £250 / £270 / £300 | — |
| Window tinting, PPF, wraps, de-chrome | by quote (unit in Birkenhead) | — |
| Maintenance Plan | after a deep clean: 30% off weekly, 25% every 2–3 weeks, 20% every 4–6 weeks; free chemical decon every visit; no contract | "From £42 on the plan" |

Deposits are taken by Stripe through Make on 7 service pages (10%, discount codes respected).

**Wrong prices live today** (fix in the rebuild, or sooner on Squarespace):
- **Crewe** and **Knutsford** pages still show the old menu: "Mini Valet from £50", "Full Valet from £100", "Interior Deep Clean from £60", "Exterior Deep Clean from £60", "Full Valet Premium from £180", "Odour Removal from £120", "1-Stage Machine Polish from £275", "Ceramic Coating 1 year from £385 / £325".
- **All Services** add-on prices to re-check against the questionnaires: "Heavy sand +£50" (questionnaire +£40), "Glaco +£15" and "+£30" both appear.
- **About** says "Mini Valets" (old name) and "eco-friendly products" (keep only if true).

## 4. Pages and the address plan for the rebuild

All current addresses stay. The only changes are the town pages and one duplicate.

| Group | Pages | Plan |
|---|---|---|
| Home | `/` | Rebuild. `/mobile-valeting-detailing-service` is a copy of the home page (canonical already points home): **301 → `/`** |
| Services | the 10 live service pages in §2 | Rebuild at the same addresses; questionnaires carried over unchanged |
| All Services | `/all-services` | Rebuild (template services menu) |
| Info | `/about`, `/contact`, `/faqs`, `/reviews`, `/portfolio-build-trust` | Rebuild at the same addresses |
| Legal | `/privacy-policy-cookie-policy`, `/terms-conditions` | Carry over the text (add meta descriptions) |
| Article | `/why-hand-car-washes-scratch-your-paint` | Carry over (the blog itself stays at blog.mobilepitstop.uk) |
| Booking | `/booking-confirmed` (not in the sitemap) | Carry over unchanged (GA4 purchase event) |
| County pages | `/mobile-valeting-service-cheshire`, `-merseyside`, `-flintshire` | Keep as area hubs, rewritten with real facts |
| Town pages | 30 more `/mobile-valeting-service-<town>` | **Keep up to 7** that Gabriel can give real facts for; **301 the rest to their county page** |

**Why the town pages change:** 31 of the 33 area pages are 97–99% the same text with the town name swapped (measured 14 Oct; only Crewe and Knutsford differ, and those two carry old prices). QA.md §7 and CONTENT-QUALITY.md §4 rule out name-swapped copies, and Google treats them as scaled content.

**Distance check:** the site promises free travel within about 20 miles of Ellesmere Port. Crewe, Nantwich, Congleton, Macclesfield, Wilmslow, Knutsford, Southport and Formby are roughly 25–40 miles away by road (estimates; check the real drive). Either drop them as pages or say plainly what travel costs there.

Town pages today, by county (from the home page's area list):
- **Cheshire:** Ellesmere Port, Chester, Runcorn, Northwich, Crewe, Knutsford, Wilmslow, Macclesfield, Congleton, Nantwich
- **Merseyside:** Liverpool, Birkenhead, Wallasey, St Helens, Prescot, Formby, Southport, Bootle, Huyton, Kirkby
- **Flintshire:** Mold, Flint, Connah's Quay, Buckley, Holywell, Hawarden, Queensferry, Shotton, Bagillt, Caergwrle

**A town page earns its place with** (per town): real jobs done there (with photos), road distance and drive time from Ellesmere Port, the usual parking and power set-up (driveways, flats, workplaces), and anything local that changes the job (e.g. coastal salt in Wallasey or Formby, tree sap in leafy roads).

**Package note:** the Full package covers "up to 10 genuine service/area pages". MobilePitStop already has 10 service pages, so it's over the cap before any areas. As our own business we keep them; for hq: existing clients may arrive with more pages than the cap.

## 5. Audit against `client-site-template/QA.md` (live site, 14 Oct)

| QA section | Result |
|---|---|
| 1. Phone first | **Pass:** no sideways scroll on any of the 54 pages at 375px (scroll width 371). Burger menu on every page; "Mobile Services" and "Unit Services" submenus. To eyeball in the rebuild: the area map iframe is wider than the screen on town pages (cropped, not scrolling) |
| 2. Photos | **Pass:** 36 images scanned, no baked-in bands (only the transparent logo and the area graphic have padding, by design). 15 images have no alt text (home 4, About 3, Interior, In & Out, Exterior, Maintenance Plan 1 each) |
| 3. Prices and booking | **Mostly pass:** every mobile service is priced; tinting/PPF/wraps by quote. Acuity page `mobilepitstop.as.me` is live. **Fail:** old prices on Crewe and Knutsford (§3). Profile booking link not checked yet |
| 4. Consistency | **Fail:** the old service menu (Mini Valet, Full Valet, Full Valet Premium) survives on two town pages and the About page |
| 5. Email | OK: one address everywhere; contact is phone, WhatsApp and email (no form) |
| 6. Domain and deploy | For later. DNS today: Squarespace/Google Cloud DNS nameservers, A records to Squarespace, `blog` CNAME to GitHub Pages, SPF `v=spf1 -all`, **no MX** (email is Gmail). Copy all of these before any nameserver switch |
| 7. Local SEO | Home heading has no town (fail); Google map is a general area embed, listing link uses the CID (rebuild: place ID); `geo` only on the home page; reviews via Elfsight, not live Google reviews with attribution; services submenu present; town pages fail (§4) |
| 8. Keys | No Google API key on the site today |

Other: Privacy and Terms have no meta description; Squarespace adds its own LocalBusiness/Organization schema next to ours.

## 6. AI-ready (CONTENT-QUALITY.md §8, PLAN.md "AI-ready websites")

| Rule | Today | Rebuild |
|---|---|---|
| Crawlers welcome | Pass: robots.txt allows GPTBot, ClaudeBot, Google-Extended and others | Keep |
| `llms.txt` | **Missing** (Squarespace can't host root files) | Add |
| Structured data on every page | Pass, but two business blocks with different names; `geo` on home only | One business block from site.json on every page, with `geo` and place ID |
| Facts in plain text | Pass: prices, times and areas are text on service pages and the home FAQ | Keep |
| One question per article, answer first | Blog is separate (routines, hands off) | Service and area pages follow it |
| Consistent everywhere | Fail: hours, address, old prices (§1, §3) | Fixed by §1 decisions |

## 7. Carry over unchanged in the rebuild

- **GTM** `GTM-MMHW32R2`, Consent Mode v2 defaults and the cookie banner (`mobilepitstop-code-injection-HEADER.html`, `-FOOTER.html`).
- **`window.MPS_REVIEWS`** (review count in one place) until live reviews replace it.
- **Questionnaires** (`* Questionnaire.txt`, `* Request.txt`, `Odour Removal Treatment.txt`): Acuity appointment-type IDs, Stripe deposits, discount codes, dataLayer events, and the **Make webhooks they call (same URLs, not edited)**.
- **`/booking-confirmed`** (`mobilepitstop-booking-confirmed.html`): GA4 `purchase` from `?amt=` / `?dep=`.
- **Old-address redirects** set in Squarespace: `/full-valet` → `/in-out-deep-clean`, `/full-valet-premium` → `/protection-detail`, `/smoke/milk/bio-odour-removal` → `/cigarette/milk/bio-odour-removal`, `/de-chrome` → `/window-tinting`.
- **Blog** stays at blog.mobilepitstop.uk (GitHub Pages); the nav links to it.

## 8. Open questions for Gabriel

1. **Profile screenshots:** Business category (primary + secondary) and the Services list; also the booking link and description.
2. ~~Address~~: customers can't visit (14 Oct). Still open: is the Birkenhead unit somewhere customers drop cars off?
3. ~~Hours~~: every day (14 Oct).
4. ~~New base town~~: Runcorn (14 Oct). Still open: the free-travel distance from it.
5. **Towns:** "keep the main big ones" (14 Oct). Proposed, nearest first: Runcorn, Widnes, Warrington, Northwich, Ellesmere Port, Chester, St Helens, Liverpool. Widnes and Warrington have no page today, so they'd be new pages and need real jobs to write about. The other 25 town pages redirect to their county page. **To confirm.**
6. **Travel** beyond the free radius: to be confirmed.
7. **Directories:** where MobilePitStop is listed today (Yell, Bing Places, Apple, Facebook, Checkatrade...), so every listing moves to the new base.
