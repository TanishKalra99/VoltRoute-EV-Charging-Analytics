# Interview walkthrough (2–3 minutes)

## 60-second project introduction

> VoltRoute is a synthetic EV charging analytics case study I developed in Excel and Power Query. I worked with 1,000 session records and a 36-station reference table. I profiled the data for duplicates, missing completed-session energy, invalid station IDs and negative durations, then separated 46 review-required rows from 954 validated records. I used Excel formulas and PivotTables to calculate completion rates, energy delivered and monthly trends, and connected the dashboard KPIs to a City slicer using GETPIVOTDATA. The completed-session rate was 81.03%. The dashboard also shows that city reliability and session volumes vary, so I recommended targeted investigation rather than drawing unsupported causal conclusions.

## Recommended screen-sharing order

1. **Dashboard (20–30 sec)** — four operational KPI cards, City slicer, status mix, monthly charts and Data Quality section.
2. **City slicer (20 sec)** — select Bengaluru or Gurugram; show the linked KPI changes and clear the filter.
3. **Raw and clean data (30 sec)** — show original 1,000 rows, 954 passing rows, and 46 exceptions.
4. **Power Query (30 sec)** — show the key cleaning steps and quality status; explain why anomalies are isolated rather than deleted.
5. **Pivot_Analysis and Monthly_Analysis (30 sec)** — show city/month aggregations and `GETPIVOTDATA`, `SUMIFS`, `COUNTIF`, and MoM.
6. **Insight (20 sec)** — contrast Chennai's completion rate with Hyderabad's and October session peaks with November energy peaks.

## Likely questions and concise answers

**Q: Why use Power Query instead of cleaning directly in worksheets?**  
A: It records repeatable transformation steps, supports refresh, and separates source data from validated analytical outputs.

**Q: Why do the four issue counts add to 47 if only 46 records were excluded?**  
A: One record has two failing checks. I count unique review rows for the overall exception rate and track each failing rule separately.

**Q: How does your City slicer update the KPI cards?**  
A: The City slicer filters multiple PivotTables, and `GETPIVOTDATA` retrieves their current totals into the KPI cells. Static worksheet `COUNTIF` formulas would not respond to slicer selections by themselves.

**Q: Why is the energy KPI 24,516.51 kWh while the monthly total is 24,921.20 kWh?**  
A: The KPI counts energy from completed sessions only; the monthly chart totals recorded energy across all quality-passing statuses. Interrupted sessions contribute the difference.

**Q: What does the 81.03% completion rate tell us?**  
A: It is the share of quality-passing session records marked Completed. It is not hardware uptime or a verified network availability KPI.

**Q: What is one actionable finding?**  
A: Hyderabad's observed completion rate is relatively low, so I would drill into stations, app versions, payment methods, and time windows before suggesting interventions.

**Q: How did you verify the numbers?**  
A: I reconciled raw, passing and review-required rows; checked status totals against the 954-row clean table; tested the slicer across cities; and cross-checked the completed-energy total against a `SUMIFS` calculation.

**Q: What would you do next?**  
A: Integrate the synthetic ticket dataset and analyse resolution SLA, add station-level reliability views, and build automated QA tests. I'd also document any revenue assumptions before constructing a revenue metric.

## Demonstrate formulas without memorising long syntax

- `COUNTIF`: Count sessions in a given status or quality category.
- `SUMIFS`: Sum energy for a given status, city, and month.
- `AVERAGEIFS`: Mean charging duration for completed sessions.
- `XLOOKUP`: Attach City to Session via Station ID.
- `GETPIVOTDATA`: Pull slicer-filtered PivotTable measures into KPI cards.
- `SUMIF`: Match monthly energy and session-count summaries.

## What not to claim

- Do not say the dashboard **reduced defects or improved reliability**; no intervention outcomes were evaluated.
- Do not claim that Hyderabad's rate establishes a technical cause.
- Do not claim revenue or service-ticket/SLA analysis was completed when it was not.
- Make clear this is a **synthetic portfolio project**, not live operational reporting for an actual EV operator.
