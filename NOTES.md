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

## 2026-10-01 — In & Out What's Included rebuilt from the correct base

File: `In & Out Deep Clean Whats Included.txt`

The first upload was the old Full Valet block. Rebuilt from the user's
In & Out draft (blue add-on styling, sealant card, 3–5 hr FAQ) and brought
in line with the questionnaire:

- **No vehicle sizes or size tables.** Size and exact price live in the
  questionnaire above; this block shows "from" prices only.
- **Seats follow the questionnaire.** Mats, carpets & boot extraction is
  included; seat treatment is an add-on (+£30 cloth, +£40 leather /
  alcantara). The draft said seats were included with stains pre-treated.
- **Sealant** +£60, clay included (+£30 if clay already paid). Removed
  "Glaco included" and "becomes a Protection Detail at the same price" —
  the questionnaire doesn't do either; Glaco is a separate £30 extra.
- **Ceramic coating** 1 / 3 / 7 years from +£120, replaces the wax, clay +
  front windows included, by request ("Request This Booking").
- **Added** (all blue, all from the questionnaire): seat treatment card +
  Seats section (treatment, protection from +£100 / +£150), Extras section
  (anti-fog £15, front windows £30, engine bay £30, ultrasonic odour £40,
  sand £50, pet hair £50).
- Add-on prices, section headings and accordions all use the blue
  add-on styling.
- Section id is `#WhatIncludedInAndOut` (the questionnaire's "See what's
  included" button points here); Book button → `#priceFinderInAndOut`.
- Removed the "How is it different from the Protection Detail?" FAQ — it
  relied on the same-price claim.
- Seat card and sealant card have no photo yet (plain panel; comment in
  each card shows where to put an image link).

---

## 2026-10-01 — Protection Detail: on-page calendar + What's Included

### Questionnaire (`Protection Detail Questionnaire.txt`)
Was the old setup: "View Calendar" sent people to Acuity's own page using
12 combined appointment types, and the engine bay went in an intake field
whose ID was never filled in (so it never reached the booking). Now books
like Exterior Detail / In & Out: calendar on the page → details → 10%
Stripe deposit → Make creates the booking. Coatings still go to
"Request This Booking".

| Acuity | ID | Page price |
|---|---|---|
| Protection Detail 3-door (base) | 93617563 | £210 |
| Protection Detail 5-door (base) | 98922355 | £230 |
| Protection Detail 7-seater (base) | 98922374 | £250 |
| Engine bay add-on | 7307199 | £30 |
| CQuartz leather, 3/5-door | 7336744 | £150 |
| CQuartz leather, 7-seater | 7336746 | £200 |
| Paint enhancement 3 / 5 / 7 (Protection Detail's own) | 7344401 / 7344405 / 7344403 | £185 / £220 / £265 |

- Deposit follows the discount code, and shows pence (£74.50, not £75).
- Tested with the webhooks stubbed: 7-seater + all add-ons → type
  98922374, add-ons [7307199, 7336746, 7334385], deposit 7450p on £745;
  with AUTUMN20 → £59.60 deposit, £536.40 balance; 3-door + 3-yr coating →
  £350, request form, no diary call.
- The 11 old combined types (93617603, 93617618, 93617631, 93617649,
  93617654, 97061113, 97061160, 97061263, 97061292, 97061395, 97061426)
  are no longer used — hide them in Acuity, don't delete.

### What's Included (`Protection Detail - Whats included.txt`)
- Removed the 3 vehicle-size price tables ("from" prices + "exact price
  in the questionnaire above").
- Add-ons in blue (prices, heading, accordion), as on In & Out.
- FAQ fixes: In & Out difference (seats are an add-on there); coatings
  mention "Request This Booking".

---

## 2026-10-01 — Exterior Detail deposit follows discount codes

File: `Exterior Detail Questionnaire.txt`. The 10% deposit, the balance
line and the booking note used the full price, ignoring a discount code.
Now use the discounted total, and the deposit shows pence. Tested:
£420 with AUTUMN20 → £336, deposit £33.60 (was £42), balance £302.40.

---

## 2026-10-01 — Interior Deep Clean & Maintenance Wash deposits

- **Interior Deep Clean:** `depositAmount()` was defined twice and the
  second, discount-blind one won, so the deposit was on the full price.
  Removed it; balance line uses the discounted total; deposit shows pence.
  Tested: £130 with AUTUMN20 → £104, deposit £10.40 (was £13).
- **Maintenance Wash:** deposit was already right (`calcTotal()` includes
  the plan / promo discount). Only the display rounded to whole pounds —
  now shows pence. Tested: CLEAN30 → £70, deposit £7, code passed on.

All five booking pages (Maintenance Wash, Exterior Detail, Interior Deep
Clean, In & Out, Protection Detail) now take the 10% deposit on the
discounted total.

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
- **Only 4 add-ons are checked for free slots** (`slice(0,4)` in
  `addonParams()` on Exterior Detail, Interior, In & Out, Maintenance Wash
  and now Protection Detail). Needs the Make availability scenario to
  accept more first. User has this noted.
- **Odd vehicle types on In & Out.** "Van, pick-up, minibus or camper" goes
  straight to a WhatsApp quote message, not the Acuity quote slot that
  Exterior Detail uses.
