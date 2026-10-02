# Dataset — Smartlog × AcmeFoods (Sample, Feb-Apr 2026)

A sample dataset **exported from Smartlog Control Tower** for a made-up customer — **AcmeFoods Vietnam** — an FMCG confectionery / snack food company, covering **3 months of operations, Feb-Apr 2026 (2026-02-01 → 2026-04-30)**.

> **Note**: This is a fictional dataset used for DA interviews at Smartlog. Customer, carrier, brand, warehouse and delivery area names are all fake. The numbers (dates, volumes, rates) keep realistic distributions so the analysis is meaningful.

---

## Business context

AcmeFoods (a fictional Smartlog customer) makes confectionery and snacks, and sells through several channels (supermarkets, traditional grocery stores, e-commerce, horeca). The company has a **network of warehouses** across Vietnam and **outsources 100% of its transport** to a small group of partner carriers. AcmeFoods' logistics operations run on the **Smartlog Control Tower** platform — the dataset you receive is an export from this platform.

Each Sales Order (**SO**) goes through these time milestones:
1. **GI date** — Goods Issue date (the day goods leave the warehouse stock).
2. **ETD planned** — planned time the truck leaves the warehouse.
3. **ETA planned** — planned time of delivery to the customer.
4. **ATD actual** — actual time the truck left the warehouse.
5. **ATA actual** — actual time of delivery to the customer.

The AcmeFoods logistics team needs regular reports / dashboards for their **Supply Chain Manager (SC Manager)**. The Smartlog DA (you, in this scenario) builds those dashboards and insights. Deciding what the right reports are is part of your job.

---

## Glossary

| Term | Meaning |
|---|---|
| **SO** | Sales Order — one customer delivery order |
| **GI** | Goods Issue — goods are picked and released from the warehouse |
| **ETD / ETA** | Estimated (planned) Time of Departure / Arrival |
| **ATD / ATA** | Actual Time of Departure / Arrival |
| **OTIF** | On-Time In-Full — the order was delivered on time **and** in the full quantity |
| **On-Time** | Delivered by the planned time (ETA) |
| **In-Full** | Delivered quantity matches the planned quantity |
| **VFR** | Vehicle Fill Rate — how full the truck is compared to its capacity (by weight or volume) |
| **CSE** | Case — the unit of quantity (one carton/case of product) |
| **DC** | Distribution Center (warehouse) |
| **Carrier** | Third-party transport company that runs the trucks |
| **Tender** | Booking a truck from a carrier for a trip |
| **GT** | General Trade — traditional channel (small grocery stores, wholesalers) |
| **MT** | Modern Trade — supermarkets, convenience stores |
| **KA** | Key Accounts — large strategic customers |
| **POSM** | Point-of-Sale Materials — marketing materials (displays, posters), not products for sale |
| **Tet** | Vietnamese Lunar New Year — the biggest holiday and sales season in Vietnam |

---

## Dataset structure

5 CSV files in this folder, encoded as UTF-8 with BOM (opens directly in Excel):

| File | Type | Description | Granularity | Row count |
|---|---|---|---|---|
| `shipments.csv` | Fact | 1 row per SO (delivery order) | Order | ~31k |
| `trips.csv` | Fact | 1 row per truck trip | Trip | ~6.6k |
| `carriers.csv` | Dim | List of carriers | Carrier | 11 |
| `locations.csv` | Dim | Warehouses + delivery areas + pickup points | Location | 19 |
| `products.csv` | Dim | Brands + cargo groups + sales channels | Product | 40 |

There are no enforced foreign keys — you need to **join by code** (see § Relationships).

---

## Detailed schema

### `shipments.csv`

| Column | Type | Description |
|---|---|---|
| `shipment_id` | string | Unique order ID, format `SH-2026-XXXXXX` |
| `warehouse_code` | string | Shipping warehouse code, FK → `locations.location_code` (location_type='WAREHOUSE') |
| `delivery_area` | string | Delivery area (Vietnamese region name, e.g. "Ha Noi", "Mekong 1") — may be empty |
| `cargo_group` | string | Cargo group — main values: `DRY`, `FRESH`, `POSM/OFFBOM`. The data may contain unusual values — profile it and decide how to handle them. |
| `carrier_code` | string | Carrier code, FK → `carriers.carrier_code` — may be empty |
| `sales_channel` | string | Sales channel (MT/GT/KA/DRP/B2B/EXPORT/OTHER) |
| `vehicle_type` | string | Truck type used (e.g. "1.4T" → "11T", with several sizes in between) — may be empty |
| `gi_date` | date | Goods Issue date — may be empty |
| `etd_planned` | datetime | Planned time the truck leaves the warehouse |
| `eta_planned` | datetime | Planned time of delivery to the customer |
| `atd_actual` | datetime | Actual time the truck left the warehouse |
| `ata_actual` | datetime | Actual time of delivery to the customer |
| `planned_qty_cse` | number | Planned quantity in cases (CSE) |
| `planned_weight_kg` | number | Planned weight (kg) |
| `planned_volume_cbm` | number | Planned volume (m³) |
| `planned_pallets` | number | Planned number of pallets |
| `delivered_qty_cse` | number | Actual quantity delivered in cases (CSE) |

### `trips.csv`

