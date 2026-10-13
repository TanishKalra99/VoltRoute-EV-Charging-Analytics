# GitHub publishing guide

Suggested repository name: **`VoltRoute-EV-Charging-Analytics`**  
Suggested description: **`Excel + Power Query portfolio project: EV charging reliability, monthly trends, and data-quality dashboard with City slicer.`**

## 1. Finish and save the workbook

Follow [Validation checklist](VALIDATION_CHECKLIST.md). Rename the final workbook:

`VoltRoute_EV_Charging_Analytics.xlsx`

The documentation pack intentionally **does not** include the old draft workbook; add only the corrected version when ready.

## 2. Capture final screenshots

Export or crop clean captures from desktop Excel as described in [Screenshot guide](../assets/screenshots/README.md). Screenshots should show project features without Excel menus covering charts.

## 3. Upload to GitHub (web UI)

1. Open your GitHub profile and select **New repository**.
2. Name it `VoltRoute-EV-Charging-Analytics`; set visibility to **Public** if you want recruiters to see it.
3. Select **Create repository**; you can skip GitHub-generated README because this package already includes one.
4. Select **Add file → Upload files**.
5. Upload the **contents** of this documentation package plus the final workbook, preserving `docs/` and `assets/screenshots/` directories.
6. Commit changes using a message such as `Add validated VoltRoute analytics portfolio project`.
7. Open the repository root and check that GitHub automatically renders `README.md`.
8. Verify links, screenshots and the workbook are accessible.

**Note:** GitHub's online `.xlsx` preview cannot run Power Query or slicers. Readers should download and open the file in desktop Excel.

## 4. Alternative Git command line

```bash
git init
git add .
git commit -m "Add VoltRoute Excel analytics project"
git branch -M main
git remote add origin https://github.com/TanishKalra99/VoltRoute-EV-Charging-Analytics.git
git push -u origin main
```

Create the empty remote repository first. If you initialise the GitHub repository with a README or license, coordinate the initial commit to avoid a divergent history.

## 5. Suggested repository topics

`excel` · `power-query` · `data-analysis` · `pivot-tables` · `dashboard` · `data-quality` · `ev-charging` · `portfolio-project`

## 6. Final disclosures

- The case study is synthetic, not employer work or real customer data.
- Confirm the **starter workbook's redistribution permissions** before publishing raw synthetic source sheets. If uncertain, share the dashboard screenshots, the methods, and a workbook with permitted data only.
- The pack does **not** choose or add an open-source license. Add one only after checking source-data rights and selecting terms you want.
- Remove any personal credentials, local absolute file paths or unintended Excel metadata before publishing.
- No project achievement should claim measured improvement or business outcomes beyond the computed synthetic-data findings.

## 7. Optional pinned-repository description

**VoltRoute EV Charging Analytics — Excel / Power Query**  
Cleaned and validated 1,000 synthetic charging-session records, separated 46 review-required exceptions, and built an interactive City-filtered dashboard for charging completion, energy demand, monthly trends, and data quality.
