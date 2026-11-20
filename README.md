# Technical Test – Data Scientist, Pricing & Revenue Management

## Context & instructions

You have just joined the Data Science team of a group operating campsites across Europe (France, Spain, Italy, Croatia, the Netherlands). The Revenue Management team sets nightly prices for each accommodation type and wants to move towards data-driven dynamic pricing. This test reproduces, on a small synthetic dataset, a few tasks you would face in your first weeks.

- **Duration: 75 minutes**, from the moment you receive this brief. Parts are timed as a guide. Not finishing everything is normal and expected: we prefer fewer questions done well, with your reasoning visible, over everything done quickly.
- **Deliverables:** one `answers.sql` file (Part A), one notebook or `.py` script (Parts B, C and D) and your written answers (as markdown in the notebook, or in a short `ANSWERS.md`). Send everything back as a single zip.
- **Tools:** any SQL engine. We recommend [DuckDB](https://duckdb.org), which queries the CSV files directly (`SELECT * FROM 'bookings.csv'`). BigQuery syntax is welcome too. For Python, use pandas, numpy, statsmodels, scikit-learn or equivalents.
- **AI tools:** allowed **only as coding assistants** (syntax, debugging, library usage, autocompletion). The analysis, the interpretation of results, the choice of approach and every written answer must be your own: do not use AI tools to analyse the data, interpret outputs or brainstorm approaches. We rely on your honesty here, and we will discuss your submission with you in the next interview.
- **Assumptions:** when something is ambiguous, make a reasonable assumption and write it down in one line. This is part of what we assess.
- **Language:** answers in English.

## The data

Five CSV files describe 12 campsites over the 2024 and 2025 seasons (open 1 April – 31 October). The data is synthetic. `bookings.csv` is a raw extract from the booking system: it has not been cleaned.

| File | Grain | Columns |
|---|---|---|
| `sites.csv` | 1 row per campsite (12) | site_id, site_name, country, region, region_type (coastal / countryside / mountain), stars |
| `inventory.csv` | 1 row per campsite × accommodation type (48) | site_id, accommodation_type, units (number of rentable units) |
| `bookings.csv` | 1 row per booking (~85,000) | booking_id, site_id, accommodation_type, booking_date, arrival_date, departure_date, total_price_eur (whole stay), channel (direct / ota), customer_country, status (confirmed / cancelled), cancellation_date |
| `daily_prices.csv` | 1 row per campsite × accommodation type × night (~20,500) | site_id, accommodation_type, stay_date, price_eur (nightly price published by Revenue Management) |
| `price_tests.csv` | 1 row per campsite × accommodation type × week in a 2025 pricing test (206) | site_id, accommodation_type, week_start (Monday), price_multiplier (0.85, 1.00 or 1.15) |

Definitions used in the questions:

- **Room-night:** one unit occupied for one night. A stay from 1 to 8 July is 7 room-nights (1st to 7th).
- **Occupancy rate:** occupied room-nights ÷ available room-nights (units × nights open).
- **On the books at date D:** bookings made on or before D and not cancelled at D (a booking cancelled after D was still on the books at D).
- **Price tests:** in 2025, for some shoulder-season weeks (May, June, September), the published price of a campsite × accommodation type was multiplied for the whole week by a factor drawn at random among 0.85, 1.00 and 1.15. `daily_prices.csv` already includes this multiplier.

## Part A – SQL (20 min)

Write each query in `answers.sql`, with a one-line comment stating any assumption. Queries should run as written on the CSV files.

**A1 – Weekly occupancy.** For 2025, compute the occupancy rate per campsite and per week, together with its 4-week rolling average. Note that a booking covers several nights, and that a stay can span two weeks.

**A2 – Booking pace.** Revenue managers compare the bookings on the books with last year at the same date. For each campsite, compute the room-nights falling in July 2025 that were on the books as of 1 May 2025, the same figure for July 2024 as of 1 May 2024, and the percentage difference.

## Part B – Python for data science (15 min)

We will read your code as we would in a code review: structure, readability and reusability count as much as the result.

**B1 – Data quality.** Load `bookings.csv` and check its quality. List the issues you find, how many rows each affects, and how you treat them (fix, exclude or flag), in a short table or a few bullets.

**B2 – Daily demand table.** Build a table with one row per campsite × accommodation type × night, containing: units, room-nights sold (confirmed bookings), occupancy rate, arrivals that night and the published price. It will be used in Part D.

## Part C – Forecasting (15 min)

**C1 – September occupancy forecast.** Put yourself on **1 July 2025**: you may only use the information available on that date (all 2024 data, and 2025 bookings as they stood on 1 July 2025). Forecast the room-nights that will be sold in **September 2025** for each campsite × accommodation type.

- Build a simple baseline and at least one better method of your choice.
- Then, and only then, compare your forecasts with what actually happened (the full 2025 data) using an error metric you justify.
- Conclude in two or three sentences written for the Revenue Management director: which method you would use, and how far they can trust it.

## Part D – Statistics & pricing (25 min)

Show your code and write your interpretation in plain words. Rigour in the reasoning matters more than sophistication of the method.

**D1 – A first look at price sensitivity.** Using your table from B2, estimate the relationship between price and demand with a log-log regression of demand on price (choose and justify the demand measure and any controls). Report the estimated elasticity. Does the result make economic sense? Explain what is going on.

**D2 – Using the price tests.** Using `price_tests.csv`, estimate the price elasticity of demand for shoulder-season weeks. Give an uncertainty range, compare with D1, and state the assumptions behind your estimate and its limits (for instance, can it be applied to August?).

**D3 – From model to price (written, 8 lines maximum).** The Revenue Management director asks you how you would recommend nightly prices for summer 2026. Describe your approach: what you would forecast, what you would optimise, which business constraints you would include, and how you would validate the approach before rolling it out to all campsites.

## Submission checklist

- [ ] `answers.sql` with A1, A2 and one-line assumptions
- [ ] Notebook or `.py` script for Parts B, C and D, runnable from the CSV files
- [ ] Written answers: data quality summary (B1), forecast conclusion (C1), interpretation of D1 and D2, approach for D3
- [ ] A note on what you did not have time to finish and how you would have done it
- [ ] A line describing how you used AI coding assistants, if at all
- [ ] Time actually spent

Please send everything back as a single zip within 75 minutes of receiving this brief.
