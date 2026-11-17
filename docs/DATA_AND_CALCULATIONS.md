# Data and calculations

## Row meaning

Each row is one synthetic hourly reading. Hour 0–23 is Day 1 and Hour 24–47 is Day 2. This dataset is a replay demonstration rather than a continuous live system.

| Field | Meaning |
|---|---|
| Hour | Elapsed hourly index across the two-day replay |
| BedsOccupied | Occupied bed count at the reading |
| TotalBeds | Available capacity; 100 in this exercise |
| RollingWaitMin | Rolling waiting-time reading in minutes |
| AcuityLevel1Count | Count of the highest-acuity group in the exercise |
| StadiumEventFlag | 1 when the illustrative event is active; otherwise 0 |
| TrafficIncidents | Illustrative traffic-incident count |

## Rules

- Occupancy rate = BedsOccupied / TotalBeds.
- Naive alert = 1 if occupancy is at least 0.85.
- Surge score = StadiumEventFlag + INT(TrafficIncidents >= 2) + INT(AcuityLevel1Count >= 3).
- Improved alert = 1 if surge score is at least 2.
- Wait severity: Normal < 45; Warning >= 45 and < 75; Critical >= 75 minutes.
- Wait band = full-demo mean plus or minus twice the sample standard deviation. These are illustrative reference limits, not a clinical operating rule or a forecast interval.

## Derived data

Day 1 is based on the original 24-hour source. Waiting times at Hours 16 and 17 are set to 38 and 39 minutes. Day 2 is generated from each revised Day 1 row: occupied beds = round(0.70 × occupied beds + 10); wait = round(0.44 × wait + 10); acuity = min(2, Day 1 acuity); event = 0; traffic = min(1, Day 1 traffic); Hour is offset by 24.

The fixed snapshot compares the score at Hour 17 (3/3) with Hour 16 (2/3). It is intentionally a single saved replay state. Inspect the full trend views and CSV for the remaining hours.

## Validation scope

The archive preserves the previously prepared Tableau package and reports. Repository checks verify ZIP integrity, row count, relative workbook data links and the supplied alert calculations. They do not add new clinical validation or claim that every interactive click path has been tested again.
