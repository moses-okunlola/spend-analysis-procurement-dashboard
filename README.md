# Spend Analysis & Procurement Performance Dashboard

A Power BI dashboard analysing FY2023 procurement spend for a manufacturing purchasing organisation — 100 purchase orders across 16 suppliers and three article segments (Row Material, Component, Packaging) — built to surface budget variance, supplier risk, and spend concentration for procurement leadership.

## Business Question

Where is the €4.6M procurement budget actually going, which suppliers carry the most spend and risk, and which article segments are driving cost overruns versus savings?

## Dataset

Two linked tables, 100 rows each:

- **Order Table** — one row per transaction: Article ID, Purchase Family, Article Segment, Order Price, Supplier Name, Lead Time (weeks), Transaction ID, Transaction/Shipment Date, Transport Type, Invoice Price.
- **Budget Table** — one row per article: Article ID, Supplier Contract (Yes/No), Financial Health Check (Yes/No), Quality Certificate Check (Yes/No), Estimated Buying Price.

This is a synthetic dataset built to mirror a realistic industrial purchasing structure: article segments split by intended budget weight (Row Material heaviest, then Component, then Packaging), supplier pools segmented by product family, and estimated buying price set as a benchmark to measure actual order and invoice price against.

## Methodology

- **Data modelling:** Power Query used to clean and type both tables; relationships built on Article ID.
- **Budget variance:** measures compare Estimated Buying Price against realised Order Price and Invoice Price, both in aggregate and by Article Segment.
- **20/80 (Pareto) supplier concentration:** built natively in Power Query rather than DAX — duplicated the order table, grouped and summed spend by supplier, added an index column, self-joined the table against itself via the index to build a running cumulative total, then calculated cumulative share with a DAX measure (`% Cumulated Spend`). Conditional formatting flags suppliers inside the top 80% of cumulative spend in green, the long tail in orange.
- **Risk flags:** Supplier Contract, Financial Health Check, and Quality Certificate Check fields are surfaced as compliance/risk indicators at the article level.

## Key Findings (FY2023)

- **€4.51M** in realised order value and **€4.47M** invoiced, against a **€4.63M** estimated budget — a net **€121K (2.6%) favourable variance** overall.
- That favourable variance masks an underlying split: **Row Material ran €177K over** its estimate, while **Packaging (–€204K)** and **Component (–€95K)** came in under — the two smaller segments' savings absorbed the larger segment's overrun.
- Segment mix of realised order spend: Row Material ~49%, Component ~38%, Packaging ~13%.
- **19% of articles (19 of 100)** have no signed supplier contract in place — a straightforward compliance gap to flag to procurement leadership.
- **36% of articles** are sourced from suppliers without a current quality certificate on file.
- Transport spend splits close to evenly between **Express and Standard** shipment, with **26% of orders** missing a recorded transport type.
- Pareto analysis shows spend is concentrated in a small subset of the 16 active suppliers — a small handful account for the majority of cumulative FY2023 spend, visualised with conditional-formatted running-total bars.

## Tools

Power BI (Power Query, DAX) · Excel

## Files

- `Spend_Analysis_Procurement_Dashboard.pbix` — full Power BI file
- `Order_Table_2023.xlsx` / `Budget_Table_2023.xlsx` — source data
- `dashboard-overview.png` — dashboard screenshot

## Notes

Estimated Buying Price, Supplier Contract, Financial Health Check, and Quality Certificate Check are recorded at the **article** level, not the supplier level, since a single supplier can carry multiple articles under different contract/certification terms. Distinct supplier count in the Order table is 16.
