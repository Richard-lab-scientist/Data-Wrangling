# Third-Party Data Reconciliation Model

A self-contained Excel project that simulates a real data-analyst workflow: pulling together messy, conflicting data from multiple sources, cleaning it, flagging every discrepancy automatically, and reporting the findings to a stakeholder.

## The scenario

Three teams record the same batch of 25 client orders, independently:

| Source | Represents | Format |
|---|---|---|
| `data/client_orders.csv` | What the client ordered (source of truth) | Order ID, Client, Product, SKU, Qty Ordered |
| `data/agency_fulfillment_report.csv` | What a third-party agency says it fulfilled | Order Ref, Product, SKU Code, Qty Fulfilled |
| `data/warehouse_dispatch_log.csv` | What the warehouse actually dispatched | Order Number, Item Description, SKU, Qty Dispatched |

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

## Skills demonstrated

- Multi-source data cleaning (`TRIM`, `UPPER`, standardising inconsistent keys)
- Cross-sheet lookups (`INDEX/MATCH` with `IFERROR` handling)
- Automated discrepancy detection (`IF`, `COUNTIF`, `COUNTIFS`) instead of manual eyeballing
- Conditional formatting to surface issues visually
- Stakeholder-ready summary and written reporting, not just raw numbers

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
