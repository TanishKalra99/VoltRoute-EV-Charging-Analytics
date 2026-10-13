# Business insights and recommendations

**Context:** Synthetic EV charging network, 2025. Observational/descriptive findings only.

## Executive summary

The network's 954 quality-passing session records include 773 completed sessions, producing an **81.03% completion rate**. Completion rates vary across cities, while month-to-month energy demand does not perfectly track session volume. Source-data integrity also needs attention: 46 of the 1,000 original rows (4.60%) require review.

## City-level observations

| City | Clean sessions | Completed | Completion rate | Recorded energy, all passing statuses (kWh) |
|---|---:|---:|---:|---:|
| Ahmedabad | 108 | 86 | 79.63% | 2,632.31 |
| Bengaluru | 137 | 115 | 83.94% | 3,685.92 |
| Chennai | 102 | 90 | **88.24%** | 2,899.05 |
| Gurugram | 131 | 112 | 85.50% | **3,848.79** |
| Hyderabad | 113 | 85 | **75.22%** | 2,578.77 |
| Jaipur | 103 | 85 | 82.52% | 2,634.59 |
| Mumbai | 123 | 93 | 75.61% | 3,145.65 |
| Pune | 137 | 107 | 78.10% | 3,496.12 |
| **Total** | **954** | **773** | **81.03%** | **24,921.20** |

### Interpretation

- **Gurugram** is highest on recorded city energy; this is **all-status energy** and should not be mistaken for completed-only energy.
- **Chennai** leads completion rate in the observed sample.
- **Hyderabad** is lowest on completion rate, and its failed-session share is **17.70%**. **Mumbai** also has a low completion rate (75.61%).
- Different city sample sizes and synthetic sampling variability mean this is a **prioritisation signal**, not evidence of causation or statistical significance.

### Recommended operational follow-up

1. Investigate Hyderabad and Mumbai session-failure records by station, app version, payment type, and time of day.
2. Compare specific stations within each city rather than concluding the city itself causes the issue.
3. Validate the pattern with a longer and/or real operational time series before prioritising capital expenditure.

## Monthly findings

| Month | Clean sessions | Recorded energy (kWh) | Average energy per valid session (kWh) |
|---|---:|---:|---:|
| 2025-01 | 70 | 1,658.27 | 23.69 |
| 2025-02 | 70 | 1,941.15 | 27.73 |
| 2025-03 | 81 | 2,192.21 | 27.06 |
| 2025-04 | 82 | 2,152.32 | 26.25 |
| 2025-05 | 76 | 1,772.52 | 23.32 |
| 2025-06 | 73 | 1,921.17 | 26.32 |
| 2025-07 | 83 | 2,101.47 | 25.32 |
| 2025-08 | 78 | 2,324.46 | **29.80** |
| 2025-09 | 82 | 2,077.01 | 25.33 |
| 2025-10 | **92** | 2,312.51 | 25.14 |
| 2025-11 | 85 | **2,344.22** | 27.58 |
| 2025-12 | 82 | 2,123.89 | 25.90 |

**Interpretation:** October has the most sessions but November has the most recorded energy. August has the highest average energy per session. Higher activity does not always imply larger energy volume.

**Next question:** Investigate charger mix, station distribution, or typical session size before treating the differences as demand changes.

## Data-quality findings

| Issue type | Flagged rows |
|---|---:|
| Duplicate session IDs | 18 |
| Missing energy for completed sessions | 14 |
| Invalid station references | 8 |
| Negative charging durations | 7 |
| **Distinct exception rows** | **46** |

The four issue counts add to **47 occurrences**, but only **46 distinct rows** are flagged, because a record may fail multiple checks.

**Recommended control improvement:** Log and review each failed rule independently, maintain exception owner/status/reason, and report weekly trends. After remediation, rerun the same validation rules rather than manually overwriting KPI values.

## Caveats for interview or public presentation

- Don't say the dashboard **reduced** failures, costs, or defects: the synthetic exercise did not measure intervention outcomes.
- Don't describe correlations as confirmed operational causes.
- Distinguish completed-only energy (`24,516.51 kWh`) from all-status recorded energy (`24,921.20 kWh`).
- Don't add issue counts to calculate unique exceptions.
- Never claim revenue or SLA results without a corresponding implemented calculation.