| Column | Type | Description |
|---|---|---|
| `trip_id` | string | Trip ID, format `TR-2026-XXXXXX` |
| `tender_date` | date | Date the truck was booked (tendered) |
| `eta_operation` | datetime | Operational ETA |
| `ata_operation` | datetime | Operational ATA |
| `pickup_location` | string | Pickup point code, FK → `locations.location_code` (location_type='PICKUP_LOCATION') |
| `delivery_area` | string | Delivery area (same values as in shipments) |
| `carrier_code` | string | FK → `carriers.carrier_code` |
| `vehicle_type` | string | Truck type (e.g. "1.4T" → "11T", "11T_16PL", with several sizes in between) |
| `cargo_group` | string | Cargo group (may contain several groups if the trip carries mixed cargo) |
| `vfr_pct` | number | Vehicle fill rate (%), 0-100 |
| `vfr_by_ton` | number | Fill rate by weight (%) |
| `vfr_by_volume` | number | Fill rate by volume (%) |
| `planned_ton` | number | Planned load weight (tons) |
| `planned_cbm` | number | Planned load volume (m³) |

### `carriers.csv`

| Column | Type | Description |
|---|---|---|
| `carrier_code` | string | PK, format `CARxxx` |
| `carrier_name` | string | Carrier name |

### `locations.csv`

| Column | Type | Description |
|---|---|---|
| `location_type` | string | `WAREHOUSE` / `DELIVERY_AREA` / `PICKUP_LOCATION` |
| `location_code` | string | PK within each location_type |
| `location_name` | string | Display name |
| `location_group` | string | Parent group (only for WAREHOUSE, may be empty) |

Note: `WAREHOUSE` and `PICKUP_LOCATION` are 2 different things (shipping warehouse vs. transit hub). `shipments.warehouse_code` joins to `WAREHOUSE`, `trips.pickup_location` joins to `PICKUP_LOCATION`.

### `products.csv`

| Column | Type | Description |
|---|---|---|
| `dim_type` | string | `BRAND_CARGO` or `SALES_CHANNEL` |
| `code` | string | PK within each dim_type (e.g. brand code, channel code) |
| `name` | string | Display name |
| `parent_group` | string | For `BRAND_CARGO`: parent cargo group. For `SALES_CHANNEL`: empty. |

---

## Relationships

```
                    +-----------------+
                    |  carriers.csv   |
                    +--------+--------+
                             |
              carrier_code   |
                             |
+-----------------+   +------v-----------+   +------------------+
|  locations.csv  +---+  shipments.csv   +---+  products.csv    |
+-----------------+   +------------------+   +------------------+
        ^
        |
        |
+-------+----------+
|  trips.csv       |
+------------------+
```

- `shipments` joins `carriers` on `carrier_code`.
- `shipments` joins `locations` on `warehouse_code` (filter `location_type='WAREHOUSE'`) or `delivery_area` (filter `location_type='DELIVERY_AREA'`).
- `shipments` joins `products` on `sales_channel` (filter `dim_type='SALES_CHANNEL'`).
- `trips` joins `locations` on `pickup_location` (filter `location_type='PICKUP_LOCATION'`).
- `trips` does not link 1-to-1 with `shipments` — 1 trip can carry many shipments, or 1 shipment can be split across many trips. **Use your own judgment when you need to connect them.**

---

## Some quirks to know when profiling

1. **Empty cells = NULL**: many columns have empty values instead of a NULL marker. In pandas they become `NaN`. In Excel they show as empty cells.
2. **Date meaning**: `gi_date` is the Goods Issue date, `eta_planned` is the planned delivery date. **An SO can have `eta_planned` inside the window but `gi_date` outside it** (e.g. ETA at the end of April, GI at the start of May).
3. **Brand is not linked to shipments**: brand only exists in `products.csv` as a standalone dim, with no FK to `shipments`. If you want to analyze by brand, you need to decide how.
4. **Over-delivery**: some orders have `delivered_qty_cse > planned_qty_cse`. This could be a data error or extra delivery requested by the customer — you decide how to handle it.
5. **Multi-cargo trips**: `trips.cargo_group` can contain several groups separated by commas (e.g. "DRY, FRESH") when one trip carries mixed cargo.
6. **Mixed vehicle types**: many truck types (1.4T → 11T) with different capacities — be careful when comparing metrics across types.
7. **Encoding**: files are saved as UTF-8 with BOM; Excel and pandas open them directly.

---

## Sample queries (just to warm up — you do not have to follow this direction)

```sql
-- Number of shipments by month × sales channel
SELECT
    DATE_TRUNC('month', gi_date)  AS month,
    sales_channel,
    COUNT(*)                       AS total_so,
    SUM(planned_qty_cse)           AS total_cse_planned,
    SUM(delivered_qty_cse)         AS total_cse_delivered
FROM shipments
WHERE gi_date IS NOT NULL
GROUP BY 1, 2
ORDER BY 1, 2;
```

```python
# pandas equivalent
import pandas as pd
df = pd.read_csv('shipments.csv', parse_dates=['gi_date','eta_planned','ata_actual'])
df['month'] = df['gi_date'].dt.to_period('M')
agg = df.groupby(['month','sales_channel']).agg(
    total_so=('shipment_id','count'),
    total_cse_planned=('planned_qty_cse','sum'),
    total_cse_delivered=('delivered_qty_cse','sum'),
)
```

---

Good luck — be curious.
