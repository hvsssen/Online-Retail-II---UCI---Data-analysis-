# Online Retail II — Data Analysis

Exploratory data analysis of the [Online Retail II](https://archive.ics.uci.edu/dataset/502/online+retail+ii) dataset: transaction-level sales data from a UK-based online gift/wholesale retailer, covering two full years (Dec 2009 – Dec 2011).

## Repository contents

| File | Description |
|---|---|
| `online_retail_II.xlsx` | Raw dataset — two sheets, `Year 2009-2010` and `Year 2010-2011`, one row per order line |
| `online_retail_eda.ipynb` | Exploratory analysis notebook (see below) |

## Dataset

Each row is one product line within a customer invoice.

| Column | Description |
|---|---|
| `Invoice` | Invoice number. Values starting with `C` are cancellations |
| `StockCode` | Product/item code |
| `Description` | Product name |
| `Quantity` | Units sold (negative = return/cancellation) |
| `InvoiceDate` | Timestamp of the transaction |
| `Price` | Unit price (GBP) |
| `Customer ID` | Buyer identifier (missing on ~23% of rows — unidentified/guest purchases) |
| `Country` | Customer's country |

**Size:** ~1.07M rows combined, 4,000+ customers, 4,000+ distinct products, ~40 countries.

## What the notebook does

`online_retail_eda.ipynb` walks through a first-pass exploratory analysis, step by step:

1. **Load & combine** both sheets into a single DataFrame (tagged with `SheetYear`)
2. **Data quality check** — dtypes, missing values, duplicate/negative edge cases
3. **Flag cancellations and returns**
   - Cancellations: invoice numbers starting with `C`
   - Returns: negative `Quantity` lines
   - Includes the **products with the most cancellations** (by units cancelled and by number of cancellation lines)
4. **Build a cleaned sales table** (`df_clean`): excludes cancellations and zero/negative price or quantity rows, adds a computed `Revenue = Quantity × Price` column
5. **Top 10 customers** — by total spend and by number of distinct orders
6. **Top 10 products** — by units sold and by revenue
7. **Revenue, orders and customers by country** (with a bar chart of the top 10)
8. **Monthly revenue trend** (line chart)
9. **Basket size & order value distribution** — items per order, units per order, order value
10. **RFM snapshot** (Recency, Frequency, Monetary) per customer — a standard starting point for customer segmentation
11. **Data-quality recap** — a short summary of caveats to keep in mind for any deeper analysis (missing customer IDs, cancellations, stray negative/zero prices, two separate yearly sheets)

## Key caveats to carry forward

- **Customer-level analysis** (top customers, RFM, segmentation) only covers the ~77% of rows with a known `Customer ID` — unidentified/guest orders are excluded.
- **Cancellations and returns** are excluded from the cleaned revenue figures; they're analyzed separately where relevant (e.g. most-cancelled products).
- A small number of rows have a **zero or negative `Price`** without being cancellations (likely manual adjustments or data entry errors) — worth inspecting if they matter for a specific question.
- `InvoiceDate` (not the sheet name) should be used for any date-based grouping, since the two sheets correspond to two overlapping-but-distinct calendar windows.

## How to run

```bash
pip install pandas openpyxl matplotlib jupyter
jupyter notebook online_retail_eda.ipynb
```

Run all cells top to bottom — no other setup required.

## Next steps / ideas

- Customer segmentation (RFM-based clustering, CLV)
- Market basket analysis (association rules between products)
- Demand forecasting per product/country
- Cohort / retention analysis
