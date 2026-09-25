# Sources & methodology

[← Summary & recommendations](README.md)

Research dates: **2026-09-24**. All counts, prices, and ratings are as of that date and will drift.

Scope: **51 convertible car seats available on Target.com.** "Convertible" = converts rear-facing → forward-facing. Includes 2-in-1 / 3-in-1 / 4-in-1 / all-in-one seats with booster modes, rotating and non-rotating. Excludes infant carriers, harness-to-booster combination seats (forward-facing only), belt-positioning boosters, and travel systems.

## The headline methodology finding

**NHTSA's official Ease-of-Use rating program covers only 126 car seats. Of the 51 seats here, 14 have a real federal rating** (plus 2 more that inherit a platform sibling's). The other 35 have none.

Verified by pulling the complete rated dataset:

```
https://api.nhtsa.gov/childSeats?dataSet=ratings&max=100&offset=0
https://api.nhtsa.gov/childSeats?dataSet=ratings&max=100&offset=100
→ meta.pagination.total = 126
```

The rated set skews toward seats manufactured 2020–2022, so newer designs — especially rotating convertibles — are largely unrated. **Only five rotating seats are rated anywhere in the federal dataset:** Baby Jogger City Turn, Cybex Sirona S, Nuna Revv, Joie Chili Spin 360, and Maxi-Cosi Emme 360. Of the 16 rotating seats on Target, two are rated ([Baby Jogger City Turn](babyjogger-city-turn.md), [Joie Chili Spin SI](joie-chili-spin-si.md)).

So "look up the safety rating" has a direct answer for 14 of 51 seats. What follows is what was used for the rest.

## How the Target catalogue was enumerated

Target's category browse URLs returned 404 from this environment, so the catalogue was built from paginated search:

```
https://www.target.com/s?searchTerm=convertible+car+seat&Nao={0,24,48,72,96,120}
https://www.target.com/s?searchTerm=all-in-one+car+seat&Nao=0
https://www.target.com/s?searchTerm=rear+facing+convertible+car+seat&Nao=0
```

Target reported ~200–219 results per query across 9–10 pages, heavily mixed with boosters, infant seats, travel systems and accessories. Results were deduplicated (many seats appear under multiple colourway TCINs) and filtered to true convertibles. Pages 4+ returned almost entirely boosters and infant seats, so enumeration stopped there.

**Known limitation:** this is a search-derived catalogue, not an authoritative category dump. A convertible that ranks poorly for all three search terms could be missing. Colour variants of included seats are collapsed to one entry each.

Every seat file links its Target product page directly.

## Primary data sources

### 1. NHTSA child seat complaint / recall / investigation database

```
https://api.nhtsa.gov/childSeats?dataSet=safetyIssues&max=100&offset={0..7700}
```

7,507 unique model records retrieved of 7,653 reported (offset 6020 and one adjacent page returned HTTP 500; partially backfilled at `max=20`). Each record carries `complaintsCount`, `recallsCount`, `investigationsCount`, full complaint text, recall summaries with unit counts and remedies, and investigation documents.

This is the closest thing to an objective safety and reliability signal available. It is a **passive reporting system** — counts scale with units sold and years on market, so a 2025 launch and a 2017 platform are not comparable on raw counts. Flagged inline wherever it matters.

Data-quality notes:
- Chicco, Evenflo, Britax, Joie, Maxi-Cosi, Baby Trend, Baby Jogger, UPPAbaby and Peg Perego use readable product model names.
- **Graco does not.** Many Graco records use legacy codes (`4EVERSL`, `4EVERSBT`) or generic buckets (`OTHER` with 199 complaints, `CHILD SAFETY SEAT` with 213). Per-model attribution for Graco is less precise than for other brands.
- **Disney Baby seats appear under make `DISNEY BABY`** with Safety 1st model names, confirming they are Dorel rebadges.
- Models appear as multiple records (active/inactive, colour variants). Deduplicated by record `id`.
- Seats matched by brand + model-name prefix. A "0 complaints" result can mean "no record exists yet" (new listings) rather than "clean" — called out per seat where relevant.

### 2. NHTSA Ease-of-Use ratings

Ratings are 1–5 on five axes: Overall, Securing the Child, Evaluation of Labels, Vehicle Installation Features, Evaluation of Instructions — per mode.

