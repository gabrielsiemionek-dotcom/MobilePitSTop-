# MobilePitStop website — working notes

Code for the service pages on mobilepitstop.uk. Each `.txt` file is the
HTML/CSS/JS pasted into one page block on the site. "Questionnaire" files
are the price finders; "Whats Included" files are the content below them.

---

## 2026-10-01 — In & Out Deep Clean questionnaire fixed

File: `In & Out Deep Clean Questionnaire.txt`

### Ceramic coating now works like Exterior Detail
- Question 6 offers 6-month sealant, ceramic coating **1 / 3 / 7 years**,
  or no add-on.
- Picking a coating carries on to the extras (question 7). The
  "Front Windows Coating" extra is hidden because the coating includes it.
- The final button reads **Request This Booking**. It opens the
  WhatsApp / email request with the full quote filled in. The deposit box
  is hidden and a dry-weather note is shown instead.
- Choosing sealant or "No add-on" (or changing an earlier answer) clears
  the coating, and the button goes back to **View Calendar**.

### Bugs fixed
1. A stray `</div>` after the coating card broke the page structure, so
   the extras and final screens fell outside the questionnaire.
2. The request panel (`#iodRequest`) was never closed, so the calendar was
   inside it. The panel is hidden until a coating is chosen, so the
   calendar never showed.
3. Leftover booking CSS copied from Exterior Detail kept only that page's
   step 5 visible, which hid this page's booking step (step 7) when the
   calendar opened. Removed; the correct copy was already further down.
4. `depositAmount()` was defined twice; the second ignored the discount
   code, so the 10% deposit was on the full price. The duplicate is
   removed and the "remaining balance" text uses the discounted total.
5. The Stripe deposit label sent a literal `&amp;` and never listed
   add-ons. It now reads "In & Out Deep Clean" and lists seat protection,
   clay / enhancement and sealant.
6. Analytics sent `odour: undefined`; now sends the real odour extra.
7. Header comment described Interior Deep Clean; rewritten for In & Out.

### Checked in a browser
- 3-door, leather seats treated, clay, 1-year coating → **£300**, button
  "Request This Booking", request form opens with the quote.
- Same without coating → **£210**, "View Calendar", calendar opens.

---

## Confirmed base prices

| Service             | 3-door | 5-door | 7-seater |
|---------------------|--------|--------|----------|
| Interior Deep Clean | £90    | £100   | £120     |
| In & Out Deep Clean | £140   | £160   | £180     |

Both questionnaires already charge these. Interior Deep Clean's code is
correct as it stands.

### In & Out price references brought in line (2026-10-01)
- Script comment above `BASE_PRICES` said £100 / £110 / £120 → now
  £140 / £160 / £180.
- `All Services.txt` In & Out card said £140–170 → now **£140–180**.
- Already correct: header comment, size pills (£140 / £160 / £180),
  "From £140" starting total.

---

## 2026-10-01 — In & Out What's Included rewritten to match the questionnaire

File: `In & Out Deep Clean Whats Included.txt`

The block was still the old **Full Valet** page (ozone, shampoo as an
add-on, a 1-year-only coating at £325+, "49 reviews"). Same layout, images
and wheel slider; the content now follows the questionnaire:

- **Title / wording:** In & Out Deep Clean throughout; base price table
  £140 / £160 / £180 (+ vans etc. by quote).
- **Included cards:** exterior detail, 8-week wax, and steam + mats,
  carpets & boot shampooed and extracted (included, not an add-on).
- **Add-on cards:** seat treatment, clay, paint enhancement, ceramic
  coating 1/3/7 years (from +£120).
- **Accordions:** Exterior (included), Interior (included), Seats
  (treatment +£30/£40 by fabric; protection £100/£150 cloth, £150/£200
  leather/alcantara), Paintwork & Protection (clay, enhancement, 6-month
  sealant, ceramic coating 1/3/7 table), Extras (anti-fog, front windows,
  engine bay, ultrasonic odour, sand, pet hair).
- **FAQs** rewritten for In & Out, including why coatings are a request.
- Review count 81, and it follows the sitewide `MPS_REVIEWS` like the
  questionnaire badge.
- **"See what's included" now works.** The questionnaire button pointed at
  `#WhatIncludedInteriorDeepClean`, which isn't on this page. The section
  id is now `#WhatIncludedInAndOut` and the button points there. The
  "Book" button goes to `#priceFinderInAndOut` (was the old Full Valet id).

If a price changes in the questionnaire, change it here too — the list of
prices is in the comment at the top of the file.

---

## Open issues (not fixed yet)

- **Two Exterior Detail files.** `Exterior Detail Questionnaire.txt` is the
  live one. `Exterior Detail.txt` is an older version and could be deleted.
- **Interior Deep Clean header comment is stale.** The prices in the code
  (£90 / £100 / £120) are correct. The comment at the top of the file
  describes a different plan (£100 / £120 / £140, 48 Acuity types, an
  `ACUITY_TYPE_MAP` that doesn't exist) and should be rewritten to match
  the code. Nothing on the page is affected.
- **All Services page — to be updated later.** The Interior Deep Clean and
  Exterior Detail cards both say "from £60", but those pages start at £90
  and £70.
- **Odd vehicle types on In & Out.** "Van, pick-up, minibus or camper" goes
  straight to a WhatsApp quote message, not the Acuity quote slot that
  Exterior Detail uses.
