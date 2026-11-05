# Track B — Demand Forecasting Assessment (customer: AcmeBrew)

Hi, and thank you for applying to **Smartlog**.

Besides the Control Tower dashboards, Smartlog also builds **demand forecasting** for its customers: forecasts that tell a manufacturer how much of each product it will sell, so it can plan production, stock and replenishment. This track simulates that work. You join the project of a fictional customer, **AcmeBrew Vietnam**, receive two years of daily sales history, and produce a six-month daily forecast for every product.

**You may use any method you like**: classical statistics, machine learning, deep learning, pretrained foundation models, heuristics, or a combination. We do not have a preferred model. The method has to fit the data, and **you have to be able to explain why you chose it**. A well-justified seasonal baseline is worth more to us than a complex model nobody can explain.

---

## General information

- **Time**: **7 calendar days** from when you receive the test. We expect about **15–25 hours of focused work**. Finishing a simpler approach properly is better than leaving an ambitious one half done.
- **Tools**: your choice. Python and R are the usual options, but anything that produces a reproducible result is fine.
- **Language**: English or Vietnamese, used consistently.
- **Compute**: a normal laptop is enough. Cloud or GPU is allowed if your method needs it, but we must be able to rerun your code (see *Reproducibility*).
- **Who takes this track**: candidates with a statistics, data science or ML background, from fresh graduates to experienced practitioners. We adjust what we expect to your experience.

---

## Context

AcmeBrew sells **108 SKUs** (instant mixes, roasted products and a few accessories) through general trade, modern trade and key accounts, from 5 distribution centers across Vietnam. Every month, the **Head of Demand Planning** signs off a forecast that drives the production plan.

The current forecast process has known weaknesses. Management wants to know whether a better forecast is possible, how accurate it can realistically be, and where it can and cannot be trusted. You are the Smartlog analyst assigned to answer that.

Read [`dataset/README.md`](dataset/README.md) first. It describes the five files, the target, and some things worth knowing about the data.

---

## The forecasting task

| | |
|---|---|
| **Target** | `qty_kg`: daily demand in kg, **per SKU, national** (summed over all DCs and channels) |
| **History you receive** | 2024-01-02 → 2025-12-31 |
| **Forecast origin** | **2025-12-31**: you know everything up to and including this date, and nothing after it |
| **Horizon** | **2026-01-01 → 2026-06-30**, every day (181 days, 1 to 181 days ahead) |
| **Grid** | All 108 SKUs × 181 days = 19,548 numbers, exactly as in `submission_template.csv` |

We hold the actual 2026 sales and will score your forecast against them. This is a single forecast made at one origin, as a planner would make it on 31 December. Rolling or updating the forecast during the horizon is not part of the task.

**External data** is allowed if it is public, would have been available on 2025-12-31, and is documented in your report (for example holiday calendars or macro indicators). Do not try to work out who the real company is. The names and scale have been changed, and identifying the company would not help your forecast.

---

## What we ask you to do

The parts below follow the order a real project would take. **Parts 1–4 are required.** Part 5 is optional.

### Part 1: Understand the demand

Before modelling, show us that you understand what you are forecasting. Focus on what matters for the model choice. Do not try to cover everything.

- What patterns does the demand have (trend, weekly, within-month, yearly, holidays/Tet)? How strong is each, and at which level (total, segment, SKU)?
- How different are the SKUs from each other? Segment them in a way that later drives modelling decisions (for example by volume, variability or intermittency).
- What in the data could mislead a model (spikes, structural breaks, launches and discontinuations, zeros, the target definition)? What did you decide to do about each issue, and why?

### Part 2: Choose and justify your approach

- Describe the approach you chose, end to end: the level you forecast at, any transformations, features, the model(s), how you handle the different SKU segments, and any post-processing or reconciliation.
- **Explain why this approach fits this data.** Link each main design decision back to something you found in Part 1.
- List the **alternatives you considered or tried** and why you dropped them. Include at least one simple baseline (for example seasonal naive or a moving average) that your approach must beat.
- State the assumptions and the known limitations of your approach.

### Part 3: Prove it with a backtest

You do not have the 2026 actuals, so you have to estimate how good your forecast is from the history.

- Design a **time-based validation** that mimics the real task: a single origin, a long horizon, and no information from after the origin. Explain your design, for example why you chose this origin or these folds.
- Report errors **against your baseline(s)**, overall and by segment. Use at least the metrics below, and add any others you find useful.
- Explain how you made sure that **no future information leaked** into training, features, scaling, target encoding or model selection.
- Based on the backtest, what accuracy do you **expect** for the 2026 forecast? We will compare your estimate with your actual score, so a well-calibrated expectation counts in your favour.

### Part 4: Deliver the forecast and explain it to the business

1. **`forecast.csv`**: `submission_template.csv` with `forecast_qty_kg` filled in for every row. The values must be numbers, non-negative and in kg, with no missing rows. The header must be exactly `sku_code,date,forecast_qty_kg`, with optional extra columns described in Part 5.
2. **A one-page memo to the Head of Demand Planning** (not to a data scientist). It should cover:
   - how accurate the forecast is expected to be, in business terms (for example "monthly volume per SKU within ±X% for the top SKUs");
   - which products the forecast can be trusted for, and which ones it cannot;
   - how the planning team should use the forecast, and what they should still decide by judgement;
   - the 2–3 next steps that would improve accuracy the most, such as data the company should start collecting or process changes.

