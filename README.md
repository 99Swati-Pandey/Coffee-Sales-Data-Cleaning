# Coffee Shop Sales: Data Cleaning & Feature Engineering (Excel Power Query)

## Overview
A practice project that takes the raw **Coffee Shop Sales** transaction log (149,116 rows, Jan–Jun 2023, 3 New York stores) and turns it into an analysis-ready table using **Power Query in Excel**. All steps live in one repeatable query, so the cleaning can be re-run with a single **Refresh All**.

**Tool:** Microsoft Excel (Power Query / M language)

## Files
| File | Description |
|---|---|
| `Coffee_Shop_Sales.xlsx` | Raw data, sheet `Transactions` (11 columns) |
| `practice_project_1_Coffee_Sales_Data_cleaning.xlsx` | Cleaned output, sheet `Sheet1` (16 columns), with the Power Query inside |

## Raw data
| Column | Description |
|---|---|
| `transaction_id` | Unique transaction number |
| `transaction_date`, `transaction_time` | When the sale happened |
| `transaction_qty` | Items in the transaction |
| `store_id`, `store_location` | Astoria, Hell's Kitchen, Lower Manhattan |
| `product_id`, `unit_price` | Product key and price per item |
| `product_category`, `product_type`, `product_detail` | Product hierarchy (detail includes the size, e.g. "Ethiopia Rg") |

**Profile of the raw data:** 149,116 rows, no blanks, no duplicate transaction IDs, 181 trading days (1 Jan – 30 Jun 2023), 9 product categories, 80 product IDs.

## What the Power Query does
1. **Loads** the `Transactions` sheet and promotes the headers.
2. **Sets data types:** dates, `transaction_time` as time, whole numbers for IDs and quantities, `unit_price` as currency.
3. **Standardises text:** proper case for `product_type` and `product_detail`.
4. **Extracts `size`** from the end of `product_detail` (`Rg` → Regular, `Lg` → Large, `Sm` → Small, otherwise Not Applicable), then removes the size code and trims `product_detail`.
5. **Adds `total_bill`** = `transaction_qty` × `unit_price` (currency).
6. **Adds date and time features:** `month_name`, `day_name`, `hour`.
7. **Renames and reorders** the columns into a logical layout.

## Cleaned output columns (16)
`transaction_id`, `transaction_date`, `month_name`, `day_name`, `transaction_time`, `hour`, `transaction_qty`, `unit_price`, `total_bill`, `store_id`, `store_location`, `product_id`, `product_category`, `product_type`, `product_detail`, `size`

## Validation checks
- Row count matches the raw data: **149,116 = 149,116**
- All `transaction_id` values are unique and the same set as the raw data
- No blank values in any column
- Total revenue matches between raw (qty × price) and cleaned (`total_bill`): **$698,812.33**
- `total_bill` equals `transaction_qty × unit_price` on every row

## Quick facts from the cleaned data
| | |
|---|---|
| Total revenue (Jan–Jun 2023) | $698,812 |
| Top category by revenue | Coffee (~$270K), then Tea (~$196K) |
| Stores | Hell's Kitchen ~$236.5K, Astoria ~$232.2K, Lower Manhattan ~$230.1K |
| Busiest month | June (~$166.5K); lowest was February (~$76.1K) |
| Peak hour by revenue | 10 am |

## Known limitations
- The first column's header in `Sheet1` is blank (it holds `transaction_id`). Rename it to `transaction_id`.
- The source data has a spelling variant, `Jamacian Coffee River` next to `Jamaican Coffee River`. It is kept as in the raw file. Merge them if you need a single product.
- The query reads the raw file from a local path (`C:\Users\...\Coffee Shop Sales.xlsx`). To refresh on another computer, change the path in the `Source` step.
- Rows are not sorted by `transaction_id`. Add a sort step if order matters.

## How to use
1. Open the cleaned workbook in Excel.
2. To review the steps, go to **Data → Queries & Connections → Transactions → Edit**.
3. Point the `Source` step to your copy of `Coffee_Shop_Sales.xlsx`, then **Refresh All**.

## Skills demonstrated
Power Query, M language, data typing, text cleaning, feature engineering (size, month, day, hour, total bill), validation against the raw data, documentation
