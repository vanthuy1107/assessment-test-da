# Dataset — AcmeBrew daily sales history (Jan 2024 – Dec 2025)

Daily sales history of **AcmeBrew Vietnam**, a fictional packaged-beverage manufacturer, exported from its ERP for this assessment.

> **About the data**: the company, product, category and warehouse names are all fictional, and the codes are randomly generated. The **demand patterns are real**: dates, weekdays, holidays, seasonality, spikes, intermittency and the mix between products are kept as they are. To protect confidentiality, every quantity has been multiplied by one constant that we do not disclose. Ratios, growth rates, shares and forecast-error percentages are not affected by this.

---

## Business context

AcmeBrew makes packaged beverages in three segments:

| Segment | What it is | Share of volume (2024–2025) |
|---|---|---|
| `INSTANT` | Instant mixes (sticks, boxes, bags) | ~89% |
| `ROASTED` | Roasted products (bags, bulk sacks) | ~11% |
| `OTHER` | Accessories, gift sets, point-of-sale items | <0.1% |

Products ship from **5 distribution centers (DCs)** to customers in three **sales channels**:

| Channel | Meaning |
|---|---|
| `GT` | General Trade: distributors, wholesalers, traditional grocery stores |
| `MT` | Modern Trade: supermarket and convenience-store chains |
| `KA` | Key Accounts: large direct customers (offices, HoReCa groups, …) |

The demand planning team sets a forecast every month. The forecast drives the production plan and DC replenishment. **Planning works in kilograms (kg)** because kg is the only unit that adds up across products (a "box" of one product and a "bag" of another are not comparable).

---

## Files

| File | Grain | Rows | Description |
|---|---|---|---|
| `sales_history.csv` | date × SKU × DC × channel | 156,456 | Daily sales, **2024-01-02 → 2025-12-31** |
| `products.csv` | SKU | 108 | Product master (all SKUs you must forecast) |
| `warehouses.csv` | DC | 6 | DC master |
| `calendar.csv` | date | 912 | Vietnamese public holidays and Lunar New Year (Tet), **2024-01-01 → 2026-06-30** (covers the forecast window) |
| `submission_template.csv` | SKU × date | 19,548 | The exact grid you must fill: 108 SKUs × 181 days (2026-01-01 → 2026-06-30) |

All files are UTF-8 CSV with a comma separator, and dates use the `YYYY-MM-DD` format.

---

## Schema

### `sales_history.csv`

A row exists only when there were sales. **A missing row means zero sales** for that date × SKU × DC × channel.

| Column | Type | Description |
|---|---|---|
| `date` | date | Order document date (the demand date) |
| `sku_code` | string | Product code, FK → `products.sku_code` |
| `warehouse_code` | string | Shipping DC, FK → `warehouses.warehouse_code`. `DC-UNK` = no DC recorded on the order line |
| `channel` | string | `GT` / `MT` / `KA` |
| `qty_kg` | float | **Demand quantity in kg: this is the forecast target.** Sum of kg over all order lines of the day |
| `promo_qty_kg` | float | Part of `qty_kg` given as free goods (promotion). Already included in `qty_kg` |
| `return_qty_kg` | float | Part of `qty_kg` on lines that were later credited back (returns or corrections). Already included in `qty_kg` |
| `net_value` | float | Net billed value in an **index currency** (not VND). It can be negative when returns exceed sales in that cell |
| `n_orders` | int | Number of distinct sales orders in this cell |
| `n_customers` | int | Number of distinct customers in this cell |

> `n_orders` and `n_customers` are distinct counts **inside the cell**. One order can cover several SKUs, so summing these columns across SKUs or DCs over-counts.

### `products.csv`

| Column | Type | Description |
|---|---|---|
| `sku_code` | string | PK |
| `sku_name` | string | Display name (fictional) |
| `category_code` | string | Product category (32 categories). Each SKU belongs to exactly one category |
| `category_name` | string | Category name (fictional brand + segment) |
| `segment` | string | `INSTANT` / `ROASTED` / `OTHER` |
| `pack_type` | string | Main selling unit: `BOX`, `BAG`, `BULK`, `CAN`, `POUCH`, `EACH`, … |
| `first_sale_date` | date | First date with sales in `sales_history.csv` (empty if none) |
| `last_sale_date` | date | Last date with sales in `sales_history.csv` (empty if none) |

### `warehouses.csv`

| Column | Description |
|---|---|
| `warehouse_code` | PK |
| `warehouse_name` | Display name |
| `region` | Region the DC serves |

### `calendar.csv`

| Column | Description |
|---|---|
| `date` | Every day from 2024-01-01 to 2026-06-30 |
| `day_of_week` | Monday … Sunday |
| `public_holiday` | Name of the statutory public holiday (empty if none). Tet covers the whole official break |
| `is_day_off` | 1 if it is a public holiday |
| `days_to_lunar_new_year` | Days until (negative) or since (positive) Lunar New Year day 1 of the same calendar year |
| `event` | `Lunar New Year (day 1)`, `Mid-Autumn Festival`, or empty |

We compiled this calendar from public sources. It only lists statutory holidays, so it does not include company-specific days off or bridge days. You may correct or extend it, as long as you document what you changed.

### `submission_template.csv`

| Column | Description |
|---|---|
| `sku_code` | 108 SKUs |
| `date` | 2026-01-01 → 2026-06-30 |
| `forecast_qty_kg` | **Empty: you fill this in** |

---

## Things worth knowing before you start

1. **Zeros are implicit.** Build a complete date × SKU calendar yourself before computing averages, lags or rolling windows. Otherwise you average only over the days that had sales.
2. **The target is national.** The required forecast is per SKU per day, summed across all DCs and channels. DC and channel are there to help you, and for an optional bonus (see the assessment).
3. **Some SKUs have very little history, or none.** Three SKUs in the template never appear in `sales_history.csv`, and some stopped selling during 2025. You must still submit a number for every row of the template, so decide how to handle these SKUs and explain your choice.
4. **The data has not been cleaned of real-world behaviour.** There are very large one-off days, quiet holiday periods and almost no Sunday sales. Treat them as data to understand. They are not errors that we planted.
5. **`qty_kg` includes free goods and credited lines.** This is how the planning team counts demand today, and it is the quantity we evaluate. If you think a different definition would be better for the business, say so in your report, but still submit `forecast_qty_kg` on the current definition.
