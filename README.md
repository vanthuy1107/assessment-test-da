# Data Analyst Assessment — Smartlog

This repo contains the **take-home assessment for data roles at Smartlog**. There are **two tracks**. **Choose one**: the one that fits your profile and the work you want to do.

## About us

### Smartlog — [gosmartlog.com](https://gosmartlog.com/)

**Smartlog** (Smartlog Supply Chain Solutions JSC, Ho Chi Minh City) builds supply chain software for manufacturers, distributors and logistics providers in Vietnam. Its products cover the main operational flows of a supply chain:

| Product | Area |
|---|---|
| **STM** | Transport management |
| **SWM** | Warehouse management |
| **SSM** | Operations |
| **STX** | Freight exchange |
| **COS** | Container operations |

These systems run our customers' daily operations, and the data they produce is what the data team works with.

### HeliX — [helix.smartlogvn.com](https://helix.smartlogvn.com/)

**HeliX** is Smartlog's data processing and solution R&D hub, and the team you would join. It builds **Smartlog Control Tower**, which brings a customer's TMS, WMS and ERP data together in one place, built on a catalogue of 103+ standardised logistics KPIs. The work runs from raw operational tables to standardised KPIs to dashboards, and further into forecasting and optimisation.

HeliX offers **6 solution modules** on one data platform:

| # | Module | What it does |
|---|---|---|
| 1 | **Control Tower** | Real-time, end-to-end visibility of operations, with alerts and 103+ standardised KPIs in one interface |
| 2 | **Demand Forecasting** | Machine-learning forecasts of demand by SKU, region and route, with accuracy and bias tracking |
| 3 | **Digital Twin** | Live 3D/2D view of docks, yards and warehouse floors; flow simulation and storage heatmaps are on the roadmap |
| 4 | **AI Analyst** | Ask operational data questions in plain language and get the root cause, with every answer traceable to its SQL |
| 5 | **Advanced Analytics** | Deep-dive supply chain analysis beyond the standard dashboards |
| 6 | **Network Optimization** | Distribution network design and simulation (future vision) |

The two tracks below mirror that work. **Track A** is the Control Tower side: turning operational data into KPIs and recommendations. **Track B** is the forecasting side: predicting demand so that capacity, stock and fleet can be planned ahead.

The HeliX website has the [dashboard library](https://helix.smartlogvn.com/dashboards), the [KPI catalogue](https://helix.smartlogvn.com/kpi) and the [team](https://helix.smartlogvn.com/about), which are useful background for either track.

---

> **About the data**: both datasets come from real operations, with every company, product, carrier and location name replaced by fictional names. Distributions and patterns are kept, so the analysis is meaningful.

---

## Choose your track

| | **Track A: Logistics Operations Analytics** | **Track B: Demand Forecasting** |
|---|---|---|
| Fictional customer | AcmeFoods (FMCG confectionery) | AcmeBrew (packaged beverages) |
| The question | "What is happening in our transport operations, and what should we fix first?" | "How much of each product will we sell every day for the next 6 months?" |
| Data | 3 months of shipments and truck trips | 2 years of daily sales for 108 SKUs |
| Main output | Data profiling, KPI analysis, a reply to a Supply Chain Manager, a dashboard widget | A 6-month daily forecast per SKU, a backtest, and a memo to the Head of Demand Planning |
| Skills it shows | SQL/Excel/BI, KPI definition, business storytelling | Time-series analysis, statistics/ML/DL, validation design, explaining a model |
| Method | Any tool | **Any model** (statistical, ML, DL, foundation models, heuristics) as long as you can justify it |
| Time | ~3–4 hours of work, **48 hours** deadline | ~15–25 hours of work, **7 days** deadline |
| Start here | [`track-a-logistics/assessment.md`](track-a-logistics/assessment.md) | [`track-b-demand-forecasting/assessment.md`](track-b-demand-forecasting/assessment.md) |

Not sure which one to choose? If you enjoy turning operational data into dashboards and recommendations, choose **A**. If you enjoy building and validating predictive models, choose **B**. Doing both is not required and gives no extra credit.

> **Who is this for?** All candidates, from **interns** to **experienced analysts and data scientists**. We look at how you think, and we adjust our expectations to your level of experience.

---

## Repo structure

```
.
├── README.md                          # This file
├── track-a-logistics/
│   ├── assessment.md                  # Track A: the task
│   └── dataset/                       # shipments, trips, carriers, locations, products + README
└── track-b-demand-forecasting/
    ├── assessment.md                  # Track B: the task
    └── dataset/                       # sales_history, products, warehouses, calendar,
                                       # submission_template + README
```

---

## Submission

Send a Google Drive link or a zip file to the recruiter with the subject `[DA Assessment – Track A] <Your name>` or `[DA Assessment – Track B] <Your name>`. Each `assessment.md` lists exactly what to include.

## Questions

If something about the data or the business context is unclear, write your assumption down in your submission ("I assume X because Y") and continue. We want to see how you handle ambiguity, not whether you guess right.

Have fun :)
