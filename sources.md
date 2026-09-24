# Sources & methodology

[← Summary & recommendations](README.md)

Research date: **2026-09-24**. All counts, prices, and ratings are as of that date and will drift.

## The headline methodology finding

**NHTSA's official Ease-of-Use rating program covers only 126 car seats. None of the eight seats in this comparison are in it.**

Verified by pulling the complete rated dataset:

```
https://api.nhtsa.gov/childSeats?dataSet=ratings&max=100&offset=0
https://api.nhtsa.gov/childSeats?dataSet=ratings&max=100&offset=100
→ meta.pagination.total = 126
```

The rated set skews toward seats manufactured 2020–2022. Rotating convertibles are a recent category and are almost entirely unrated. The only rotating seats with official NHTSA ease-of-use scores are Baby Jogger City Turn, Cybex Sirona S, Nuna Revv, Joie Chili Spin 360, and UPPAbaby Knox — none of which you listed.

So "look up the safety rating" has no direct answer for these eight. What follows is what was substituted.

## What was used instead

### 1. NHTSA child seat complaint / recall / investigation database (primary)

```
https://api.nhtsa.gov/childSeats?dataSet=safetyIssues&max=100&offset={0..7700}
```

7,507 unique model records retrieved (of 7,653 reported; offsets 6020 and one adjacent page returned HTTP 500 and were partially backfilled at `max=20`). Each record carries `complaintsCount`, `recallsCount`, `investigationsCount`, plus full complaint text, recall summaries with unit counts and remedies, and investigation documents.

This is the closest thing to an objective safety and reliability signal available for these seats. It is a **passive reporting system** — counts scale with units sold and years on market, so raw comparisons between a 2025 launch and a 2017 platform are not apples to apples. Noted inline wherever it matters.

Notes on data quality:
- Chicco, Evenflo, Britax, and Joie use readable product model names. **Graco does not** — many Graco records use legacy model codes (`4EVERSL`, `4EVERSBT`) or generic buckets (`OTHER` with 199 complaints, `CHILD SAFETY SEAT` with 213). Per-model attribution for Graco is therefore less precise than for the other brands.
- Some models appear as multiple records (active and inactive variants, colour variants). Deduplicated by record `id`.

### 2. NHTSA Ease-of-Use ratings, used as brand and sibling proxies

Where a seat had no rating, the nearest rated relative was pulled and reported explicitly as a proxy, never as the seat's own score. Ratings are 1–5 across five axes: Overall, Securing the Child, Evaluation of Labels, Vehicle Installation Features, Evaluation of Instructions.

Proxies used:

| Target seat | Proxy used | Proxy relationship |
|---|---|---|
| [Joie Matcha Spin SI](joie-matcha-spin-si.md) | Joie Chili Spin 360 — **RF 5/5, FF 4/5** | Sibling rotating convertible, same platform family |
| [Graco 4Ever DLX Slim](graco-4ever-dlx-slim.md) | Graco 4Ever DLX — **RF 4/5, FF 5/5, booster 4/5** | Same platform, narrower variant. Closest proxy in this comparison |
| [Chicco Fit360](chicco-fit360-cleartex.md) | Chicco OneFit ClearTex (3/3/3), Chicco Fit4 (4/4/3) | Brand calibration only — different platforms |
| [Britax Galaxy 360](britax-galaxy-360-slim.md) | Britax Poplar (RF 4/5, FF 3/5) | Brand calibration only |
| [Evenflo Revolve360 Slim](evenflo-revolve360-slim.md) / [Extend](evenflo-revolve360-extend.md) | Evenflo All4One (4/4/3), EveryFit (4/5/3) | Brand calibration only |
| [Graco Turn2Me](graco-turn2me.md), [Graco EasyTurn 360](graco-easyturn-360.md) | Graco 4Ever DLX | Brand calibration only |

### 3. Car Seats for the Littles (csftl.org)

The CPST community's reference site. Full teardown reviews with newborn/preemie doll fit testing, harness slot measurements, belt-path assessments, and recommended-list status.

