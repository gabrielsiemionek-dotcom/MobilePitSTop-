# MobilePitStop: go-live checklist (Nearside onboarding test)

The rebuild on `client-site-template` replaces the Squarespace site at the same addresses. Background and facts: `research.md`, `facts.json`. **Don't cancel anything at Squarespace until step 7.**

## 1. Answers from Gabriel (research.md §8)
- [x] Google profile read from the dashboard (14 Oct, research.md §2).
- [x] Address: customers can't visit → hidden on the profile, service area only (14 Oct).
- [ ] Is the Birkenhead unit customer-facing?
- [x] Sundays: open every day (14 Oct).
- [x] New base town: Runcorn (14 Oct). Free-travel distance from it still to confirm.
- [ ] The main big towns to keep, from the new base. The rest redirect to their county page.
- [ ] List of directories MobilePitStop is on, to move to the new base.
- [ ] Travel beyond 20 miles: free, a charge, or not covered?

## 2. Google profile fixes (Gabriel approves each change)
- [ ] Hours: every day 06:00–22:00 on both.
- [ ] Base moved to the new town (Google may ask to re-verify).
- [ ] Address shown or hidden as decided; service area set to the towns and counties on the site.
- [ ] Categories and services named exactly as the site's service pages (ONBOARDING.md §5.1), real work only.
- [ ] Booking link goes where the site's Book buttons go.
- [x] The §5.1 table in `research.md` filled in (14 Oct).

## 2b. Directories (Full package: citations)
- [ ] Every listing updated to the same name, phone, website and new base town; no street address where the site hides it.

## 3. Build the preview
- [ ] Repo from `client-site-template` (where: decide at this step), hidden preview on workers.dev, `launched: false`.
- [ ] Every current address rebuilt (research.md §4); `/mobile-valeting-detailing-service` → `/`; town redirects as decided; the old Squarespace redirects kept.
- [ ] Questionnaires, GTM, consent banner and `/booking-confirmed` carried over unchanged (research.md §7). Make webhooks not edited.
- [ ] Current prices everywhere (research.md §3); old menu names gone (grep for Mini Valet, Full Valet, Full Valet Premium).
- [ ] `llms.txt`, one business schema block with `geo` and place ID, live Google reviews with attribution, map by place ID.
- [ ] `wrangler.jsonc` has a `previews` block.

## 4. QA (client-site-template/QA.md, ONBOARDING.md §5.2)
- [ ] Every page at 375px and 1280px: menu, submenus, photo→heading spacing, no sideways scroll.
- [ ] Every Book button and questionnaire opened live: the Acuity slot loads, the deposit total is right. Gabriel does any real-money test.
- [ ] Every old address returns 200 or the planned 301.
- [ ] Gabriel checks it on his phone.

## 5. Domain (Gabriel's Squarespace Domains login)
- [ ] Logged in to Cloudflare as **agency@nearsidedigital.com**.
- [ ] Zone `mobilepitstop.uk` created; **copy every record first**: Squarespace A records (`@`), `www`, `blog` CNAME → `gabrielsiemionek-dotcom.github.io` (DNS only, or the blog breaks), SPF `v=spf1 -all`, any verification TXT. There is no MX today.
- [ ] DNSSEC off at the registrar, then nameservers switched; zone active.
- [ ] Custom domain connected and `launched: true` **in the same change**.
- [ ] Zone hardening (README "Going live"): Always Use HTTPS, TLS 1.2, HSTS, /api/ rate limit, then DNSSEC.

## 6. After launch
- [ ] Search Console: sitemap resubmitted; agency@ added. GA4: agency@ added; AI-referral view (chatgpt.com, perplexity.ai...).
- [ ] Test a booking end to end (Gabriel).
- [ ] Recheck the §5.1 map; add the monthly check.

## 7. After a few days of the new site working
- [ ] Cancel the Squarespace **website** plan only; keep the domain registration.
- [ ] Turn off the workers.dev preview address.
