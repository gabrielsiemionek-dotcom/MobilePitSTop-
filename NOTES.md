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

## 2026-10-01 — Ceramic Coating questionnaire + What's Included

**Prices checked — all correct**, identical to Exterior Detail with a
coating: 1 yr from £190 / £230 / £290, 3 yrs £270 / £310 / £370, 7 yrs
£370 / £410 / £470; enhancement +£185 / £220 / £265 (clay already in the
coating); engine bay +£30. Request-only page (no Acuity), same as a coating
on every other page.

Questionnaire (`Ceramic Coating Questionnarie.txt`):
- Final screen showed "To book, pay a 10% deposit" with card/cash/invoice
  before the request — hidden now; the weather note and "the 10% deposit
  is taken once it's booked" show instead (same as the other pages).
- Button starts as "Request This Booking" (the markup said View Calendar).
- Request message total ignored a discount code — now "£448 (code
  AUTUMN20, 20% off £560)", matching the breakdown; refreshes when a code
  is added or removed.
- Cleaning Power badge said "Exterior Detail" → "Ceramic Coating".
- Analytics sent `paint` / `prot` (fields this page doesn't have) → now
  `coating` / `enhancement`.
- Request panel scrolls into view when opened.

What's Included (`Ceramic Coating Whats Included.txt`):
- No vehicle sizes: the price table is now by coating length only (from
  £190 / £270 / £370) and the enhancement size table is gone; both say the
  exact price is in the questionnaire above.
- Add-ons in blue (prices, heading, accordion).
- "Why request a date" FAQ names the Request This Booking button.

---

## 2026-10-01 — Machine Polishing questionnaire + What's Included

**Prices checked — all correct.** 1-stage £285 / £330 / £385 (= Exterior
Detail + enhancement); correction from £600 / £650 / £700 by quote;
sealant +£30; coating from +£90 / £170 / £270 (3-door) — all equal to the
Exterior Detail totals for the same choices.

Questionnaire (`Machine Polishing Questionnarie.txt`):
- Was booking six old combined types (97045121, 97045202, 97045162,
  97045205, 97045167, 97045224) by sending people to Acuity's page. Now
  books on the page like Exterior Detail: calendar → 10% Stripe deposit
  (after any discount code, shown to the penny). Uses its own standalone
  Paint Enhancement types, 5 hrs each: 3-door 98943820 (£285), 5-door
  98943855 (£330), 7-seater 98943877 (£385), + sealant add-on 7344721.
- Correction, any coating, and vans etc. stay "Request This Booking";
  the request message total follows a discount code.
- Sealant card no longer claims Glaco (Exterior charges Glaco separately);
  Cleaning Power badge "Exterior Detail" → "Machine Polishing";
  analytics sent fields this page doesn't have → now level / protection.
- Tested: 7-seater + sealant → type 98943877, add-ons [7344721],
  £415, deposit £41.50; 3-door + 3-yr coating → £455, request; correction
  → from £600, request; 3-door + AUTUMN20 → £228, deposit £22.80.

What's Included (`Machine Polishing Whats Included.txt`):
- Removed the 3 vehicle-size tables ("from" prices + "exact price in the
  questionnaire above").
- Add-ons in blue; sealant no longer claims Glaco; "cheaper than anywhere
  else on the site" → "£30 less than on their own" (Protection Detail
  already includes a sealant, so the old line wasn't true).

---

## 2026-10-01 — Sealant without clay bar (7344721, £30)

When the clay is already paid for, the page charges the 6-month sealant at
£30 but used to send add-on 7334413 (£60, includes clay), so Acuity's
appointment read £30 high. Now:

| Page | Sealant chosen with | Add-on sent |
|---|---|---|
| Exterior Detail, In & Out | paint enhancement | 7344721 (£30) |
| Exterior Detail, In & Out | clay bar, or nothing | 7334413 (£60, clay included; clay add-on not sent) |
| Machine Polishing | always (the polish includes clay) | 7344721 (£30) |

Tested: Exterior 3-door enhance + sealant £315 → [7264641, 7344721];
clay + sealant £130 → [7334413]; In & Out 3-door enhance + sealant £385 →
[7264641, 7344721]; Machine Polishing 5-door + sealant £360 → 98943855 +
[7344721]. Acuity's totals now match the page in every case.

---

## 2026-10-01 — Window Tinting reviewed, ready to go live

Quote-only request form (no prices, no Acuity), at the Birkenhead unit.
`APPROVED = true`, so it shows live (not "Coming soon"). Tested: name /
car / year checks, chips into the message, WhatsApp to 07592 196929 then
the "send a copy" to the fitter (447426487900), both links between the
blocks, no overflow at 375px.

Only change: added a "Remove old tint" option — the FAQ told people to
mention it but the form had nowhere to say so.

To confirm before launch (wording, not code): the fitter's WhatsApp
number; that the film really carries a warranty and blocks 99% UV; the
headlight / rear-light tint wording (smoked lights are generally not road
legal in the UK).

---

## 2026-10-01 — All Services page brought up to date and linked

File: `All Services.txt` (live at /all-services; /book-your-valet-now should 301 there).

Every mobile card now mirrors its questionnaire — price, what's
included, add-ons, cleaning-power dots:

| Card | Price | Link |
|---|---|---|
| Maintenance Wash | from £60 | /maintenance-wash (was /mini-valet — 404) |
| Interior Deep Clean | £90–120 (was "from £60") | /interior-deep-clean |
| Exterior Detail | £70–90 (was "from £60") | /exterior-detail (was /car-exterior-deep-clean — 404) |
| In & Out Deep Clean | £140–180 | /full-valet |
| Protection Detail | £210–250 (was £250–280) | /full-valet-premium |
| Enhancement Polish | £285–385 (was £275–425) | /machine-polishing |
| Ceramic Coating | from £190 (was £385) | /ceramic-coating |
| Odour Removal | £250–300 | /smoke/milk/bio-odour-removal |

Wording fixed: Exterior / In & Out include an 8-week wax, not a 6-month
sealant; Protection Detail has a 6-month sealant (not a 1-year coating) and
engine bay is an add-on; the polish includes a wax, not a sealant; Ceramic
Coating doesn't include a polish. Add-ons list rebuilt from the
questionnaires (ozone, "leather deep clean +£50", engine bay £40 gone).
Garage: Paint Correction from £600 (was £550), at the unit overnight, quote
→ Machine Polishing page; Window Tinting → /window-tinting; De Chrome →
/de-chrome (both NOT LIVE YET). Vinyl Wrap / PPF still → contact page.

Pages to create at exactly these addresses:
/paint-enhancement-machine-polishing, /window-tinting, /de-chrome.

---

## 2026-10-01 — Odour Removal: one 4-stage treatment

Files: `Odour Removal Treatment.txt`, `Odour Removal Whats Included.txt`
(the duplicate uploads without .txt were identical and are removed).

Replaces the two options (ozone, chlorine dioxide) with one service:
1 steam clean with enzyme or chlorine (chosen on the day) + full
extraction · 2 air-con system foam clean · 3 ozone 1 hr 30 min ·
4 ultrasonic purifying, disinfecting mist. The chlorine dioxide
standalone service is gone.

| Size | Acuity type | Price |
|---|---|---|
| 3-door | 92502297 | £250 |
| 5-door | 92502302 | £270 |
| 7-seater | 98946189 | £300 |

- Questionnaire rebuilt on the current setup: review badge, discount box,
  size → odour (same price, odour goes into the booking notes) → calendar
  on the page → 10% Stripe deposit. Was a redirect to Acuity's page with
  12 types.
- Cabin filter: customer supplies a new one; we take the old one out first
  and fit the new one at the end. On the info banner, in the breakdown, in
  the booking notes, and a required "Understood" in the booking details.
- What's Included: one 4-stage card instead of the Ozone / Chlorine
  Dioxide switch; FAQs, trust pills and payment wording updated.
- All Services card: £250–300, the four stages.
- Tested: 7-seater bio → 98946189, £300, deposit £30, notes carry the odour
  and the filter; booking blocked until the filter is confirmed; 3-door +
  AUTUMN20 → £200, deposit £20.
- Old types to HIDE in Acuity (not delete): 92502309, 92502362, 92502371,
  92502382, 92502407, 92502425, 92502427, 92502318, 92502323, 92502332.

---

## 2026-10-01 — Homepage section points at /all-services

File: `homepage section` ("Our Approach to Car Detailing" + service pills).

- "See All Services" button and every garage pill → /all-services
  (garage pills use #correction / #ppf / #wrap / #tint / #dechrome, which
  open the All Services page on its Garage half). Enhancement Polish →
  /all-services#machine-polishing until its own page is live.
- Fixed links: /mini-valet → /maintenance-wash, /car-exterior-deep-clean
  → /exterior-detail, Ceramic Coating → /ceramic-coating; nothing points
  at the non-existent /mobile-detailing-services any more.
- Pill prices brought in line: Interior £90, Exterior £70, Protection
  Detail £210, Enhancement Polish £285, Ceramic Coating £190, Odour
  Removal £250 (Maintenance £60, In & Out £140 unchanged).
- Unit location Ellesmere Port → Birkenhead (matches every other page).
- All 8 linked pages checked live (200); fits 375px with no overflow.

---

## 2026-10-01 — Homepage: reviews + service-area map added

File: `homepage section`. Appended the "REVIEWS + COVERAGE" block from the
old v6 services widget, unchanged except "Every service above" → "Every
mobile service above" (the homepage now also lists the at-the-unit
services). Elfsight Google reviews (loaded only when scrolled near), the
20-miles-of-Ellesmere-Port area with 21 town links (all checked live, 200)
and the Google map. Namespaced .mps-hsrv / #mpsHs*, so no clash with .mph.
Once this is live, the old v6 widget can be removed from the homepage.

---

## 2026-10-01 — Homepage hero buttons

File: `Homepage hero`. "Book Online" → /all-services (was the old
/book-your-valet-now). The phone-number button is now "Maintenance Plan"
→ /car-detailing-maintenance-plan, solid blue #00B8FF with dark text like
the add-on labels. Icons removed from both; on phones the padding and gap
are tightened so the two sit side by side (checked at 360px and 375px).

---

## 2026-10-02 — Maintenance Plan shows Maintenance Wash prices

File: `maintenance plan` (/car-detailing-maintenance-plan).

- Prices were the old Mini Valet ones (£50/£55/£60). Now the Maintenance
  Wash prices — Small £60, Medium £70, Large £80 — with the plan discount
  shown on each frequency: weekly 30%, every 2-3 weeks 25%, every 4-6
  weeks 20%.
- Extras now match the Maintenance Wash page: seat stain removal +£30 OR
  full steam, shampoo & extraction +£50, 8-week wax +£10, Glaco front
  windows +£30 (plan discount applies). The old "interior deep clean" and
  stacked seat/full-interior steps are gone.
- Booking: a valid plan code (CLEAN30/25/20, must match the frequency
  picked) unlocks "Book My Visit" → /maintenance-wash?code=…&size=…, which
  applies the code, shows the calendar and takes the 10% deposit. The old
  Mini Valet Acuity types (86209466 etc.) are no longer used by this page.
- No plan yet → "Start with an In & Out Deep Clean" (/full-valet).
- Maintenance Wash page now shows pence (£97.50 rather than £98) so the
  two pages agree.
- Homepage hero "From £35 on the plan" → "From £42" (£60 less 30%).
- Tested: medium, every 2-3 weeks, full interior + wax → £97.50 on both
  pages, £9.75 deposit; wrong-tier and seasonal codes rejected.

---

## 2026-10-02 — Page addresses updated (pages renamed on the site)

Checked every link in every file against the live site. Current addresses:

| Service | Address |
|---|---|
| Maintenance Wash | /maintenance-wash |
| Interior Deep Clean | /interior-deep-clean |
| Exterior Detail | /exterior-detail |
| In & Out Deep Clean | /in-out-deep-clean (was /full-valet) |
| Protection Detail | /protection-detail (was /full-valet-premium) |
| Machine Polishing | /machine-polishing (also the Paint Correction quote) |
| Ceramic Coating | /ceramic-coating |
| Odour Removal | /cigarette/milk/bio-odour-removal (was /smoke/milk/bio-odour-removal) |
| Window Tinting | /window-tinting |
| Maintenance Plan | /car-detailing-maintenance-plan |
| All Services | /all-services |
| De-Chrome | /de-chrome — NOT LIVE YET |

Updated in All Services, homepage section (Enhancement Polish and Window
Tinting pills now go to their own pages), maintenance plan, and the header
notes of Protection Detail and Odour Removal. If a page is renamed again,
set a 301 redirect from the old address in Squarespace (Settings >
Advanced > URL Mappings) so old links and Google keep working.

---

## 2026-10-02 — Window Tinting request panel

File: `Window Tinting Request.txt`.
- "Send on WhatsApp" now goes to the tint fitter, 07563 718029 (+44 7563 718029;
  changed from 07426 487900 on 2 Oct). The
  "send a copy" button after it goes to MobilePitStop (07592 196929), so
  every tint request still reaches the main number. Email unchanged.
- Postcode field removed (tinting is done at the unit) — also out of the
  message.
- Preferred week date box no longer overflows the panel on phones (iOS
  gives date inputs a minimum width; overridden). Checked at 375px.

---

## 2026-10-02 — "Send by email" works on PCs without an email app

"Send by email" uses a mailto: link, which does nothing on a PC with no
email app set up (anyone using Gmail / Outlook in a browser). Every request
form now uses a shared `window.mpsEmail(to, subject, body, cc, button)`
helper (the EMAIL FALLBACK block at the top of each file). It still tries
the email app first, then shows a panel under the buttons: Open in Gmail,
Open in Outlook (both pre-filled), Copy message, and our address.

Files: Window Tinting, De-Chrome, Ceramic Coating, Exterior Detail,
In & Out, Protection Detail, Machine Polishing (all the forms with a
"Send by email" button). Tested on Window Tinting and Ceramic Coating:
validation still runs first, panel appears once, Gmail/Outlook links carry
the address, subject and full message.

---

## 2026-10-02 — Homepage: "Our Services" by category

File: `homepage section`. "Our Approach to Car Detailing" (Approach /
Techniques / Tools / Products) replaced by "Our Services" in three blocks:

| Block | Label | Chips (linked where the service has a page) |
|---|---|---|
| Valeting | Mobile · from £60 | Maintenance Wash, Interior Deep Clean, Exterior Detail, In & Out Deep Clean, Odour Removal, Engine Bay Detail, Pet Hair & Stain Removal |
| Detailing | Mobile · from £190 | Paint Enhancement, Ceramic Coating, Protection Detail, Ceramic Sealant, Clay Bar, Multi-Stage Correction |
| Appearance & Ultimate Protection | At our unit · by quote | Vinyl Wraps, Paint Protection Film, Window Tinting, De-Chrome |

Images are stand-ins from the site, each marked "IMAGE:" for the edited
photos. Service pills, See All Services button, reviews and map unchanged.
All links checked live.

---

## 2026-10-02 — All Services tidy-up

File: `All Services.txt`.
- Category bar: arrows removed (they covered the pills on phones). Phones
  swipe; desktop already wraps every pill onto two lines.
- Descriptions, included lists, labels and add-on names now white.
- Add-ons moved from the blue "Add:" box into an "Optional add-ons"
  accordion (blue, one per line, prices in blue), straight above the
  button.
- Removed the extra notes between add-ons and button (Maintenance "not a
  first clean", Machine Polishing correction note, Ceramic 1/3/7 prices,
  Odour cabin-filter note, Paint Correction "from £600", Tinting UK law)
  and the garage "Finish it with" / "Often paired with" lines.
- Category headings removed where a section has one service; kept for
  In & Out (two services) and Add-ons.
- Yellow line between every service, including the two In & Out cards.

---

## 2026-10-02 — Preferred date box fits on phones, every form

iPhones give date inputs a built-in minimum width, so the Preferred date /
week box ran past the edge of the request panel. The fix already on Window
Tinting (appearance:none, min-width:0, max-width:100%) is now on every
request form with a date box: Ceramic Coating, Exterior Detail, In & Out,
Protection Detail, Machine Polishing and De-Chrome. Checked at 375px.

---

## 2026-10-03 — Free-slot check sends up to 8 add-ons

The Make availability scenario now reads a1–a8, so every booking page sends
up to eight add-ons when checking free slots (`addonIds().slice(0,8)` in
`addonParams()`): Maintenance Wash, Interior Deep Clean, Exterior Detail,
In & Out, Protection Detail, Machine Polishing, Odour Removal. Before, only
the first four counted, so a booking with more add-ons could be offered a
slot too short for the job. Checkout already sent every add-on.

---

## 2026-10-05 — Reviews + map as a standalone block

New file `Reviews and Map.txt`: the Google reviews (Elfsight) and the
"Free travel within 20 miles of Ellesmere Port" area with the 21 town links
and the map, copied from the bottom of `homepage section` so it can be
pasted on any page as its own Code Block.

- Same look, wording and links as the homepage. "Every mobile service
  above" now reads "Every mobile service", because the block can stand on
  its own. The divider line above it is gone.
- Classes renamed `.mps-rvm-` and the ids removed, so it doesn't clash with
  the homepage copy and works even if it appears twice on one page. The
  Elfsight script is only loaded once per page.
- Reviews still load only when scrolled near (Elfsight is 531 KB).
- The homepage section keeps its own copy, unchanged. If towns, wording or
  the map change, update both files.

Checked at 375px with the block twice on one page: no sideways scroll,
both review slots filled, one platform.js request.

---

## 2026-10-05 — Deposit request sends the real job total

Every booking page now sends `total` to the deposit hook alongside
`deposit`, so Make can record the job value instead of working it out as
deposit x 10. Both are in pence. `total` is the price after any discount
code or plan code (`netTotal()`; on Maintenance Wash `calcTotal()`, which
already has the discount in it). Example: In & Out 3-door £140 with
AUTUMN20 sends total=11200, deposit=1120.

Pages: Maintenance Wash, Interior Deep Clean, Exterior Detail, In & Out,
Protection Detail, Machine Polishing, Odour Removal. The Make deposit
scenario has to map `total` for it to be used.

---

## 2026-10-06 — Funnel tracking (dataLayer → GTM → GA4)

Every questionnaire pushes funnel events to `window.dataLayer`. GTM
(GTM-MMHW32R2) picks them up; there is no gtag/GA code on the pages. Each
file has a "FUNNEL TRACKING" block just above `update()`.

| Event | When | Fields |
|---|---|---|
| `quote_start` | first choice made, once per page load | service |
| `quote_complete` | first full price shown, once per page load | service, value, currency |
| `slot_select` | a time picked in the calendar | service, value, currency, slot_date |
| `begin_checkout` | Pay deposit pressed, after the form checks, before the Stripe redirect (preceded by `{ ecommerce:null }`) | service, deposit, ecommerce{currency, value, items[{item_name, item_category:'Detailing', price, quantity:1}]} |
| `generate_lead` | WhatsApp or email request sent | service, method ('whatsapp' / 'email') |

- `value` is in pounds, as a number, after any promo or plan code:
  `netTotal()` on every booking page except Maintenance Wash, where
  `calcTotal()` already includes the discount. Exterior Detail uses
  `netTotal()` too, because its `calcTotal()` is before the promo.
  Ceramic Coating has no `netTotal()`, so it uses `calcTotal()` less the
  promo. `deposit` is `depositAmount()`, also in pounds.
- `service` slugs: maintenance_wash, interior_deep_clean, exterior_detail,
  in_and_out, protection_detail, machine_polishing, odour_removal,
  ceramic_coating, window_tinting, de_chrome.
- "Full price" means every required question is answered. A
  van/camper quote or a Machine Polishing correction ("from" price) never
  sends `quote_complete`.
- Ceramic Coating has no calendar, so no `slot_select` or
  `begin_checkout`. Window Tinting and De-Chrome have no price, so only
  `quote_start` (first chip tapped) and `generate_lead`. Their "send a copy
  to MobilePitStop" button doesn't count as a second lead.
- `generate_lead` also fires on the "Get a quote" WhatsApp button for odd
  vehicles on Interior and In & Out. The existing `pf_*` events are all
  kept unchanged.
- No personal data: no name, email, phone, address, postcode or reg.
- Only additions. No booking, pricing or payment code changed.

Tested every page in the browser with the Make webhooks faked: each event
fired once at the right moment with the right value (e.g. Exterior 3-door
with AUTUMN20 = 56; Maintenance Wash 5-door with CLEAN30 = 49, deposit 4.9).

---

## 2026-10-06 — Live site-wide code added to the repo

Copies of the current live code, so the repo matches the site:

- `mobilepitstop-code-injection-HEADER.html`: Settings > Developer tools >
  Code injection > HEADER. Consent Mode v2 defaults (ads off until
  accepted, statistics on unless switched off), then GTM-MMHW32R2, then
  `window.MPS_REVIEWS` (the only place to change the review count).
- `mobilepitstop-code-injection-FOOTER.html`: Code injection > FOOTER. GTM
  noscript, ad click ID capture (only with ad consent), the Elfsight
  reviews app, and the cookie banner (`mps_consent_update` event).
- `mobilepitstop-booking-confirmed.html`: the /booking-confirmed page.
  Pushes `purchase` (GA4 ecommerce) and `mps_booking_confirmed` once per
  Stripe session. `value` comes from `?amt=` in pence (the job total the
  questionnaires send as `total`); `deposit_paid` from `?dep=`. If `amt`
  is ever missing it falls back to deposit x 10.

In & Out: the odd-vehicle "Get a quote" WhatsApp message said "a quote for
an interior deep clean". It now reads "a quote for an In & Out Deep Clean
on a larger vehicle (van, pick-up, minibus or camper)".

---

## 2026-10-05 — Stripe success URL sends the real total (done in Make)

Make scenario "MPS · Stripe deposit link" (7611308), updated 5 Oct 2026:
the Stripe success URL now sends `amt={{1.total}}` and `dep={{1.deposit}}`
(both pence), and `name=` has been removed. So the booking-confirmed
`purchase` event reports the real job total (`value_source: 'amt'`), not
deposit x 10, and no customer name reaches the address bar.

---

## 2026-10-06 — Booking confirmed: purchase carries the service slug

The `purchase` push on /booking-confirmed now has a top-level `service`,
using the same slugs as each questionnaire's `FUNNEL_SERVICE`, so purchases
line up with quote_start → begin_checkout in GA4. It comes from the `svc`
label in the URL through the `SERVICE_SLUGS` table, matched anywhere in
the label (case and punctuation ignored), so both "Protection Detail" and
"10% deposit — Protection Detail, 5-door + engine bay" give
`protection_detail`. No match gives `null`. Everything else in the push is
unchanged. If a service is renamed, add the new name to the table.

---

## 2026-10-06 — Cookie banner policy link fixed

The "Cookie policy" link in the cookie banner (FOOTER code injection)
went to `/privacy-policy#pp-cookies`, which is a 404. It now goes to
`/privacy-policy-cookie-policy#pp-cookies`, the live policy page, which has
the `pp-cookies` section.

---

## 2026-10-08 — Terms & Conditions tick box on the 7 deposit pages

Interior Deep Clean (`idc`), Maintenance Wash (`mps`), Exterior Detail
(`edc`), In & Out (`iod`), Protection Detail (`fvp`), Machine Polishing
(`mpol`) and Odour Removal (`odr`).

- A native checkbox `#<prefix>BkTerms`, unticked by default, sits after the
  "Occasional offers by email?" row and before the error line. Text: agree
  to the Terms & Conditions (linked, opens in a new tab), want the work done
  within the 14-day cancellation period, pay for work done if cancelled
  after starting, and can't cancel a finished job.
- Styled like the yes/no questions (13px, white at 66%), link underlined in
  brand yellow, whole label tappable (about 114px tall on a phone), blue
  focus ring on the box.
- Paying without ticking shows "Please tick to confirm you agree to our
  Terms & Conditions." and moves focus to the box; nothing is sent to Make.
  The check runs after the power/water/space check.
- The deposit request now also sends `terms: 'yes'` and
  `terms_version: '2026-09-17'` (the "Last updated" date on
  /terms-conditions). Make puts both into the Stripe metadata. **If the
  terms change, update `terms_version` on all 7 pages.**
- In & Out: `pf_book_click` said `service:'interior_deep_clean'`; it now
  says `'in_and_out'`, matching `FUNNEL_SERVICE`.

Tested on every page with the Make webhooks faked: unticked = error +
focus + no request; ticked = request includes both fields and the page
redirects to the checkout link it gets back.

---

## 2026-10-08 — In & Out: sealant line in the price breakdown fixed

Adding the 6-month ceramic sealant printed `" title="Remove">×` as text in
the breakdown. The sealant row was the only one that didn't give its remove
button a plain-text name, so the button used the row title, whose
"Clay bar included" sub-line has quote marks that closed the button's
`aria-label` early. The row now passes 'ceramic sealant', and `removeBtn()`
strips any HTML from its label so a future row can't do the same. The other
pages' rows all pass plain names and weren't affected.

---

## 2026-10-08 — Diary errors: Acuity add-ons and the Interior 7-seater ID

"Couldn't reach the diary" appeared on Maintenance Wash and Machine
Polishing. The Make availability scenario failed with Acuity 400 Bad Request
whenever an add-on wasn't enabled for the appointment type:

- 6425104 (Maintenance Wash full interior) and 7344721 (6-month sealant
  without clay) weren't enabled for any type. Gab switched them on; both
  now work on Maintenance Wash, Machine Polishing, In & Out and Exterior.
- Interior Deep Clean 7-seater: the old type 84626189 is rejected even with
  no add-ons, so 7-seaters couldn't see any dates. The page now uses
  **98913503**.

Swept every add-on ID on all 7 booking pages against the matching size
types; everything else returns free days. How to re-check: call the
availability hook with `type=<id>&month=YYYY-MM&a1=<addon>`; a 500
"Scenario failed to complete" means Acuity rejected the pairing.

---

## 2026-10-09 — FAQs, homepage FAQ & plan block, and all page schemas updated

New files in the repo (they only existed on Squarespace before). The first
commit on this branch is the live code as it was, so the PR diff shows the
changes:

- `FAQs page.txt` — the main code block on /faqs
- `Homepage FAQ.txt` — the "Frequently Asked Questions" code block
- `Homepage Maintenance Plan.txt` — the "Set Up Your Maintenance Plan" code
  block (visual + calculator)
- `Schema/` — the JSON-LD in each page's Page Header Code Injection

What changed:
- Current names and prices everywhere (All Services is the source):
  Maintenance Wash £60–80, Exterior Detail £70–90, Interior Deep Clean
  £90–120, In & Out Deep Clean £140–180, Protection Detail £210–250,
  Enhancement Polish £285–385, Ceramic Coating from £190 (1, 3 or 7 years,
  up to £470), Odour Removal Treatment £250–300 (four stages), unit
  services by quote. Add-on list copied from All Services.
- "What's included" answers rewritten from each service page; durations
  from the service pages' FAQs. **TODO for Gab: no Odour Removal duration
  exists anywhere — see the TODO comment in `FAQs page.txt`.**
- Payment, deposit and cancellation wording follows /terms-conditions.
- Maintenance plan: "start with a deep clean", Maintenance Wash prices
  (£60/70/80) in the homepage calculator; CTA goes to /in-out-deep-clean.
- /faqs now has one H1 (the Squarespace text block; the code block's H1
  was removed). Links: /exterior-detail, /all-services instead of
  /book-your-valet-now, tel:+447592196929 and WhatsApp wa.me/447592196929.
- Both FAQPage schemas are generated from the visible Q&A, word for word.
- Business schema: AutomotiveBusiness (was AutoWash), Car_wash type removed,
  offer catalog rebuilt with live URLs, Instagram → mobilepitstopvalet.
  Service schemas renamed to the current services, new URLs and @ids, and
  `provider` now points at `#business` (it pointed at the homepage URL).
- New schemas: Machine Polishing, Ceramic Coating, All Services (catalog).

Found but not changed (need Gab):
- /window-tinting carries the Odour Removal schema in its page header —
  delete it there.
- /car-detailing-maintenance-plan has a second Service schema inside its
  "Car Detailing Maintenance Plan — Cheshire, Merseyside & Flintshire" text
  block — delete that `<script>` so the page has one.
- Interior Deep Clean What's Included says heavy sand +£40; the
  questionnaire and All Services charge +£50.

---

## 2026-10-09 — Privacy & Cookie Policy updated

New file `Privacy and Cookie Policy.txt` (the Code Block on
/privacy-policy-cookie-policy; it only lived on Squarespace before). The
first commit on the branch is the live version, so the PR diff shows the
changes. Last updated now 9 October 2026.

- Booking happens on our own site; Acuity is only reached through the
  reschedule/cancel links in confirmation emails (intro + section 9).
- Section 2: records the customer's agreement to the Terms and when.
- Advertising measurement: Google only. Meta and Reddit removed from the
  uses, sharing list, transfers and the advertising cookie table.
- Processors: Stripe (replaces Square), Make added, Squarespace now also
  sends marketing emails (Email Campaigns); separate "email marketing
  provider" line removed.
- Essential cookies: `mps_conv_…` added (booking-confirmed de-duplication),
  plus a note that Stripe Checkout sets its own essential cookies.
- Links: /terms-conditions and /privacy-policy-cookie-policy.

---

## 2026-10-09 — Discount link, dynamic question numbers (sticky bar removed)

**#13 Sticky mobile action bar** — built, then removed on 2026-10-09 at Gab's request
(not wanted). Nothing for it remains in the footer code.

**#14 Questionnaires (7 booking pages)**
- The discount code panel moved from the top of the booking card to the
  final step, under the total, behind a "Have a discount code?" link.
  Same element ids, so MPS_PROMO is unchanged; the panel opens by itself
  when a code is already applied (announcement bar, ?code=, sessionStorage).
  Maintenance Wash: its plan/promo code form moved the same way; the link
  opens it in place (it used to scroll back to the top) and it opens by
  itself for ?code= plan links.
- Question numbers are now computed: only visible questions, in order,
  renumbered when one shows or hides (Interior and In & Out used to show
  1, 2, 5, 6, 7 before a seat fabric was picked).
- Interior and In & Out got the ResizeObserver the other pages already had,
  so the final step's height follows the opened panel.

Tested at 375px on all 7 pages.

---

## Open issues (not fixed yet)

- **Two Exterior Detail files.** `Exterior Detail Questionnaire.txt` is the
  live one. `Exterior Detail.txt` is an older version and could be deleted.
- **Interior Deep Clean header comment is stale.** The prices in the code
  (£90 / £100 / £120) are correct. The comment at the top of the file
  describes a different plan (£100 / £120 / £140, 48 Acuity types, an
  `ACUITY_TYPE_MAP` that doesn't exist) and should be rewritten to match
  the code. Nothing on the page is affected.
- **Odd vehicle types on In & Out.** "Van, pick-up, minibus or camper" goes
  straight to a WhatsApp quote message, not the Acuity quote slot that
  Exterior Detail uses.
