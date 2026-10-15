# Hotel Ava — NFC Promo Landing Page

Static single-file landing page for the NFC marketing cards. Tap the card → phone opens this page → guest copies the promo code → books on the live Hotel Ava system.

## Files

- `index.html` — everything (inline CSS + JS). No build step, no dependencies (Google Fonts + hotlinked hotel photos only).
- `logo.png` — the official Hotel Ava mark (copy of `src/assets/images/Hotel Ava logo.png`), used for the header brandmark and the page favicon.

## What's on the page

1. **Capstone notice** (very top) — a dismissible dark banner stating this is **not** the real Hotel Ava Malate site, just a student capstone NFC promo prototype. It reappears every new session on purpose; if a panelist should ask, that's the honest answer.
2. **Hero + promo card** — NFC code + Copy + Book Now.
3. **Stats strip** — 5 room types / 13 nearby spots / 24/7 front desk.
4. **How it works** — tap → copy → book, now an animated illustration guide (three looping SVG scenes revealed on scroll).
5. **Rooms** — Standard, Deluxe, Regular Suite with rates.
6. **Location & neighborhood** — full address (2184 Madre Ignacia St. cor. Quirino Ave., Ermita, Manila), Google Maps link, "why guests pick this spot" facts, and **13 nearby places** (Baywalk, DLSU, Malate Church, Robinsons, Zoo, Gil Puyat LRT, Star City, Luneta, MOA, National Museum, Ocean Park, Intramuros, NAIA T1) — every card links to Google Maps directions. Data mirrors `src/data/nearbyPlaces.ts`.
7. **Why Hotel Ava** + footer.

## How to host (GitHub Pages)

1. **Option A — separate repo (recommended, URL is short for NFC):**
   - Create a new repo, e.g. `hotel-ava-promo`.
   - Upload this `index.html` at the repo root.
   - Settings → Pages → Deploy from branch → `main` / root.
   - Site lives at `https://<your-username>.github.io/hotel-ava-promo/`

2. **Option B — in this repo:**
   - Settings → Pages → Deploy from branch → `main` / root.
   - Site lives at `https://<your-username>.github.io/<repo>/nfc-promo/`
   - Downside: longer URL — NTAG213 cards hold ~144 bytes, so the short Option A URL is safer.

The live Hotel Ava system is deployed at `https://hotelava.vercel.app/` (checked 2026-10-12: `/rooms` returns 200).

## Folder contents

```
nfc-promo/
  index.html   ← the landing page (everything inline)
  logo.png     ← Hotel Ava brandmark (header + favicon)
  places/      ← 13 Wikimedia Commons photos for the "Explore nearby" cards
  README.md
```

Upload the **whole `nfc-promo/` folder** (not just `index.html`), otherwise the nearby-place images 404 — and the logo will too.

## Data sources (keep these in sync)

- **Rooms** — names, prices, and photos come from the live `GET /api/rooms/public` on `hotelava.vercel.app`. Room images are hotlinked from Supabase Storage. Re-check prices there before printing cards.
- **Nearby places** — mirrors `src/data/nearbyPlaces.ts`; photos copied from `public/places/`.
- **Map** — live Google Maps embed, same query string as `src/components/home/LocationSection.tsx`.

## Configure (edit `index.html`, search for `CONFIG`)

```js
var BOOKING_BASE = "https://hotelava.vercel.app/rooms"; // your live system (or http://localhost:5173/rooms for dev)
var PROMO_CODE = "AVANFC10";                              // ← replace with your real promo code
```

The Book Now links are built as:

```
{BOOKING_BASE}?promo={PROMO_CODE}&src=nfc
```

`src=nfc` marks the traffic so you can tell panelists how many bookings came from the cards.

## Write the NFC card

1. Install **NFC Tools** (iOS/Android, free).
2. Open the app → **Write** → **Add record** → **URL**.
3. Paste your hosted page URL, e.g. `https://<user>.github.io/hotel-ava-promo/`
   - Keep it short — NTAG213 stores ~144 bytes total (URL record overhead included).
   - Use NTAG215/226 if you need a longer URL.
4. Tap **Write**, hold the phone against the card until it confirms.
5. **Lock the tag** (NFC Tools → Other → Lock) so it can't be overwritten.
6. Test: tap the card with a phone, confirm the page opens, tap **Copy**, then **Book Now** and check the promo shows up.

## Checklist before printing cards

- [ ] `PROMO_CODE` replaced with the real code
- [ ] `BOOKING_BASE` points to the live production URL (not localhost)
- [ ] Hosted page loads on iPhone Safari (clipboard fallback tested)
- [ ] Book Now lands on `/rooms` and the promo param is present
- [ ] One physical card tested end-to-end on a real phone

## Notes

- This page is marketing only — it never validates or applies the discount itself. The Hotel Ava backend stays the source of truth (once promo-code validation is built server-side).
- Room photos are hotlinked from the Hotel Ava Supabase Storage bucket (`voabhnoephsgdkklabgo.supabase.co/.../room-images/`); if those URLs ever break, re-pull them from `/api/rooms/public`.

## What's on the page

1. **Capstone disclaimer bar** (top, amber dot) — states plainly that this is a student demo, not the real Hotel Ava Malate site. Dismissible, but uses `sessionStorage` so it re-appears every new visit.
2. **Hero + promo card** — the NFC promo code with a **Copy** button and a **Book Now with Promo** CTA.
3. **Stats strip** — 5 room types · 13 nearby spots · 24/7 front desk.
4. **How it works** — an animated illustration guide: three looping, pure-CSS SVG scenes (phone tapping the NFC card → promo code copying → price dropping to a -10% badge), revealed one-by-one as you scroll, with chevron connectors between cards on desktop. Respects `prefers-reduced-motion` (freezes on a complete resting frame) and stays fully visible with JS disabled.
5. **Stay at Hotel Ava** — all five real room types with live prices and Supabase photos: Standard ₱1,500 · Deluxe ₱2,500 · Junior Suite ₱2,599 · Executive Deluxe ₱2,900 · Superior Suite ₱5,000.
6. **Right in the heart of Malate** — a **live Google Maps embed** pinned at 2184 Madre Ignacia St., plus four reasons guests pick the spot.
7. **Explore nearby** — 13 photo cards, each showing distance and linking to Google Maps directions.
8. **Why Hotel Ava?** — four selling points (AI discounts, instant confirmation, secure checkout, prime Malate location).
9. **Footer** — address and the NFC tagline.
