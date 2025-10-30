# Tableau Dashboard — Summary & Publish Instructions

This document describes the Tableau dashboard created to visualize customer segments and LTV forecasts, and explains how to publish/embed it.

## Dashboard purpose
- Show RFM distribution and clusters.
- Surface high-value segments (top deciles) and their contribution to revenue.
- Display predicted LTV by customer and aggregated by segment.
- Provide filters for time ranges, geography, and product categories.

## Recommended sheets
1. RFM Score Grid — heatmap of Recency vs Frequency (color by average Monetary or LTV).
2. Segment Contribution — bar chart of segments (e.g., High, Mid, Low) vs revenue %.
3. LTV Forecast Trend — line chart of total predicted revenue over forecast horizon.
4. Top Customers Table — top N customers by predicted LTV, with recent orders summary.
5. Cohort/Retention — cohort retention curves by acquisition month and segment.

## Data prep tips
- Aggregate model outputs prior to publishing (pre-compute LTV predictions and join to customer master).
- Keep extracts small: publish only the fields needed for the dashboard.
- Use Tableau extracts (.hyper) for performance with large tables.

## Layout suggestions
- Top-left: filter panel (date range, region, segment).
- Center: RFM heatmap and segment contribution bar.
- Right: Top customers & LTV forecast summary.
- Bottom: cohort/retention visualization.

## Exporting and publishing
1. Save workbook locally as .twb (links to local data) or .twbx (packaged data).
2. To publish to Tableau Public:
   - Sign in to Tableau Public (create an account if necessary).
   - File → Save to Tableau Public As...
   - Choose a project name and description.
3. To publish to Tableau Server / Online:
   - File → Publish to Tableau Server...
   - Provide project, permissions, and data extract options (refresh schedule if needed).

## Embedding the dashboard
After publishing to Tableau Public, embed with an iframe:

```html
<iframe src="https://public.tableau.com/views/WORKBOOK_NAME/DASHBOARD_NAME?:showVizHome=no&:embed=true"
  width="1000" height="800"></iframe>
```

Replace WORKBOOK_NAME and DASHBOARD_NAME with values from the published URL. Set `?:showVizHome=no&:embed=true` to hide Tableau header and enable embedding.

## Screenshots and image recommendations
- Export high-resolution PNGs for documentation: Dashboard → Export Image.
- Use these images in README/docs for a quick preview.

## Automation & refresh
- If data is in BigQuery, use Tableau Bridge or Tableau Server scheduled extracts / refreshes.
- Alternatively schedule a job to write aggregated data to a hosted CSV or DB that Tableau reads.

---

If you want, I can:
- Create a packaged .twbx export (requires the workbook).
- Provide the exact embed HTML snippet after you publish to Tableau Public (I can fill in the published URL).
- Add sample screenshots (placeholders are included here; to add real screenshots I’ll need you to publish or provide exported images).