# Data Analyst Assessment — Smartlog (customer: AcmeFoods)

Hi, and thank you for applying for the **Data Analyst** role at **Smartlog**.

Smartlog provides a **Control Tower** platform plus a DA team that helps logistics customers analyze their data and build dashboards. As a Smartlog DA, you will **work on the customer's operational data**. The main outputs are **dashboards + insight reports** for the customer's stakeholders (usually the Supply Chain Manager). This test simulates exactly that situation — you have just been assigned to the project of a fictional customer, **AcmeFoods**.

This is a take-home test to help us understand how you **approach a new customer's data**, **ask the right questions**, and **tell a story to business stakeholders (not internal developers)**. There is NO single "right answer" — what matters is how you think.

---

## General information

- **Who takes this test**: all DA candidates, from **interns** to **experienced analysts**. We adjust our expectations to your level of experience — an intern is not expected to produce the same depth as a senior analyst.
- **Time**: target ~3 hours, hard cap 4 hours (timeboxed — please do not spend more than 4 hours; we care more about how you manage your time than about finishing everything).
- **Deadline**: 48 hours after you receive the test.
- **Tools**: your choice — SQL (DuckDB/SQLite/Postgres, etc.), Python (pandas/polars), R, Excel/Google Sheets, Power BI, Tableau, Metabase, Looker Studio... anything you are comfortable with.
- **Language**: English or Vietnamese — both are fine, just use one consistently.

---

## Context

**AcmeFoods Vietnam** (a fictional Smartlog customer) is an FMCG confectionery company that sells through several channels (supermarkets, grocery stores, e-commerce, horeca). They **outsource all of their transport** to a small group of partner carriers. Every month, the AcmeFoods logistics team meets with the **Supply Chain Manager (SC Manager)** — this is the customer-side stakeholder for whom you (the Smartlog DA) build dashboards and send insights.

You have just been assigned to the AcmeFoods project and received a dataset covering **3 months of operations, Feb-Apr 2026 (2026-02-01 → 2026-04-30)** from the Smartlog Control Tower system. Read `dataset/README.md` carefully before you start — it describes the 5 CSV files (shipments, trips, carriers, locations, products), how they relate to each other, and a glossary of terms.

We do **NOT** give you a list of KPIs to calculate, and we do **NOT** give you specific questions to answer. Real customers often do not know what they want their dashboard to show either — much of the value of a good Smartlog DA comes from **knowing which questions to ask** before writing SQL and building charts.

---

## Requirements

The test has 4 parts. **You do not have to complete all of them** — if time is short, prioritize Part 1 and Part 3 (required), then choose either Part 2 or Part 4.

### Part 1 (REQUIRED) — Data Profiling

Before analyzing anything, **explore the data**. Write a short answer (~300-500 words) covering:

1. How many rows does each file have? What is the actual date range?
2. What **data quality issues** can you find? (e.g. NULLs, duplicates, outliers, strange values, inconsistencies...) — list as many as you can.
3. **3-5 first observations** that you find interesting or worth digging into. They do not have to be KPIs — they can be patterns, anomalies, unusual distributions, or relationships between columns.
4. Based on what you see, **suggest 2-3 business questions** the SC Manager might care about.

> Tip: run a simple `.describe()` / `COUNT(*)` / `GROUP BY` first before jumping into complex KPIs. Profiling is the foundation — doing it carefully usually saves time later.

---

### Part 2 (CHOOSE Part 2 OR Part 4) — Deep dive into 1 KPI you define

Choose **1 important KPI** that you think can be calculated from this dataset. You define the metric, the formula, and why it matters.

Some ideas (not a complete list, and you do not have to use them): on-time delivery, in-full delivery, vehicle fill rate, lead time, carrier performance, hidden costs (e.g. over-delivery), ... — or any other metric you come up with.

Requirements:
1. **Clear definition**: metric name, formula (text or SQL), and why it matters for the business. How do you handle NULLs and edge cases? Explain the trade-offs.
2. **Break it down by at least 2 dimensions**: month, sales channel, warehouse, delivery area, carrier, vehicle type, cargo group... your choice.
3. **A table + at least 1 chart** to present the results.
4. **3-5 sentences of commentary**: what do the numbers say? Is anything unusual?

