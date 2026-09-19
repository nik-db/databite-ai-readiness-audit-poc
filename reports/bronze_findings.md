# Bronze Layer Findings — insurance_claims.csv
- 1000 rows, 1000 unique policy_number (no duplicate records)
- Phantom column `_c39` (100% null) — trailing comma artifact in source CSV, dropped
- Sentinel "?" values found in: [list columns from the check above] — NOT captured by standard null checks
- policy_bind_date, incident_date correctly typed as datetime
- All other columns typed as expected (int64/float64/object)

# Bronze Layer Findings — insurance_claims.csv
**Landed:** [today's date] | **Source:** insurance_claims.csv (Mendeley/Kaggle public dataset) | **Rows:** 1,000

## Structural Issues
- Phantom column `_c39` (100% null) — trailing comma artifact in source CSV. Dropped post-landing.
- No accompanying data dictionary or schema documentation from source.

## Data Quality — Sentinel Values (not caught by standard null checks)
| Column | '?' rows | % missing |
|---|---|---|
| property_damage | 360 | 36.0% |
| police_report_available | 343 | 34.3% |
| collision_type | 178 | 17.8% |

Standard `isna().sum()` reported **zero nulls** across all three columns — the missingness is entirely masked by the use of a string placeholder instead of a true null. Any downstream pipeline (BI, ML, or RAG) trusting the null count alone would silently treat "?" as a valid category.

## Data Quality — Outliers
- `umbrella_limit`: 1 row with a negative value — flagged for row-level review before deciding whether it's a data-entry error or a known sentinel pattern.

## Uniqueness & Completeness
- 1,000 rows, 1,000 unique `policy_number` — no duplicate policy records.
- All other columns: fully populated per pandas null-check (though see sentinel-value caveat above — a full audit assumes nothing based on `isna()` alone going forward).

## Typing
- `policy_bind_date`, `incident_date` correctly inferred as datetime.
- All other types as expected on auto-inference.

## Data Quality — Outlier Investigation
- `umbrella_limit = -1000000` occurs on exactly 1 of 1,000 rows (policy_number 526039).
  Not a repeated sentinel pattern — likely an isolated data-entry error, since negative
  liability limits are not valid in practice. Distinct from the '?' sentinel pattern:
  this is a single implausible value, not a systemic missing-value encoding.