# Technical Test – Data Scientist, Pricing & Revenue Management

## Context & instructions

You have just joined the Data Science team of a group operating campsites across Europe (France, Spain, Italy, Croatia, the Netherlands). The Revenue Management team sets nightly prices for each accommodation type and wants to move towards data-driven dynamic pricing. This test reproduces, on a small synthetic dataset, a few tasks you would face in your first weeks.

- **Duration: 120-150 minutes**, from the start of your slot (when we send you the link to this repository and to the shared drive). Suggested timing: 5 min to read and set up, Part A 10 min, Part B 15 min, Part C 15 min, Part D 20 min, 10 min to finalise and upload. Not finishing everything is normal: we prefer fewer questions done well, with your reasoning visible, over everything done quickly.
- **Deliverables:** one notebook or `.py` script (Parts A, C and D) with your written answers (as markdown in the notebook, or in a short `ANSWERS.md`), and one `answers.sql` file (Part B), submitted as a single zip file (see *Submitting your work* at the end). No slides today: if you move to the next step, we will ask you to present your findings to the Revenue Management director.
- **Environment:** a `requirements.txt` is provided. Create a fresh environment with Python 3.10 to 3.13 and run `pip install -r requirements.txt` (uv or conda are fine too). If you add a package, add it to `requirements.txt` with its exact version (`package==x.y.z`). We will run your code from a fresh environment built from your `requirements.txt`, so make sure it runs that way.
- **Tools:** any SQL engine. We recommend [DuckDB](https://duckdb.org), which queries the CSV files directly (`SELECT * FROM 'data/bookings.csv'`). BigQuery syntax is welcome too. For Python, use pandas, numpy, statsmodels, scikit-learn or equivalents.
- **AI tools:** allowed **only as coding assistants** (syntax, debugging, library usage, autocompletion). The analysis, the interpretation of results, the choice of approach and every written answer must be your own: do not use AI tools to analyse the data, interpret outputs or brainstorm approaches. We rely on your honesty here, and we will review your submission with you in the next interview.
- **Assumptions:** when something is ambiguous, make a reasonable assumption and write it down in one line. This is part of what we assess.
- **Language:** answers in English.

## The data

Five CSV files describe 12 campsites over the 2024 and 2025 seasons (open 1 April – 31 October). The data is synthetic.

| File | Grain | Columns |
|---|---|---|
| `sites.csv` | 1 row per campsite (12) | site_id, site_name, country, region, region_type (coastal / countryside / mountain), stars |
| `inventory.csv` | 1 row per campsite × accommodation type (48) | site_id, accommodation_type, units (number of rentable units) |
| `bookings.csv` | 1 row per booking (~67,000) | booking_id, site_id, accommodation_type, booking_date, arrival_date, departure_date, total_price_eur (whole stay), channel (direct / ota), customer_country, status (confirmed / cancelled), cancellation_date |
| `daily_demand.csv` | 1 row per campsite × accommodation type × night open (~20,500) | site_id, accommodation_type, stay_date, units, room_nights_sold, occupancy, arrivals, price_eur (nightly price published by Revenue Management) |
| `price_tests.csv` | 1 row per campsite × accommodation type × week in a 2025 pricing test (~260) | site_id, accommodation_type, week_start (Monday), price_multiplier (0.85, 1.00 or 1.15) |

`bookings.csv` is a raw extract from the booking system: it has not been cleaned. `daily_demand.csv` is maintained by the data engineering team from the cleaned booking system: `room_nights_sold` and `arrivals` count the final, confirmed bookings only.

Definitions used in the questions:

- **Room-night:** one unit occupied for one night. A stay from 1 to 8 July is 7 room-nights (1st to 7th).
- **Occupancy rate:** occupied room-nights ÷ available room-nights (units × nights open).
- **On the books at date D:** bookings made on or before D and not cancelled at D (a booking cancelled after D was still on the books at D).
- **Price tests:** in 2025, for some shoulder-season weeks (May, June, September), the published price of a campsite × accommodation type was multiplied for the whole week by a factor drawn at random among 0.85, 1.00 and 1.15. `price_eur` in `daily_demand.csv` already includes this multiplier.

## Part A – Data quality (10 min)

We will read your code as we would in a code review: structure, readability and reusability count as much as the result.

**A1 – Data quality.** Load `bookings.csv` and check its quality. List the issues you find, how many rows each affects, and how you treat them (fix, exclude or flag), in a short table or a few bullets. Reuse this cleaning in Parts B and C.

## Part B – SQL (15 min)

Write the query in `answers.sql`, with one-line comments stating any assumption. It should run as written on the CSV files.

**B1 – Booking pace.** Revenue managers compare the bookings on the books with last year at the same date. For each campsite, compute the room-nights falling in July 2025 that were on the books as of 1 May 2025, the same figure for July 2024 as of 1 May 2024, and the percentage difference. Note that a booking covers several nights, and that a stay can start in June and end in July.

## Part C – Forecasting (15 min)

Revenue managers forecast with the **pick-up** method: final demand = what is already on the books today + what will still be booked between today and the stay (the pick-up), estimated from last year.

**C1 – September forecast.** Put yourself on **1 July 2025**. For each campsite × accommodation type, forecast the room-nights that will be sold in **September 2025** with the pick-up method, and with a naive "same as last year" forecast. Only use information available on 1 July 2025. Compare both forecasts with what actually happened, using an error metric you justify. How did you estimate the pick-up from last year, and why? Which forecast would you give Revenue Management, how far can they trust it, and in which situations would it break down?

## Part D – Pricing (20 min)

Show your code and write your interpretation in plain words. Rigour in the reasoning matters more than sophistication of the method.

**D1 – Price sensitivity from history.** Using `daily_demand.csv`, estimate the price elasticity of demand with a log-log regression (choose and justify the demand measure and any controls). Report and interpret the estimate.

**D2 – Price sensitivity from the tests.** Using `price_tests.csv` and `daily_demand.csv`, estimate the price elasticity of demand. Give an uncertainty range, compare with D1, and state the assumptions behind your estimate and where it can and cannot be used.

## Submitting your work

1. Get the material: `git clone` this repository, or use *Code → Download ZIP* on the repository page.
2. Put your work in one folder named `firstname-lastname` (lowercase, no accents, e.g. `marie-dupont`) containing:
   - your notebook or `.py` script for Parts A, C and D
   - `answers.sql`
   - `ANSWERS.md` (if your written answers are not in the notebook)
   - `requirements.txt` (updated if you added packages)
3. Zip it as `firstname-lastname.zip` (do not include the data files or your virtual environment) and upload it to the shared drive link we sent you, **within 75 minutes** of the start of your slot. The upload time counts.

## Submission checklist

- [ ] Notebook or `.py` script, runnable from a fresh environment built from `requirements.txt`, with the data in `data/`
- [ ] Written answers: data quality summary (A1), forecast comparison (C1), interpretation of D1 and D2
- [ ] `answers.sql` with B1 and one-line assumptions
- [ ] `requirements.txt` updated if you added packages
- [ ] In `ANSWERS.md` or the notebook, a few lines: your Python version, what you did not have time to finish and how you would have done it, how you used AI coding assistants (if at all), and the time actually spent
- [ ] `firstname-lastname.zip` uploaded to the shared drive within 75 minutes
