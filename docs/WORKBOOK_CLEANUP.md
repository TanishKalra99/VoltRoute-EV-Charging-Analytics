# Workbook cleanup plan — preserve dependencies

This is a **guide**, not a record of changes made to the user's uploaded file. Take a backup before deleting any worksheet or Power Query query.

## Safe candidates after basic review

| Item | Suggested action | Impact |
|---|---|---|
| `Dashboard_Template` | Remove after confirming no references | Unused draft layout |
| `Monthly_Analysis!J4:K4` | Clear after testing | Temporary Bengaluru `SUMIFS` practice |
| `Skill_Roadmap` | Hide/remove | Educational scaffolding, not core analytics |
| `START_HERE` | Keep or hide | Useful scope/provenance/disclosure |
| `Raw_Tickets` | Keep separately for future SLA phase or remove only if no query needs it | Out-of-scope ticket data |
| Four duplicate QA PivotTables on `DQ_Analysis` | Optional removal | Summary formulas + chart can stand alone |

## High-risk candidates: never delete without checking

**`Raw_Sessions (2)`**

The uploaded draft contains extra formatting extending to Excel's maximum worksheet row. This inflates file size substantially. Some `Data_Quality_Log` formulas still refer to that duplicate copy, and a Power Query source may depend on its Excel Table. Migrate formulas and inspect **Queries & Connections** first.

**`Station_Master (2)`**

The draft contains two station-master worksheets. The clean table's `City` lookup used the station-ID column from one named Excel Table and the city column from another. Replace with **one consistent lookup table** before consolidating or deleting the second reference sheet:

```excel
=XLOOKUP([@Station_ID],StationMaster[Station_ID],StationMaster[City],"Not Found")
```

**`Clean_Sessions_Data`**

This is the main dataset for five operational PivotTables. Do not delete, rename its table casually, or replace its 954 rows with an unfiltered query result.

**`Data_Quality_Exceptions`**

This is the 46-row investigation output. Keep it and preserve the underlying Excel Table name referenced in `DQ_Analysis` formula cells (`Data_Quality_Status` in the audited draft).

## Optional output-column cleanup

- Remove the empty `Session_Count` column from final **output** if it is no longer referenced.
- Remove merged duplicate station field `Station_ID.1` from final **output** if no step requires it.
- Retain original source identifiers and quality flags in a reproducible upstream query.

## Suggested final visible sheets

`Dashboard`, `KPI_Analysis`, `Pivot_Analysis`, `Monthly_Analysis`, `DQ_Analysis`, `Clean_Sessions_Data`, `Data_Quality_Exceptions`, `Raw_Sessions`, `Station_Master`, `Data_Quality_Log`.

You may hide helper sheets, but retaining them is helpful when reviewers want to inspect your calculations. Keep Power Query connection-only helper queries where they support refresh; hidden does not mean deleted.

## Release testing after cleanup

1. **Data → Refresh All**.
2. Recheck 1,000 / 954 / 46 reconciliation.
3. Recheck formula-driven and slicer-driven KPIs.
4. Select two cities and confirm all linked visuals respond.
5. Save, close, reopen, and check once more.
