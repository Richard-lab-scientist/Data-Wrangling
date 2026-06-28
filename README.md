## Data Reconciliation & Modelling

### Project Overview

Three records containing the same client order details from a database each contains: client order table, fulfillment table and warehouse table. Presented in inconsistent formats and conflicting records. This Project aims to reconcile the data by: Cleaning, joining and flagging discrepancies.

<img width="1412" height="541" alt="Screenshot 2026-06-28 182542" src="https://github.com/user-attachments/assets/5f663701-54e2-4b31-9ce3-393f4663f8c4" />

### Data Sources 
The data sources contains the:
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

- data cleaning (`TRIM`, `UPPER`, standardising inconsistent keys)
- Cross-sheet lookups (`INDEX/MATCH` with `IFERROR` handling)
- Automated discrepancy detection (`IF`, `COUNTIF`, `COUNTIFS`)
- Conditional formatting to surface issues visually

### Issues Found
- Quantity Mismatches vs Client Order: 5 orders are inconsistent with the agency report and 4 client order disagrees with warehouse log
- Missing Records: ORD-1009 and ORD-1017 do not appear in agency report, despite being dispatched by the warehouse. ORD-1019 is dispatched per the agency but missing from the warehouse log.
- Data quality: ORD-1004 is a duplicate record in the agency report - flagged to avoid double counting the same order.

### Recommendation
-  Agency - quantity confirmation: ORD-1001, ORD-1006, ORD-1013, ORD-1020, ORD-1024 (5 orders where fulfilled qty does not match the order).						
-  Agency - missing records: ORD-1009, ORD-1017 do not appear in the agency report; ask the agency to confirm fulfilment status.						
-  Agency - data quality: flag the ORD-1004 duplicate row so it is not double-counted in any downstream agency reporting.						
-  Warehouse - quantity confirmation: ORD-1002, ORD-1008, ORD-1016, ORD-1023 (4 orders where dispatched qty does not match the order).						
-  Warehouse - missing record: ORD-1019 is confirmed by the agency but absent from the dispatch log; confirm whether it was shipped.						
-  Hold final client sign-off on this batch until the above 12 orders are confirmed.						 