Where a seat had no rating of its own, the nearest rated relative was reported **explicitly as a proxy**, never as the seat's own score:

| Seat | Proxy | Relationship |
|---|---|---|
| [Graco 4Ever DLX Slim](graco-4ever-dlx-slim.md) | Graco 4Ever DLX (4/5/4) | Same platform, narrower variant |
| [Britax Poplar S](britax-poplar-s.md) | Britax Poplar (4/3) | Same platform, trim variant |
| [Joie Matcha Spin SI](joie-matcha-spin-si.md) | Joie Chili Spin 360 (**5**/4) | Sibling rotating platform |
| [Disney Grow and Go](disney-grow-and-go.md) | Safety 1st Grow and Go (2/3/3) | Identical rebadge |
| [Disney EverSlim](disney-everslim.md) | Safety 1st SlimRide EverSlim (2/3/3) | Identical rebadge |
| [Chicco Fit360](chicco-fit360-cleartex.md), [Zip](chicco-fit360-zip.md), [Fit3x](chicco-fit3x.md), [OneFit Max](chicco-onefit-max.md) | Chicco OneFit ClearTex (3/3/3), Fit4 (4/4/3) | Brand calibration only |
| [Britax Galaxy 360](britax-galaxy-360-slim.md), [One4Life](britax-one4life.md), [Slim](britax-one4life-slim.md) | Britax Poplar (4/3) | Brand calibration only |
| Evenflo seats | Evenflo All4One (4/4/3), EveryFit (4/5/3) | Brand calibration only |
| [Graco Turn2Me](graco-turn2me.md), [EasyTurn 360](graco-easyturn-360.md), [SlimFit3 LX](graco-slimfit3-lx.md), [Extend2Fit](graco-extend2fit-2in1.md) | Graco 4Ever DLX, SlimFit, TriRide | Brand calibration only |
| [Maxi-Cosi Andi](maxicosi-andi.md), [Kani](maxicosi-kani.md), [Pria Max](maxicosi-pria-max.md) | Maxi-Cosi Pria All in One (3/3/3), Romi (3/3) | Brand calibration only |
| [Joie Saffron SI](joie-saffron-si.md), [Pepper Spin SI](joie-pepper-spin-si.md) | Joie Chili Spin 360 (**5**/4) | Brand calibration |
| [Safety 1st](safety1st-turn-and-go-360.md) rotating seats | Grow and Go (2/3/3), EverSlim (2/3/3), Trimate (2/3/3) | Brand calibration only |
| [Peg Perego](pegperego-primo-viaggio-kinetic.md) seats | **none** — NHTSA rates only Peg Perego *boosters* | No proxy available |
| [UPPAbaby Rove](uppababy-rove.md) | UPPAbaby Knox (rated, not on Target) | Brand only |

**The cross-cutting finding from the rated set:** brands document seats reasonably well (instructions and labels often 4–5/5) and consistently fail on **vehicle-installation features** — seven of the fourteen rated seats here score 1/5 or 2/5 there. That is the axis that determines whether a seat ends up tight in the car.

### 3. Car Seats for the Littles (csftl.org)

The CPST community's reference site. Full teardowns with newborn/preemie doll fit testing, harness slot measurements, belt-path assessments, and recommended-list status.

