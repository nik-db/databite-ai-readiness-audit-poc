# Bronze Layer Findings — insurance_claims.csv
- 1000 rows, 1000 unique policy_number (no duplicate records)
- Phantom column `_c39` (100% null) — trailing comma artifact in source CSV, dropped
- Sentinel "?" values found in: [list columns from the check above] — NOT captured by standard null checks
- policy_bind_date, incident_date correctly typed as datetime
- All other columns typed as expected (int64/float64/object)