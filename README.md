# Speedioo web · front-end build

Static, framework-free front end for the Speedioo used two-wheeler marketplace (Pune, Mumbai, Bangalore).
Every flow works in the browser today; data is kept in `localStorage` until the backend is connected.

## Run locally
```bash
npm run dev          # serves the folder on http://localhost:3000 (needs Node 18+)
# or: python3 -m http.server 3000
```
Opening `index.html` directly from disk also works.

## Deploy to Vercel
1. Push this folder to a GitHub repo.
2. In Vercel: **Add New → Project → Import** the repo.
3. Framework preset: **Other**. Build command: *(empty)*. Output directory: `.` (root).
4. Deploy. `vercel.json` adds security headers and tells browsers to re-check files on every visit, so a new deploy shows up immediately.

**Releasing a change:** bump the `?v=` number on the CSS/JS links in `index.html` (and `version` in `package.json`) so phones never mix old and new files.

CLI alternative: `npm i -g vercel && vercel` (then `vercel --prod`).

## What's in here
| Path | Purpose |
|---|---|
| `index.html` | Page shell: header, footer, SEO meta, JSON-LD, script tags |
| `assets/css/styles.css` | Design tokens (red / white / black), components, motion |
| `assets/js/data.js` | Sample inventory, media URLs, FAQs, reviews, contact details |
| `assets/js/api.js` | **API layer.** Promise-based mocks for every backend call + GA4 `dataLayer` events |
| `assets/js/app.js` | Hash router, pages, interactions, chatbot v1 |
| `assets/js/story.js` | 20-second animated story "Sold in 28 minutes" (SVG, scrubbable, used on Home and Sell) |
| `assets/video/speedioo-sold-in-28-minutes.mp4` | The same story exported as a 1280×720 MP4 for WhatsApp / ads |
| `#leads` route | Team demo inbox of everything the forms captured in this browser, with CSV export |

## Pages and flows that work now
Home (video hero, Buy/Sell finder, brand marquee, shop-by, fresh arrivals, speedometer "how it works", services, rider-story video, reviews, FAQ, WhatsApp opt-in) · Buy listing (filters, sort, quick view, compare, request a bike) · Bike detail (gallery + lightbox, inspection report, specs, test ride, reserve, EMI) · Sell (3-step quote, OTP, photo upload, estimate, slot booking, tracker, referral) · About / reviews / team / contact · Blog · Search (typo-tolerant, "under 1 lakh", voice placeholder) · OTP login + account (shortlist, test rides, sell requests) · Hindi / Marathi switch · Chat assistant (scripted v1).

Demo OTP: any 4 digits.

Logo intro: first-time visitors see the two tyre "oo"s roll in from the right. Add `?intro` to the URL to replay it.

## Connecting the backend (ERPNext)
Set `API_BASE` in `assets/js/api.js` and replace each mock body with a `fetch()`; function names map 1:1 to endpoints
(`listBikes`, `getBike`, `lookupReg`, `sendOtp`, `verifyOtp`, `createLead`, `estimate`, `bookInspection`, `bookTestRide`, `reserve`, `rcStatus`, `saveShortlist`).
Leads carry `utm_*` and `gclid` captured from the landing URL for Google Ads attribution.

Analytics: paste the GTM / GA4 snippet in `index.html`; events already pushed include `page_view`, `view_item_list`, `view_item`, `select_item`, `add_to_wishlist`, `search`, `generate_lead`, `valuation_shown`, `book_inspection`, `book_test_ride`, `begin_checkout`, `chat_open`.

## Before launch
- **Font:** replace the `--font` token in `styles.css` with the family from the live speedioo.com CSS.
- **Media:** photos and videos are hot-linked from Pexels (free licence, pexels.com/license) and are stand-ins. Replace with Speedioo's own inspection photos and customer videos (store on a CDN such as Cloudinary or Vercel Blob). If a photo fails to load, a drawn illustration is shown.
- **Placeholders:** everything in red `[brackets]` needs a real value or must be removed (ratings, counts, warranty, token amount, lender names).
- **SEO:** hash routes are fine for a front-end build. For indexable city/brand/model pages, move to Next.js (SSR/SSG) when the API is live.

## Media credits
Hero video: Pexels #4606791. Rider story: Anastasia Shuraeva (Pexels #9606662). Walk-around sample: Pavel Danilyuk (Pexels #5052415). Bike and scene photos: Pexels photo IDs listed in `assets/js/data.js`.
