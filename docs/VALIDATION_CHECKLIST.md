# Pre-publish validation checklist

Run this after applying the cleanup recommendations and **before** uploading the final Excel workbook to GitHub. Check each item in **desktop Microsoft Excel** and save the workbook afterward.

## Data integrity and formulas

- [ ] `Raw_Sessions` has exactly **1,000** records (exclude the header).
- [ ] `Clean_Sessions_Data` has **954** records and every row's final quality status is `Pass`.
- [ ] `Data_Quality_Exceptions` has **46** records and every row is `Review_Required`.
- [ ] `954 + 46 = 1,000` after **Refresh All**.
- [ ] Completed = **773**, Failed = **117**, Interrupted = **64**; totals = **954**.
- [ ] Completed energy = **24,516.51 kWh**; all-status clean energy = **24,921.20 kWh**.
- [ ] **Fix `KPI_Analysis!B7`** to average `Duration_Minutes` for completed sessions: expected **85.15 minutes**.
- [ ] Dashboard exception-rate card is **4.60%**, formatted as a percentage.
- [ ] City lookup references the **same** station master table for IDs and city: `StationMaster[Station_ID]` and `StationMaster[City]`.
- [ ] No `#REF!`, `#N/A`, `#VALUE!`, `#DIV/0!`, `#NAME?`, or unresolved lookup values in released tables.

## Dashboard interaction tests

- [ ] Clear all City slicer filters → 954 clean sessions, 773 completed, 81.03% completion, 24,516.51 kWh completed energy.
- [ ] Select **Bengaluru** → 137 clean sessions and 115 completed; city-based PivotCharts respond.
- [ ] Select **Gurugram** → 131 clean sessions and 112 completed; completion rate **85.50%**.
- [ ] Select **Chennai** → completion rate **88.24%**.
- [ ] Select a city and verify **both monthly trend charts** update, not just their source worksheet numbers.
- [ ] Clear the City slicer again; all operational KPIs return to all-city values.
- [ ] Exceptions card remains 46 and exception rate remains 4.60% regardless of City selection (global denominator).
- [ ] Chart titles and units match the actual plotted measure; **city energy chart should not mix session counts and kWh on one axis**.
- [ ] The **Monthly Charging Sessions** chart is on `Dashboard`, not stranded on a helper sheet.

## Data-quality audit

- [ ] Duplicate flag count **18** rows.
- [ ] Missing energy for Completed count **14** rows.
- [ ] Invalid station references count **8** rows.
- [ ] Negative durations count **7** rows.
- [ ] Distinct review-required count **46**, not sum of flags (47).
- [ ] `Year_Month` contains 12 periods from 2025-01 to 2025-12.
- [ ] All clean session `City` values resolve; no `Not Found`.
- [ ] Raw synthetic data and other refresh inputs are preserved if Power Query needs them.

## Workbook safety and publishing

- [ ] Remove only confirmed-unused copies (`Dashboard_Template`, redundant raw/station sheets **after** checking query/table/formula references).
- [ ] Review the oversized `Raw_Sessions (2)` worksheet for accidental formatting to Excel's last row; consider recreating/removing it only once dependencies are migrated.
- [ ] Confirm data-source dependencies in **Queries & Connections**; no private absolute file paths, credentials or nonportable sources.
- [ ] Verify all PivotTables are refreshed; save with City slicer cleared.
- [ ] Reopen the saved `.xlsx` and confirm the dashboard renders in **desktop Excel**.
- [ ] Remove personal data or unwanted workbook metadata if present; synthetic session/customer IDs are not real identities.
- [ ] Capture the three screenshots listed in `assets/screenshots/README.md`.
- [ ] Confirm you have permission to publish the synthetic starter data/workbook or publish transformed results/screenshots only.
- [ ] Add the final `.xlsx` to the repo root under the expected filename.

**Release gate:** Do not mark the project final until all applicable items have been checked. The README assumes the duration formula and chart cleanup have been completed.
