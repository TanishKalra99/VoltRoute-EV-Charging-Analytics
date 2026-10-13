# Methodology and analytical workflow

## Scope

This synthetic case study models 1,000 EV charging-session source rows for 36 stations across eight cities in January–December 2025. Its central output is a validated operational analysis dataset, with investigation rows retained separately instead of silently discarded.

## 1. Profile inputs

- Inspect identifier uniqueness and duplicates using `Session_ID`.
- Inspect missing or nonnumeric `Energy_kWh`.
- Validate station keys against the station master.
- Validate datetime types and calculate session duration.
- Check status spellings and known categories.
- Preserve an untouched raw-data worksheet for auditability.

The synthetic starter source has **16 missing energy values, 18 rows associated with duplicated session IDs, 7 negative-duration rows, and 8 invalid-station rows**. These represent different failure modes and can overlap.

## 2. Transform and validate using Power Query

1. Convert start and end timestamps to the correct date/time types.
2. Standardise session-status text to `Completed`, `Failed`, `Interrupted` (the raw source also contained a nonstandard `complete` label).
3. Derive `Duration_Minutes` from `End_Time - Start_Time`.
4. Derive `Year_Month` from the start timestamp using `yyyy-MM`.
5. Calculate `Duplicate_Status` based on repeated `Session_ID` values; investigate duplicate groups before excluding them.
6. Compare `Station_ID` against the master to derive `Station_Validation`.
7. Derive `Energy_Quality_Flag`: completed sessions with missing energy require review; missing energy for failed/interrupted sessions is retained under the project's documented rule.
8. Combine applicable quality checks into `Data_Quality_Status`.
9. Branch the queries by final status: **954 `Pass` rows** versus **46 `Review_Required` rows**.
10. Load these datasets into separate Excel Tables. Map the clean rows to station cities using a station-key lookup.

### Exception policy

A row is sent to review when it has **any** of the following observed conditions:

- repeated session identifier (`Review_Duplicate`);
- completed session lacking recorded energy (`Missing_Completed`);
- station reference missing from station master (`Invalid`);
- negative duration (`Negative_Duration`).

**Overlapping issues:** among 46 review-required rows, the individual category counts are 18, 14, 8 and 7. Their sum is 47 because one row has both a missing completed-session energy value and a negative duration. The quality rules are a **portfolio business assumption**, not a universal EV charging industry standard.

### Sample M transformation

The Power Query logic can derive a month key with the following expression (replace the column identifier if needed):

```powerquery
Date.ToText(DateTime.Date([Start_Time]), "yyyy-MM")
```

And a duration flag can be expressed as:

```powerquery
if [Duration_Minutes] < 0 then "Negative_Duration" else "Valid"
```

These are representative transformation snippets; the final query steps should be inspected in the workbook before publishing them as exact source code.

## 3. Define measures before visualising

**Denominators are intentional.** Operational completion metrics use only the 954 quality-passing rows, not all 1,000 raw rows.

```excel
Valid Sessions:
=COUNTA(Clean_Sessions[Session_ID])

Completed Sessions:
=COUNTIF(Clean_Sessions[Session_Status],"Completed")

Completed Energy (kWh):
=SUMIFS(Clean_Sessions[Energy_kWh],Clean_Sessions[Session_Status],"Completed")

Average Completed Duration (minutes):
=AVERAGEIFS(Clean_Sessions[Duration_Minutes],Clean_Sessions[Session_Status],"Completed")

Completion Rate:
=Completed_Session_Count/Valid_Session_Count
```

Keep **completed-only energy** separate from **all-status recorded energy**:

- Completed-only: **24,516.51 kWh**.
- All passing statuses: **24,921.20 kWh**.
- Difference: **404.69 kWh**, attributable to energy recorded in non-completed statuses in the cleaned data.

## 4. Build the analytical layer

- Group clean sessions by `City` for counts, energy, and status mix.
- Group by `Year_Month` for session counts, recorded energy, and mean energy per session.
- Calculate MoM growth as `(current - previous) / previous`, leaving the first period undefined.
- Build a distinct exception summary using `COUNTIF` on the review-required Excel Table.
- Create PivotCharts for city and month comparisons. Drive the city-specific operational cards with `GETPIVOTDATA` and a City slicer.

### Why the KPI cards are dynamic

`GETPIVOTDATA` queries PivotTable measures. Because the City slicer is connected to those PivotTables, the displayed KPI changes with the selection. Conversely, the global exception count/rate deliberately stay fixed, representing the **whole source population**, not the selected city.

## 5. Validate the result

The baseline reconciliation is:

```text
1,000 raw records
  ├── 954 pass             (95.40%)
  └──  46 review required  ( 4.60%)
```

Status check: `773 Completed + 117 Failed + 64 Interrupted = 954`.

See [Validation checklist](VALIDATION_CHECKLIST.md) for the complete release gate.

## Known modelling limitations

- A duplicate flag marks all rows associated with duplicated IDs; it does not choose a canonical winner.
- Retained missing-energy values on non-completed sessions need an explicit missing-value policy for any subsequent average or revenue interpretation.
- The City field is looked up via station master; master integrity and consistent key formats matter.
- The dataset does not expose charger outage logs, confirmed failure causes, or externally measured station uptime.
- Customer ratings and tickets are available in the synthetic starter, but not used in the delivered operational dashboard.
- Cached Excel PivotTable values do not prove that a brand-new data refresh will succeed on a different machine; validate query sources when publishing.
