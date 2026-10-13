# Release readiness — action required

**These documentation files are prepared ahead of the final workbook cleanup.** They are not evidence that the uploaded Excel workbook was changed or republished.

## Before your initial GitHub commit

- Fix `KPI_Analysis!B7`: use duration in minutes, not energy in kWh. Target: **85.15 minutes**.
- Show `4.60%` rather than `0.046` for the exception-rate dashboard card.
- Correct `XLOOKUP` to use one station master table for both the lookup and return arrays.
- Ensure the City Energy chart doesn't mix session count and kWh on one axis.
- Move the Monthly Sessions chart onto the Dashboard, if desired, and confirm slicer interactivity.
- Clean redundant sheet copies only after verifying Power Query dependencies.
- Add **final corrected** `VoltRoute_EV_Charging_Analytics.xlsx` to repository root.
- Add the three dashboard screenshots in `assets/screenshots/`.
- Complete `docs/VALIDATION_CHECKLIST.md`.

The metrics cited in the documentation were independently checked against the source and cached data in the uploaded draft workbook, including the corrected average duration computed from its clean records.

## Scope reminders

- **Synthetic data and fictional company.**
- **Ticket/SLA analysis not included**; `Raw_Tickets` remains an unused starter source.
- **No revenue model is yet implemented**; do not claim net revenue or realised ROI.
- **A source-data licence has not been established**; check rights before publishing the underlying template data.