Reviews used — 15 teardowns covering 20 of the 51 seats (platform siblings share a review):
- [Chicco Fit360](https://csftl.org/chicco-fit360-rotating-convertible-car-seat-review/) · [Chicco OneFit](https://csftl.org/chicco-onefit-all-in-one-review/)
- [Graco Turn2Me](https://csftl.org/graco-turn2me-multimode-car-seat-review/) — incl. the **2024 update removing it from the recommended list** · [Graco 4Ever DLX](https://csftl.org/graco-4ever-dlx-multimode-car-seat-review/) · [Graco SlimFit3 LX](https://csftl.org/graco-slimfit3-lx-review/) · [Graco TriRide](https://csftl.org/graco-triride-multimode-car-seat-review/)
- [Britax Poplar](https://csftl.org/britax-poplar-convertible-car-seat-review/) · [Britax One4Life Slim](https://csftl.org/britax-one4life-slim-multimode-car-seat-review/)
- [Safety 1st EverSlim / SlimRide](https://csftl.org/safety-1st-everslim-and-slimride-multimode-car-seat-review/) · [Safety 1st Trimate](https://csftl.org/safety-1st-trimate-multimode-car-seat-review/) · [Safety 1st Turn and Go](https://csftl.org/safety-1st-turn-and-go-multimode-car-seat-review/) · [Safety 1st Grow and Go Extend n Ride](https://csftl.org/safety-1st-grow-and-go-extend-n-ride-multimode-car-seat-review/)
- [Maxi-Cosi Pria Max](https://csftl.org/maxi-cosi-pria-max-3-in-1-multimode-car-seat-review/) · [UPPAbaby Rove](https://csftl.org/uppababy-rove-convertible-car-seat-review/) · [Baby Jogger City Turn](https://csftl.org/baby-jogger-city-turn-convertible-car-seat-review/)
- [Baby Trend Cover Me](https://csftl.org/baby-trend-cover-me-multimode-car-seat-review/) · [Baby Trend Trooper](https://csftl.org/baby-trend-trooper-convertible-car-seat-review/)
- [Evenflo Revolve360](https://csftl.org/evenflo-revolve360-review/)
- [Rotating Car Seats](https://csftl.org/rotating-car-seats/) — cross-category comparison chart
- [Car Seats have an expiration date!](https://csftl.org/car-seats-have-an-expiration-date/) and [Car Seats: Why do they Expire?](https://csftl.org/expired-seats/) — expiry reference; CSFTL gives the industry range as **4–12 years** and notes lifespan "is often listed in the manual as well"

**20 of 51 seats have a CSFTL teardown**; 31 do not, and those have no independent expert install assessment. Flagged per seat. CSFTL reviews were located via their WordPress search API (`csftl.org/wp-json/wp/v2/search?search=<model>`), which surfaced substantially more coverage than site search did.

CSFTL earns affiliate revenue via an Amazon storefront (disclosed on their site).

### 4. Target listings — specifications and owner review subscores

Supplied dimensions, weight, per-mode limits, install mechanism, expiry where published, star averages, **per-attribute subscores** (installation / comfort / quality / durability / value), star distributions, and "would recommend" percentages.

Product pages were fetched individually for the 8 originally-listed seats plus 12 of the highest-consequence additions. For the remaining seats, per-mode limits come from NHTSA's own `weightRange` / `heightRange` / per-mode fields, and specs not obtainable are marked as such in the seat file.

Two caveats applied throughout:
- **"Would recommend" is more informative than the star average.** Several seats show a high star average with a low recommend rate — [Britax Galaxy 360](britax-galaxy-360-slim.md) 4.11★ / 29%, [Britax One4Life](britax-one4life.md) 4.03★ / 28%, [Evenflo Revolve360 Extend](evenflo-revolve360-extend.md) 4.47★ / 55%, [Graco Turn2Me](graco-turn2me.md) 4.54★ / 64%.
- **Target explicitly flags promotion-collected and syndicated reviews** on the Galaxy 360, Turn2Me, SlimFit3 LX, UPPAbaby Rove and both Evenflo rotating seats. Star averages there are inflated.
- Several listings **contradict themselves** on forward-facing weight or height limits. Noted per seat; verify against the manual.

Reachability from this environment: chiccousa.com, evenflo.com, britax.com, csftl.org and target.com returned 200. gracobaby.com and joiebaby.com returned 403. nhtsa.gov's web front end returned 403 (Akamai) while its API was fully accessible. Google, Bing, DuckDuckGo and Mojeek were unusable or bot-challenged.

### 5. Seat expiration

**Every car seat has a manufacturer-stated expiration; only 9 of the 51 Target listings publish it.** Expiry figures in these notes come from CSFTL teardowns (which read it off the seat label and the manual) and from the listings that state it. 24 seats are verified, 13 inherit a verified platform sibling's figure, and 14 could not be verified from available sources — those are marked "label + manual" rather than "not stated," since the manufacturer does specify it.

Verified distribution: **21 of 24 are 10 years.** Exceptions: [Baby Trend Trooper Slim](babytrend-trooper-slim.md) 7 years, [Baby Trend Cover Me](babytrend-cover-me.md) 7 years harnessed / 9 years in booster, [Chicco Fit360](chicco-fit360-cleartex.md) 8 years.

Manufacturer FAQ pages were not reachable from this environment (chiccousa.com FAQ covers ordering only; britax.com and evenflo.com expiry URLs returned 404; joiebaby.com returned 403), so brand-level policy pages could not be used as a cross-check.

**Age caps can be shorter than expiry** and override it: [Chicco Fit360](chicco-fit360-cleartex.md)/[Zip](chicco-fit360-zip.md) 6 years, [Evenflo Revolve360 Slim](evenflo-revolve360-slim.md) 6 years, [Britax Galaxy 360](britax-galaxy-360-slim.md) 6 years. Cost-per-year uses the shorter figure.

### 6. Regulatory context

- **FMVSS 213** — the federal child restraint standard (frontal crash, labelling, installation means, structural integrity).
- **FMVSS 213a** — side-impact standard, mandatory for seats manufactured after **June 30, 2025**. Covers children to 40 lb / 43" in a 5-point harness. Britax discloses this scope limit explicitly; most brands do not.
- **FMVSS 213b** — updated frontal/general standard. Only the three Joie SI seats claim it.

**Only 9 of 51 seats cite 213a on their listing:** [Graco 4Ever DLX](graco-4ever-dlx.md), [Graco Turn2Me](graco-turn2me.md), [Britax Galaxy 360](britax-galaxy-360-slim.md), [Britax One4Life](britax-one4life.md) + [Slim](britax-one4life-slim.md), [Chicco Fit3x](chicco-fit3x.md), [Chicco OneFit Max](chicco-onefit-max.md), [Chicco Fit360 Zip](chicco-fit360-zip.md), and the three Joie SI seats. Everything else makes a generic side-impact claim or none. **Check the manufacture date sticker** — that, not the marketing copy, determines which standard the seat was built to.

## What was not available

- **Consumer Reports crash-test ratings** — paywalled. CR runs a more severe crash protocol and rates seats independently; worth a month's subscription before a $300+ purchase.
- **IIHS booster ratings** — IIHS rates boosters for belt fit but does not rate harnessed convertibles.
- **NHTSA ease-of-use for 35 of the 51 seats** — does not exist.
- **Independent expert teardowns for 31 of 51 seats.**
- **Target specs for a handful of seats** (Maxi-Cosi Andi / Kani / Pria Max, both Peg Perego seats) — flagged in those files as unverified.

## How the 1–5 scores were assigned

The per-seat scores are **mine, not any agency's**. They are a transparent weighting of the evidence above, not a measurement.

| Axis | Built from |
|---|---|
| **Safety** | Open investigations, recalls (weighted by severity and unit count), injury reports, FMVSS 213a/213b citation, structural features (anti-rebound panel, one-piece base, true lock-off, SIP pods), rear-facing weight limit, NHTSA ease-of-use score where one exists |
| **Reliability** | NHTSA complaint count adjusted for exposure, complaint trend direction, CSFTL recommended-list status, "would recommend" rate, one-star share, durability/quality subscores |
| **Ease of use** | Installation subscore, install-mechanism quality, NHTSA vehicle-installation-features score, CSFTL install findings, recline adjustability, cover removal, harness range, weight |
| **Features / longevity** | Number of modes, weight/height ceilings, years of usable life, age cap, width, FAA approval |
| **Value** | Price ÷ realistic years of use, cross-checked against the value subscore owners assigned |

Cost-per-year uses the shorter of expiry and age cap, since an age cap ends usability regardless of expiry date. That is why [Chicco Fit360](chicco-fit360-cleartex.md) scores badly — an 8-year expiry with a 6-year age cap is 6 years of use.

Scores are not weighted equally in the recommendations. [Graco 4Ever DLX](graco-4ever-dlx.md) tops the raw total but does not rotate; [Joie Chili Spin SI](joie-chili-spin-si.md) has the best federal rating in the rotating category but a 4.08★ owner average. The seat file always carries more information than the number.

## Reproducing the data

```bash
# Full NHTSA ease-of-use rated dataset (126 seats)
curl -s -A "Mozilla/5.0" \
  "https://api.nhtsa.gov/childSeats?dataSet=ratings&max=100&offset=0"

# Full complaint/recall/investigation dataset (~7,650 records, paginate at max=100)
curl -s -A "Mozilla/5.0" \
  "https://api.nhtsa.gov/childSeats?dataSet=safetyIssues&max=100&offset=0"

# Single recall campaign detail
curl -s -A "Mozilla/5.0" \
  "https://api.nhtsa.gov/recalls/campaignNumber?campaignNumber=25C010000"
```

`dataSet` accepts `ratings`, `safetyIssues`, or `all`. The API **ignores** `make`, `keyword` and `productModel` filter parameters — page the whole set and filter client-side. `offset=6020` returns HTTP 500 and needs backfilling at a smaller `max`.

### Campaign and action numbers referenced in these notes

| ID | What | Units |
|---|---|:--:|
| **AQ24001** | **Open** OVSC investigation — Evenflo Revolve360 FMVSS 213 compliance and shell/base separation | — |
| 14C003000 | Evenflo convertibles incl. Symphony — buckle difficult to unlatch | **1,368,649** |
| 16C002000 | Graco Extend2Fit — recline label misplaced, FMVSS 213 | 15,064 |
| **21C003000** | **Maxi-Cosi Pria 85 — head injury with lap-belt-only FF install, FMVSS 213. "No remedy has been established at this time."** | **119,863** |
| 21C004000 | Maxi-Cosi Pria 70 — same defect; remedied with labels + free booster | 4,000 |
| 23C005000 | Evenflo — missing warning labels, FMVSS 213 | 1 |
| 24C001000 | Britax One4Life Slim — incorrect max RF lower-anchor weight on labels, FMVSS 213 | 1,286 |
| 25C003000 | Evenflo — missing recall registration card, FMVSS 213 | 1,068 |
| 25C005000 | Chicco MyFit Zip Air — excessive chest movement, FMVSS 213 | 30,984 |
| 25C008000 | Evenflo Titan 65 — wrong instruction manual, FMVSS 213 | 36,694 |
| **25C010000** | **Evenflo Revolve360 Slim / Gold Slim — headrest foam choking hazard** | **324,997** |
| 25C011000 | Evenflo All4One — RF recline may shift, FMVSS 213 | 74,710 |
| 26C002000 | Evenflo — incorrect Spanish manual, FMVSS 213 | 14,020 |
| 26C003000 | Graco SnugRide Turn & Slide — carrier may detach from base, FMVSS 213 | 5,126 |
| **26C004000** | **Evenflo Reo by Revolve360 — headrest shifts in a crash, FMVSS 213; remedy "under development"** | **59,661** |
| 26C005000 | Evenflo Revolve180 Litemax NXT — miscalibrated recline indicator, FMVSS 213 | 35,951 |
| 26C006000 | Britax B-Safe family — carrying handle may detach | 114,660 |

## Caveats to carry into any decision

1. **Complaint counts are exposure-weighted, not risk-weighted.** A clean record on a seat launched in 2025 says less than a clean record on a platform selling since 2017.
2. **Absence of a recall is not evidence of compliance.** [Chicco Fit360](chicco-fit360-cleartex.md)'s chest clip has 26 complaints and no recall; [Britax Poplar](britax-poplar.md)'s backwards recline indicator has three and no recall; [Evenflo Symphony](evenflo-symphony-extend-breeze.md)'s harness fraying has 81 and no current recall.
3. **A good ease-of-use rating does not predict compliance.** Evenflo's All4One scored 4/5 rear-facing from NHTSA and was later recalled (25C011000) for a rear-facing recline mechanism that shifts out of position.
4. **A good NHTSA rating does not predict owner satisfaction, and vice versa.** [Joie Chili Spin SI](joie-chili-spin-si.md) has the best rotating-seat federal rating and a 4.08★ average. [Graco TriRide](graco-triride-3in1.md) scores 4/5 in all three modes and 66% would recommend. NHTSA measures documentation and hardware, not comfort or harness pull force.
5. **Manufacture date matters more than model name.** 213a applies by build date; recall ranges are defined by build date.
6. **Register whatever you buy.** Two of the recalls above were themselves for missing registration cards.
7. **A free CPST install check beats every spec in these notes.** Most installs that fail in the field fail because of installation, not design — and NHTSA scored install features 1/5 or 2/5 on half the rated seats here.

---

[← Summary & recommendations](README.md)
