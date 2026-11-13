# Tactics

Each tactic: why it works, how to search, how to write up the result. Comparisons always use the true total.

## T1 — Wide dates × wide airports

**Why**: Prices vary a lot by airport pair and day; a city's secondary airport (SAW, BGY, BVA, CRL…) is often a low-cost base.
**How**: From every airport around the origin to every airport in the destination region, across the target month. Google Flights accepts comma-separated airports; Skyscanner has a "whole month" view.
**Output**: The 5 cheapest date combinations; weekday and weekend departures shown separately (let the calendar decide which days are cheap — no fixed rule holds across routes). Price, number of stops and total duration for each.

## T2 — Date-shift table

**Why**: Shifting by one day can sometimes halve the fare; holiday start/end days are the most expensive.
**How**: Google Flights "Date grid" (outbound × return matrix) and "Price graph".
**Output**: Outbound prices for −7…+7 days, cheapest day marked; add the return grid too when it is meaningful.

## T3 — Split one-way tickets

**Why**: Low-cost carriers price per direction; flying out with one airline and back with another can beat a round trip. Full-service carriers (Turkish Airlines, Lufthansa…) often show the opposite.
**How**: Search the cheapest one-way for each direction separately (Kiwi combines these automatically), compare the sum with the cheapest round trip on the same dates.
**Output**: Sum of two one-ways vs round trip, net difference. Note: with separate tickets, a cancellation on one leg does not protect the other.

## T4 — Alternative destination airport

**Why**: When the route to the main city is expensive (monopoly, season), flying low-cost to a nearby city and continuing by train/bus can be cheaper.
**How**: List airports within ≤3 h by road/rail of the destination (e.g. Barcelona → Girona, Reus; Milan → Bergamo; Brussels → Charleroi, Eindhoven; Vienna → Bratislava). For each, find flight + ground transport prices (Omio, Trainline, FlixBus, operator site).
**Output**: Flight + ground transport = true total, plus the extra time. Say so when the saving isn't worth the extra 2–3 hours.

## T5 — Multi-city / open-jaw

**Why**: With several cities, the order and ticket structure decide the total.
**How**: Compare the possible orders (6 for 3 cities). Options: (a) multi-city ticket, (b) fly into A, home from C (open-jaw) + train/bus/low-cost legs in between, (c) separate round trips.
**Output**: The cheapest 2–3 orders/structures, with true totals including inter-city transport.

## T6 — Positioning flight

**Why**: Long-haul fares from some hubs (e.g. London, Amsterdam, Frankfurt, Athens, Gulf cities) are much cheaper; adding a cheap low-cost leg to reach the hub can lower the total.
**How**: Scan fares to the destination from major hubs; add the low-cost fare from the origin to whichever hub comes out cheap.
**Output**: Positioning + main leg total. Because these are separate tickets, recommend at least a 4–5 h buffer or travelling the night before instead of same-day connection (the buffer's cost goes into the true total).

## T7 — Why-is-it-expensive analysis

**Why**: Knowing the cause points to the right alternative.
**How**: Check: season/school holidays, public holidays, events on those dates (trade fair, match, concert, festival), a single airline operating the route, demand direction (e.g. outbound from a big city before a holiday is pricey, the return is cheap).
**Output**: 2–3 causes, each paired with a concrete alternative that sidesteps it (other day, other airport, other airline).

## T8 — Risky tactics

Suggest only when the user has explicitly accepted risk; carry the risks below into the report verbatim.

**Hidden-city (skiplagging)**: Buying a ticket to a further destination whose connection is in your actual destination, and getting off there.
- One-way tickets only; skipping a leg cancels all remaining legs.
- Cabin bag only: checked bags go to the final destination. On full flights the cabin bag can be gate-checked — which breaks the plan.
- Violates airline terms: frequent-flyer account may be closed, fare difference may be charged.
- If the airline reroutes or changes the connection, you land somewhere else.
- Skiplagged lists these combinations.

**Self-transfer**: Connecting on two separate tickets (Kiwi offers these).
- If the first flight is delayed, the second ticket is lost; the airline isn't responsible (mention paid protection like Kiwi Guarantee if available).
- You must collect bags and check in again → entry into the connecting country and a visa may be required (e.g. a Schengen self-transfer needs Schengen entry, hence a Schengen visa for Turkish citizens).
- At least 3–4 h buffer; confirm both flights use the same airport (London LHR/LGW/STN, Paris CDG/ORY, Milan MXP/LIN/BGY traps).