---

### Part 3 (REQUIRED) — Email from the SC Manager

The SC Manager sends you a short email:

> *"Hi,*
>
> *You just got 3 months of operations data. Quick questions:*
>
> *(a) Is there **anything worth noting** in these 3 months? Any strange patterns, anomalies to watch, or carriers/regions/vehicle types causing problems?*
>
> *(b) If you had to pick **1 priority issue** to fix next week, which would you pick? Why that one and not something else?*
>
> *(c) What actions do you recommend? Specifically: who should do what, which metric do we use to measure it, and what is the target and timeline?"*

Reply in ~400-600 words, with 1-2 charts/tables to support your answer.

**Notes**:
- You decide **what is "worth noting"** — there is no guideline. This part tests your ability to ask questions.
- If the data does **not** support a conclusion, say clearly "there is no evidence" instead of making something up.
- Recommended actions must be **specific**: what to do, who does it, and how to measure success.
- Clearly separate what is a **fact** (the numbers say so) and what is a **hypothesis** (your guess).

---

### Part 4 (CHOOSE Part 2 OR Part 4) — Propose 1 dashboard widget

> This part is very close to the real day-to-day work of a Smartlog DA. You do not have to choose it, but if you do, it is a chance to show a "build for the customer to use" mindset (not just internal analysis).

If you could build **only 1 widget** on the AcmeFoods SC Manager's Control Tower dashboard, what would it be?

1. **The question the widget answers**: 1 clear sentence.
2. **Visualization**: what chart type? (bar / line / heatmap / KPI card / table / ...)
3. **Mockup**: hand-drawn / made with a tool / described in words — it does not need to look nice, it needs to communicate the idea.
4. **Refresh frequency**: real-time / hourly / daily / weekly?
5. **Edge cases**: what does the empty state show? What happens if data fails to load?
6. **Why this one**: why this widget and not another? (3-5 sentences)

---

## Deliverables

Submit 1 zip file or a Google Drive link with:

1. **Main report** (`report.pdf`, `report.md` or a notebook `.ipynb`).
2. **Code/queries** you used (if any): `.sql`, `.py`, `.xlsx` files... in a `code/` folder.
3. **Chart exports** (if charts cannot be embedded in the report).
4. **A `notes.md` file** (1 page) — the "behind the scenes":
   - Which part did you spend the most time on? Why?
   - Were there any assumptions or approaches you considered and then dropped? 1-2 short sentences for each.
   - If you had 2 more hours, what would you do next?

---

## Scoring criteria

Your submission is scored on 4 areas (we will share the details after the discussion session):

| # | Area | Weight | What we look at |
|---|---|---|---|
| 1 | **Curiosity & Framing** | 30% | Did you **ask the right questions** before calculating? How deep is your profiling? Do you clearly separate fact from hypothesis? |
| 2 | **Execution** | 25% | How you write queries / pandas / Excel — correct, efficient, reproducible? Are your metric definitions defensible? |
| 3 | **Insight quality** | 25% | What patterns did you find? Did you break down the root cause? Are your recommendations actionable? |
| 4 | **Communication** | 20% | Can a business stakeholder (who does NOT know code) understand it? Are the charts clear? |

---

## A few notes

- **It does not need to be "perfect"** — we care more about **how you think** than about the amount of output. 1 deep analysis is usually worth more than 5 shallow ones.
- **No KPI is the "right answer"** in this test. Whether you choose OTIF, On-Time, In-Full, VFR, lead time, carrier performance, or another metric — it is all fine as long as you can defend your choice.
- **Do not just copy AI output**: using AI to help is OK — but you must understand the output. The discussion session will include follow-up questions, and it will be obvious if you do not understand your own work.
- **Write down your assumptions**: if something in the test is unclear, **do not ask us** — write your assumption in the report (e.g. "I assume X because Y") and keep going. Being able to state and defend your own assumptions is part of the test.

Have fun with it.

— Hiring Team, Smartlog
