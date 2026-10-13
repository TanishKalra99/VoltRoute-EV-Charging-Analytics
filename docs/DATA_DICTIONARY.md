# Data dictionary

This reference describes the fields observed in the audited VoltRoute workbook. Names and table identifiers should be reconfirmed if the workbook is streamlined before release.

## Datasets and grain

| Dataset | Grain | Rows in audited draft | Role |
|---|---|---:|---|
| `Raw_Sessions` | One source record per session event row; IDs may repeat | 1,000 | Immutable starting point for session cleaning |
| `Clean_Sessions_Data` (`Clean_Sessions` table) | One quality-passing session row | 954 | Operational metrics and city/month analytics |
| `Data_Quality_Exceptions` (`Data_Quality_Status` table in draft) | One source row flagged for review | 46 | Exception investigation |
| `Station_Master` / station reference table | One station record | 36 | Station-to-city mapping, station validation |
| `Raw_Tickets` | One service-ticket row | 150 | Synthetic starter data, **not yet analysed** |

> **Naming detail:** Excel worksheet names, Excel **Table names**, and Power Query **query names** are not interchangeable. The exception worksheet is named `Data_Quality_Exceptions`, while its underlying Excel Table in the audited draft is named `Data_Quality_Status`.

## Session fields

| Field | Meaning / type | Notes |
|---|---|---|
| `Session_ID` | Text session identifier | Duplicate IDs investigated; unique among passing records |
| `Station_ID` | Text foreign key to station master | Validate membership in master |
| `Customer_ID` | Synthetic customer identifier | Avoid interpreting as personally identifiable real-world data |
| `Start_Time` | Date/time | Session start; month derived from this field |
| `End_Time` | Date/time | Session end |
| `Energy_kWh` | Decimal kWh | Missing completed-session energy requires review; noncompleted blanks may be retained |
| `Session_Status` | Categorical text | Standardised: Completed, Failed, Interrupted |
| `Payment_Method` | Categorical text | Payment-channel attribute, not used in final KPIs |
| `Discount_Pct` | Decimal fraction | Potential input for future revenue model |
| `Rated_Experience_1to5` | Numeric survey rating | 150 missing among the 954 passing rows |
| `App_Version` | Text | Application version; not used in final KPIs |

## Derived and quality fields

| Field | Meaning / type | Observed / intended values |
|---|---|---|
| `Energy_Quality_Flag` | Text quality category | `Valid`, `Missing_Completed`, `Missing_Failed`, `Missing_Interrupted` |
| `Session_Count` | Intermediate duplicate-check column | Present but blank in the final `Clean_Sessions` output; consider removing from output once dependencies are checked |
| `Duplicate_Status` | Text validation flag | `Unique`, `Review_Duplicate` |
| `Station_ID.1` | Duplicate station identifier from merge | Intermediate; consider removing after validation |
| `Station_Validation` | Text quality flag | `Valid`, `Invalid` |
| `Duration_Minutes` | Numeric minutes | Derived from `End_Time - Start_Time`; negative values flagged |
| `Duration_Quality_Flag` | Text quality flag | `Valid`, `Negative_Duration` |
| `Data_Quality_Status` | Overall status | `Pass`, `Review_Required` |
| `Year_Month` | Text key (`yyyy-MM`) | Calendar month from session start |
| `City` | Text geographic label | Attached to cleaned sessions via station lookup; eight observed cities |

## Station master fields

| Field | Description |
|---|---|
| `Station_ID` | Primary station key used to join to sessions |
| `Station_Name` | Human-readable station name |
| `City` | Station city |
| `Region` | Geographic region |
| `Site_Type` | Deployment context |
| `Connectors` | Number of connectors |
| `Charger_Type` | Charger category |
| `Price_per_kWh_INR` | Listed energy price; not used in current revenue calculation |
| `Operator` | Operator name |
| `Launch_Date` | Station opening date |

## Measures and denominators

| Measure | Definition | Population |
|---|---|---|
| Valid sessions | `COUNT(Clean_Sessions[Session_ID])` or `COUNTA` | 954 passing rows |
| Completed sessions | `COUNTIF(status,"Completed")` | Passing rows only |
| Completion rate | Completed / passing rows | 773 / 954 = 81.03% |
| Completed energy | `SUMIFS(Energy_kWh,status,"Completed")` | Passing completed rows only |
| All-status energy | `SUM(Energy_kWh)` | All passing rows with numeric energy |
| Avg duration, completed | `AVERAGEIFS(Duration_Minutes,status,"Completed")` | Passing completed rows |
| Avg energy, all-status | Energy sum / passing rows | Valid monthly sessions including statuses with zero/blank energy |
| Exception rate | Review-required rows / original rows | 46 / 1,000 = 4.60% |
| City completion rate | Completed city rows / all passing city rows | Slicer-filtered city totals |
| MoM session growth | (Current month count / previous month count) - 1 | Consecutive months, same filtering context |

**Caution:** Issue-flag counts are non-mutually-exclusive. Their sum must **not** replace the 46 distinct review-required rows.
