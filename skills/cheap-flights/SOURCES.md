# Sources and search patterns

Airline bases, routes and site behaviour change; treat every specific below as an example to re-check, not a fact.

## Flight search

| Site | Strength | How to search |
|---|---|---|
| Google Flights | Calendar with per-day prices, date grid, price graph, price insights ("prices are currently low/typical/high"), Carry-on bag filter | Open `https://www.google.com/travel/flights?hl=en&curr=EUR` and fill the form. The `?q=Flights from … to …` URL does **not** run a search (lands on the home page — tested Nov 2026). Once on a results page, change the origin/destination fields in place. |
| Skyscanner | "Whole month" / "cheapest month", destination = "Everywhere" | **Not usable from the browser tool**: it serves a "press & hold" bot check (tested Nov 2026). Never attempt CAPTCHAs — give the user the link `https://www.skyscanner.net/transport/flights/ista/lis/260310/260317/` (dates YYMMDD) to open themselves. |
| Kiwi.com | **Main second source** (replaces Skyscanner). ±2-day price strip, bag-inclusive prices, self-transfer and split one-way combos | `https://www.kiwi.com/en/search/results/istanbul-turkey/edinburgh-united-kingdom/2027-02-25/2027-03-02` (city slugs `city-country`, dates YYYY-MM-DD; add `?bags=1.0-` for one cabin bag) — tested Nov 2026, works without login. Choose "Reject all" on the cookie dialog. |
| Skiplagged | Hidden-city combinations (T8 only) | Site form |
| Airline site | Winner's final price, fare-family comparison (what each package includes), baggage/seat fees, promotions | Booking flow up to the fare-family screen — no login needed. E.g. Pegasus, AJet, Turkish Airlines, SunExpress, Wizz Air, Ryanair, easyJet |

Airline-site tips:
- Many low-cost sites show a fare calendar in the date picker (Pegasus: one-way price per day for outbound, then for return once a departure is picked). It is the second source for the winner's price — Pegasus's calendar matched Google Flights to the euro in testing.
- Booking flows often run on a separate host (Pegasus: `web.flypgs.com`, deep link `https://web.flypgs.com/booking?language=en&adultCount=1&departurePort=SAW&arrivalPort=EDI&currency=EUR&dateOption=1&departureDate=2027-02-25&returnDate=2027-03-02`). The browser extension needs that host permitted; when blocked, give the user the deep link to check fare packages themselves.

Kiwi tips:
- **Baggage cost by difference**: load the results once without bags and once with the Bags filter set (cabin and/or checked); the price difference on the same flight is that airline's real bag fee for this route. This settled Pegasus's cabin-bag fee in testing where Google's bag filter could not.
- Read the "Days" price strip and results with `javascript_tool` (`document.body.innerText`, sliced from `'Days'`).
- Prices show in the site currency (TRY from Turkey); switch via the currency selector top-right or compare in that currency consistently.
- Kiwi-only combos are marked "Self-transfer" — those are T8 territory.

Google Flights gotchas:
- Prices are per adult; multiply for the party size yourself.
- The Carry-on bag filter is unreliable for low-cost carriers (it kept a Pegasus fare unchanged although the cheapest Pegasus international package includes only a 3 kg under-seat item). Confirm baggage on the airline site.

Departing from Turkey (example): Istanbul's IST (mostly Turkish Airlines) and SAW (mostly Pegasus, AJet) often have different direct routes — check both; direct low-cost flights from Ankara ESB, Izmir ADB or Antalya AYT can beat a domestic hop to Istanbul + international leg.

## Point of sale

The same flight can be cheaper on the airline's site for another country or in another currency (no VPN needed — switch the site's country/currency selector). Compare at most two points of sale for the winner, and add the card's foreign-transaction fee and exchange spread to the true total.

## Ground transport (T4, T5)

Omio, Trainline (Europe-wide), FlixBus, national rail sites (Trenitalia, Renfe, SNCF, DB, ÖBB, LNER).

## Working in the browser

- On cookie banners, accept only essential cookies.
- Read result lists with `javascript_tool` (`document.body.innerText`, sliced) — cheaper than full-page text; use screenshots for calendars and price grids.
- Stop at any step that needs login, an account, or payment details — hand it to the user.
- Close the tabs you opened when done.