Reviews used:
- [Chicco Fit360 Rotating Convertible Car Seat Review](https://csftl.org/chicco-fit360-rotating-convertible-car-seat-review/)
- [Graco Turn2Me Multimode Car Seat Review](https://csftl.org/graco-turn2me-multimode-car-seat-review/) — including the **2024 update that removed it from the recommended list**
- [Evenflo Revolve360 Review](https://csftl.org/evenflo-revolve360-review/)
- [Graco 4Ever DLX Multimode Car Seat Review](https://csftl.org/graco-4ever-dlx-multimode-car-seat-review/)
- [Rotating Car Seats](https://csftl.org/rotating-car-seats/) — cross-category comparison chart

**Evidence gap:** CSFTL has no teardown for [Britax Galaxy 360 Slim](britax-galaxy-360-slim.md), [Joie Matcha Spin SI](joie-matcha-spin-si.md), or [Graco EasyTurn 360](graco-easyturn-360.md). Three of the eight seats have no independent expert install assessment. Flagged in each file.

CSFTL earns affiliate revenue via an Amazon storefront (disclosed on their site).

### 4. Retailer listings — specifications and owner review subscores

Target product pages supplied dimensions, weight, per-mode limits, expiration where published, star averages, **per-attribute subscores** (installation / comfort / quality / durability / value), star distributions, and "would recommend" percentages.

Two caveats applied throughout:
- **"Would recommend" is more informative than the star average.** Several seats here show a high star average with a low recommend rate — [Britax Galaxy 360](britax-galaxy-360-slim.md) at 4.11★ / 29%, [Evenflo Revolve360 Extend](evenflo-revolve360-extend.md) at 4.47★ / 55%, [Graco Turn2Me](graco-turn2me.md) at 4.54★ / 64%.
- **Target explicitly flags promotion-collected and syndicated reviews** on the Galaxy 360, Turn2Me, and both Evenflo seats. Star averages on those are inflated.

Manufacturer sites were partially unreachable from this environment: chiccousa.com and evenflo.com and britax.com returned 200; gracobaby.com and joiebaby.com returned 403. nhtsa.gov's web front end returned 403 (Akamai) while its API was fully accessible.

### 5. Regulatory context

- **FMVSS 213** — the federal child restraint standard (frontal crash, labelling, installation means, structural integrity).
- **FMVSS 213a** — the newer side-impact standard, mandatory for seats manufactured after **June 30, 2025**. Covers children to 40 lb / 43" in a 5-point harness.
- Only **[Britax Galaxy 360](britax-galaxy-360-slim.md)** and **[Graco Turn2Me](graco-turn2me.md)** cite 213a explicitly. The others make generic side-impact claims using marketing terms (Graco "ProtectPlus Engineered," Evenflo "L.I.F.E. Guard"). **Check the manufacture date sticker on any seat you buy** — that, not the marketing copy, determines which standard it was built to.

## What was not available

- **Consumer Reports crash-test ratings** — paywalled. CR runs its own more severe crash protocol and rates seats independently; worth a month's subscription before a $300+ purchase.
- **IIHS booster ratings** — IIHS rates boosters for belt fit but does not rate harnessed convertible seats, so it does not cover these.
- **NHTSA ease-of-use for any of the eight** — does not exist, as above.
- Independent expert teardowns for three of eight seats (see CSFTL note).

## How the 1–5 scores in these notes were assigned

The per-seat scores are **mine, not any agency's**. They are a transparent weighting of the evidence above, not a measurement. Basis for each axis:

| Axis | Built from |
|---|---|
| **Safety** | Open investigations, recalls (weighted by severity and unit count), injury reports, FMVSS 213a citation, structural features (anti-rebound panel, one-piece base, true lock-off), rear-facing weight limit, NHTSA ease-of-use score where one exists |
| **Reliability** | NHTSA complaint count adjusted for exposure, complaint trend direction, CSFTL recommended-list status, "would recommend" rate, one-star share, durability/quality subscores |
| **Ease of use** | Installation subscore, install mechanism quality, CSFTL install findings, recline adjustability, cover removal, harness range, weight |
| **Features / longevity** | Number of modes, weight/height ceilings, years of usable life, age cap, width, FAA approval |
| **Value** | Price ÷ realistic years of use, cross-checked against the value subscore owners assigned |

Cost-per-year uses the shorter of expiry and age cap, since an age cap ends usability regardless of the expiry date. This is why [Chicco Fit360](chicco-fit360-cleartex.md) scores badly — an 8-year expiry with a 6-year age cap is 6 years of use.

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

`dataSet` accepts `ratings`, `safetyIssues`, or `all`. The API ignores `make`, `keyword`, and `productModel` filter parameters — you must page the whole set and filter client-side. `offset=6020` returns HTTP 500 and needs backfilling at a smaller `max`.

Key campaign and action numbers referenced in these notes:

| ID | What |
|---|---|
| **AQ24001** | Open OVSC investigation into Evenflo Revolve360 FMVSS 213 compliance and shell/base separation |
| 25C010000 | Evenflo Revolve360 Slim / Gold Slim — headrest foam choking hazard, 324,997 units |
| 26C004000 | Evenflo Reo by Revolve360 — headrest shifts in a crash, FMVSS 213, 59,661 units |
| 26C005000 | Evenflo Revolve180 Litemax NXT — miscalibrated recline indicator, FMVSS 213, 35,951 units |
| 25C011000 | Evenflo All4One — rear-facing recline may shift, FMVSS 213, 74,710 units |
| 25C005000 | Chicco MyFit Zip Air — excessive chest movement, FMVSS 213, 30,984 units |
| 26C003000 | Graco SnugRide Turn & Slide — carrier may detach from base, FMVSS 213, 5,126 units |
| 26C006000 | Britax B-Safe family — carrying handle may detach, 114,660 units |

## Caveats to carry into any decision

1. **Complaint counts are exposure-weighted, not risk-weighted.** A clean record on a seat launched in 2025 says less than a clean record on a platform selling since 2017.
2. **Absence of a recall is not evidence of compliance.** The [Chicco Fit360](chicco-fit360-cleartex.md)'s chest clip has 26 complaints and no recall. NHTSA opens investigations when volume crosses a threshold; it has not here yet.
3. **A good ease-of-use rating does not predict compliance.** Evenflo's All4One scored 4/5 rear-facing from NHTSA and was later recalled for a rear-facing recline mechanism that shifts out of position.
4. **Manufacture date matters more than model name.** 213a applies by build date. Recall ranges are defined by build date. Check the sticker.
5. **Register whatever you buy.** Recall notification depends on it, and two of the recalls above were themselves for missing registration cards.
6. **A free CPST install check beats every spec in these notes.** Most installs that fail in the field fail because of installation, not design.

---

[← Summary & recommendations](README.md)
