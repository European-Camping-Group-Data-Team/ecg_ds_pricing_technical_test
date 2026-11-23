# Dataset – fictional European campsite group

Synthetic data, seasons 2024 and 2025 (campsites open 1 April – 31 October).
Any resemblance to real campsites is coincidental. Column descriptions are in the test brief.

| File | Grain | Rows (approx.) |
|---|---|---|
| sites.csv | one row per campsite | 12 |
| inventory.csv | one row per campsite × accommodation type | 48 |
| bookings.csv | one row per booking (raw extract, not cleaned) | 67,000 |
| daily_demand.csv | one row per campsite × accommodation type × night open | 20,500 |
| price_tests.csv | one row per campsite × accommodation type × week in the 2025 price tests | 260 |

Tip: DuckDB can query these files directly, e.g. `SELECT * FROM 'bookings.csv' LIMIT 10;`