### Part 5 (optional bonus)

Pick any of these if you have time. None of them is needed for a strong result.

- **Uncertainty**: add `p10` and `p90` columns to `forecast.csv`, giving a prediction interval per SKU per day. We will check how often the actuals fall inside it.
- **DC level**: submit `forecast_by_warehouse.csv` with columns `sku_code,warehouse_code,date,forecast_qty_kg`, a forecast per SKU × DC × day.
- **Something we did not ask for** that you think a planner would value.

---

## How we evaluate the forecast

We compare `forecast.csv` with the hidden 2026 actuals using **WAPE** (Weighted Absolute Percentage Error), at four levels of aggregation:

```
WAPE = Σ |forecast − actual| / Σ actual × 100%
```

| Metric | How errors are summed |
|---|---|
| `WAPE_day` | per SKU × day |
| `WAPE_week` | forecast and actual first summed per SKU × calendar week, then WAPE |
| `WAPE_month` | forecast and actual first summed per SKU × calendar month, then WAPE. **This is the main business metric** |
| `WAPE_period` | summed per SKU over the whole 6 months, then WAPE |
| `Bias` | (Σ forecast − Σ actual) / Σ actual × 100% |

Errors are summed **per SKU first**, so an over-forecast on one SKU cannot cancel an under-forecast on another. Because WAPE weights by volume, the large SKUs dominate the score, but your report should still show how you handled the small ones.

We also report the results by SKU segment and after summing the SKUs up to category level.

---

## Deliverables

Send a Google Drive link or a zip file to the recruiter with the subject `[DA Assessment – Track B] <Your name>`, containing:

```
<your-name>/
├── forecast.csv                 # REQUIRED: the filled template
├── report.pdf | report.md | report.ipynb   # REQUIRED: Parts 1–3 (≈ 10–15 pages or an equivalent notebook)
├── memo.pdf | memo.md           # REQUIRED: Part 4 memo, 1 page
├── model_card.md                # REQUIRED: 1 page (see below)
├── notes.md                     # REQUIRED: behind the scenes (see below)
├── code/                        # REQUIRED: everything needed to reproduce forecast.csv
│   ├── README.md                #   how to run it: one command if possible
│   └── requirements.txt | environment.yml | renv.lock
└── forecast_by_warehouse.csv    # optional (Part 5)
```

**`model_card.md`** (1 page): inputs used, training window, features, model(s) and key hyper-parameters, how SKUs without history are handled, runtime, known limitations, and situations in which the model should not be used.

**`notes.md`** (1 page):
- Roughly how many hours you spent, and on which parts.
- Which AI tools you used (if any) and for what. Using AI is fine, but you must understand and be able to defend every part of your submission.
- Approaches you tried and dropped, with 1–2 sentences on each.
- What you would do with one more week.

**Reproducibility**: running your code from a clean environment must produce your `forecast.csv`, or a file within a negligible difference of it. Set random seeds, pin library versions, and state the runtime and the hardware you used.

---

## Scoring criteria

| # | Area | Weight | What we look at |
|---|---|---|---|
| 1 | **Problem framing & data understanding** | 20% | Did you find the patterns and traps that matter for forecasting? Did your findings drive your later decisions? |
| 2 | **Method choice & justification** | 25% | Does the approach fit the data? Can you explain why it beats the alternatives, including its weaknesses? |
| 3 | **Validation rigor** | 20% | Is the backtest realistic and leakage-free, with honest baselines? Is your expected accuracy well calibrated? |
| 4 | **Forecast accuracy** | 15% | Your `forecast.csv` scored on the hidden 2026 actuals and compared with simple baselines |
| 5 | **Business communication** | 10% | Can the Head of Demand Planning understand and act on your memo? |
| 6 | **Reproducibility & code quality** | 10% | Does the code run, is it readable, and does it reproduce your forecast? |

Accuracy counts, but it is deliberately **not** the largest weight. Six months of one real year includes luck. What we can judge reliably is how you reason.

---

## Discussion session

If your submission passes, we invite you to a **60-minute discussion**: a 10-minute presentation followed by questions. Expect questions such as "why this model and not X?", "what happens to your forecast if Tet moves two weeks?" and "show me where in the code you prevent leakage". We may also ask you to change one thing and rerun it live.

---

## A few notes

- **There is no single right model.** Statistical, ML, DL, foundation-model and hybrid approaches are all welcome, as long as you can defend the choice with evidence.
- **Simple is fine.** If a seasonal naive or ETS model wins your backtest, submit it and explain why. That is a valid and respected result.
- **Write down your assumptions.** If something is unclear, do not ask us. State your assumption ("I assume X because Y") and continue. Handling ambiguity is part of the job.
- **Keep the data confidential.** Do not publish the dataset or your forecast publicly, for example in a public GitHub repo or on Kaggle.

Good luck, and have fun with it.

— Hiring Team, Smartlog
