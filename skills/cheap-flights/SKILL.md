---
name: cheap-flights
description: Flight deal hunt — finds a trip's lowest true total cost from live prices (flexible dates, nearby airports, split one-way tickets, alternative destinations, multi-city routing, hidden fees). Use when the user asks about cheap flights / ucuz uçak bileti, comparing airfares, "when/where is it cheapest to fly", or a travel budget.
---

# Flight Deal Hunt

Goal: scan the combinations a plain "A→B round trip" search misses and find the lowest **true total**.

**True total** = fare + baggage + seat + check-in/payment fees + airport↔city transport + (where applicable) transit visa, layover hotel, extra ground transport. Every ranking, comparison and recommendation in this skill uses the true total.

**Verified price**: every price in the table was seen through a tool (browser, WebFetch) in this session, and carries its source site and timestamp. Without tool access, give the user the list of searches to run instead of prices (site + ready-made URL — see [SOURCES.md](SOURCES.md)).

This skill researches; the user completes the booking and payment themselves. Write the report in the user's language.

## Step 1 — Brief

The user states only their trip: where from, where to, roughly when, who, what luggage. Ask only for what is missing, in **one message**, offering a sensible default for each so the user can just say "yes". When nothing is missing, move straight on to Step 2.

| Input | Suggested default |
|---|---|
| Origin | The city named; you expand it to airports yourself |
| Destination | The city named; you expand it to airports yourself |
| Date flexibility | ±3 days |
| Nights, passengers | — (ask) |
| Baggage | Cabin bag only |
| Connection tolerance | Max 1 stop, total time ≤ 2× the direct flight |
| Risk appetite | Low (self-transfer and hidden-city off) |
| Currency / budget | Local currency and EUR side by side |

Done when: every row is filled from the user or an accepted default.

## Step 2 — Map the search space yourself

The user's request names cities; mapping them to the full search space is your job. Research it (WebSearch, airport/rail sites) rather than relying on memory, and build:

- **Origin airports**: every airport serving the origin city, plus nearby cities with direct low-cost service a domestic hop or bus away.
- **Destination airports**: the main airport, every airport within ≤3 h by rail/road (with the rail/bus time and typical fare), and hubs from which a cheap final leg or train reaches the destination.
- **Trip context**: events, festivals and holidays at the destination in the travel window; entry/visa requirements for the user's nationality, including transit countries on candidate routes.

Show the map to the user in 2–4 lines ("Origin: IST, SAW · Destination: EDI + GLA (train 1h15), NCL (train 1h30)… · Note: festival in August, UK visa needed") — it feeds every tactic below.

Done when: both airport lists and the context notes are written, each alternative with its transfer time and cost.

## Step 3 — Tactic selection

Pick the tactics to run from the brief and tell the user in one line ("Scanning: …"). When each tactic pays off and how to search it lives in [TACTICS.md](TACTICS.md); read it before selecting.

| # | Tactic | Run when |
|---|---|---|
| T1 | Wide dates × wide airports | Always |
| T2 | Date-shift table (±7 days) | Dates flexible by at least ±1 day |
| T3 | Split one-way tickets | Always |
| T4 | Alternative destination airport | Another airport sits within ≤3 h by road/rail of the destination |
| T5 | Multi-city / open-jaw | More than one city on the itinerary |
| T6 | Positioning flight | Long-haul (≳5 h) or an expensive route |
| T7 | Why-is-it-expensive analysis | Best price still above budget, or the user asked |
| T8 | Hidden-city / self-transfer | Only with "high" risk appetite — carry the risks from TACTICS.md into the report verbatim |

Done when: the selected tactic list has been told to the user.

## Step 4 — Scan

Start with the **Google Flights calendar**: it is the highest-yield single view. Fill the form, open the departure date picker, set the trip length ("N day trips" = N nights) with the arrows at its bottom, and page to the target month — one screenshot shows every departure day's cheapest round-trip price. Extend the view a week either side of the user's window and flag cheaper days just outside it. Pick the cheapest dates from there.

Then get the **baseline** (the standard search the user has in mind: direct or best-known route, A→B round trip, requested dates) and run each selected tactic. Reuse one results page: change only the origin/destination field between searches, and read results with `javascript_tool` (`document.body.innerText.split('Top departing flights')[1]`) instead of full-page text.

**Budget**: about 25 browser actions for the whole scan. Spend them baseline → calendar → tactics in table order; when the budget runs out, stop scanning and report what you have, listing the tactics left unscanned.

For each tactic record at least one verified price, or why no price could be obtained (site blocked, no route…). When sources disagree, note both.

Done when: the baseline and every selected tactic have a result row (or are listed as unscanned), and the winner's price is confirmed on a second source — ideally the airline's own booking flow.

## Step 5 — True total

For each of the top 3–5 candidates, fill in every line item of the true total. For transport use the cheapest reasonable airport↔centre option (bus/train).

**Baggage is the line item sources most often disagree on.** Aggregator bag filters, third-party sites and airline glossaries contradict each other, and fare-family names change ("Basic", "Light", "Essentials"…). Trust in this order: the airline's booking flow (fare-family comparison screen, reached without logging in) → Kiwi's bags-filter price difference on the same flight (see [SOURCES.md](SOURCES.md)) → the airline's official baggage page for that route type (domestic / international) → nothing else. When only aggregators are available, write the item as "unverified — check fare package at booking" rather than guessing.

Done when: every line item of every candidate is an amount, "0 (included)", or explicitly marked unverified with what the user must check.

## Step 6 — Report

1. **Recommendation** (1–2 lines): winning option, its true total, savings vs the baseline (amount and %).
2. **Comparison table**, ascending by true total; the first row is always the baseline:

| Option | Dates | Airline | Fare | Extras | **True total** | Duration / stops | Risk | Source, time |
|---|---|---|---|---|---|---|---|---|

3. **Why** (2–4 bullets): where the difference comes from — date, airport, ticket structure, season.
4. **Next step**: where to book (preferably the airline's own site, for easier changes/refunds), a price-alert suggestion, and a "buy now / wait" call — grounded in a price-history chart if you saw one, otherwise flagged as an estimate.

Add one line under the table noting that prices can change within minutes.
