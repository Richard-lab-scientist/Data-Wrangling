## Data Reconciliation & Modelling

### Project Overview

Three records containing the same client order details from a database each contains: client order table, fulfillment table and warehouse table. Presented in inconsistent formats and conflicting records. This Project aims to reconcile the data by: Cleaning, joining and flagging discrepancies.

### Data Sources 
The data sources were contains the:
- Client_Order.csv
- Agency_fulfillment_report.csv
-  Warehouse_dispatch_log.csv

### Tools
- Excel - Data Cleaning
- Formula's

### Data Cleaning/Preparation
The initial data preparation phase, the following tasks were pereformed:
1. Data Loading and Inspection.
2. Handling values and inconsistent formats.
3. Data Cleaning and Formatting.

### Data Reconciliation and Modelling
1. Consolidating data in a workbook.
2. Comparisons of records and quantities.
3. Flag Mismatch automatically.
4. Creating a Unified Model, highighting errors and mismatches.

## Skills demonstrated

- Multi-source data cleaning (`TRIM`, `UPPER`, standardising inconsistent keys)
- Cross-sheet lookups (`INDEX/MATCH` with `IFERROR` handling)
- Automated discrepancy detection (`IF`, `COUNTIF`, `COUNTIFS`)
- Conditional formatting to surface issues visually

### Issues Found
- Quantity Mismatches vs Client Order: 5 orders are inconsistent with the agency report and 4 client order disagrees with warehouse log
- Missing Records: ORD-1009 and ORD-1017 do not appear in agency report, despite being dispatched by the warehouse. ORD-1019 is dispatched per the agency but missing from the warehouse log.
- Data quality: ORD-1004 is a duplicate record in the agency report - 

None of the three match. IDs arrive in inconsistent casing (`ORD-1002` vs `ord-1002`) and with stray whitespace, column names differ between sources, one row is duplicated, two orders are missing from the agency report, and one is missing from the warehouse log. Quantities disagree on 9 of the 25 orders.

This is deliberate — it mirrors what actually lands in an inbox when reconciling third-party data, rather than a clean tutorial dataset.

## What the model does

Built in `Data_Reconciliation_Model.xlsx`:

- **Raw tabs** — each source kept exactly as received (messy, unedited), plus a `Clean Key` helper column (`=TRIM(UPPER(...))`) that standardises IDs for joining.
- **Reconciliation Model** — the master tab. Uses `INDEX/MATCH` to pull the agency and warehouse figures onto the client order list via the cleaned key, then flags every order with `IF` logic:
  - `Quantity Mismatch` if quantities disagree
  - `Missing in Agency Report` / `Missing in Warehouse Log` if an order is absent from a source
  - A duplicate-row flag carried through from the raw agency data
  - An overall `NEEDS REVIEW` / `OK` flag per order, with conditional formatting (red/green)
- **Discrepancy Summary** — a rollup by discrepancy type and by client, with a chart, built entirely from formulas referencing the model (no hardcoded numbers).
- **Findings Memo** — a short written report in the format you'd actually send to a senior stakeholder: what was found, and what to do about it.

### Result
Of 25 orders, **12 (48%) are flagged for review**, across 13 individual discrepancies (5 agency quantity mismatches, 4 warehouse quantity mismatches, 2 missing-in-agency, 1 missing-in-warehouse, 1 duplicate row).



## Repo structure
```
├── Data_Reconciliation_Model.xlsx   # the full model — open this first
├── data/
│   ├── client_orders.csv
│   ├── agency_fulfillment_report.csv
│   └── warehouse_dispatch_log.csv
└── README.md
```

## How to use it
1. Download `Data_Reconciliation_Model.xlsx` and open in Excel (or Google Sheets — re-check formulas import correctly, see note below).
2. Start on the **Start Here** tab for a guided tour of the workbook.
3. Edit any value in a `Raw -` tab and watch the Reconciliation Model, Discrepancy Summary, and chart update automatically.

> Note: this was built and verified in Excel-compatible formula syntax (`INDEX/MATCH`, not `XLOOKUP`) specifically so it opens correctly in any Excel version and in Google Sheets.

---
*Built as a portfolio project to demonstrate Excel-based data reconciliation and discrepancy-checking skills.*
