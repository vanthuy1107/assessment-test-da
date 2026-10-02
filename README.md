# Data Analyst Assessment — Smartlog (customer: AcmeFoods)

This repo contains the **Data Analyst take-home assessment** for the **DA role at Smartlog**, plus a **sample dataset** for you to work on at home.

> **Who is this for?** We use the same assessment for all DA candidates, from **interns** to **experienced analysts**. You do not need to do everything — we look at how you think, and we adjust our expectations to your level of experience.

> **About the role**: Smartlog provides a **Control Tower** platform plus a DA team that helps logistics customers analyze their data and build dashboards. A Smartlog DA works on the customer's operational data. The main outputs are **dashboards + insight reports** for the customer's stakeholders (for example, the Supply Chain Manager).

> **About the dataset**: This is **fictional data** that simulates the FMCG transport operations of a made-up customer ("AcmeFoods"). Company, carrier, warehouse and brand names are all fake. The numbers keep realistic distributions so the analysis is meaningful.

---

## Where to start

1. Read **[`test/assessment.md`](test/assessment.md)** — the task, requirements and deliverables.
2. Read **[`dataset/README.md`](dataset/README.md)** — schema of the 5 CSV files, how to join them, and key terms (OTIF, On-Time, In-Full, VFR).
3. Profile the dataset with any tool you like (SQL / Python / Excel / BI tool — your choice).
4. Complete the tasks in `assessment.md`.

---

## Repo structure

```
.
├── README.md                  # This file
├── dataset/
│   ├── README.md              # Schema + business context — READ THIS FIRST
│   ├── shipments.csv          # Fact: delivery orders (OTIF)
│   ├── trips.csv              # Fact: truck trips (VFR)
│   ├── carriers.csv           # Dim: carriers (transport companies)
│   ├── locations.csv          # Dim: warehouses + delivery areas
│   └── products.csv           # Dim: brands + cargo groups
└── test/
    └── assessment.md          # The task
```

---

## Time & submission

- **Suggested time**: about 3-4 hours. This is a guideline, not a strict limit — if you need more time to finish the test, feel free to take it.
- **Deadline**: 48 hours after you receive the test.
- **Submit**: send a Google Drive link (or a zip file by email) with your analysis files (notebook / Excel / PDF / BI export) + at least 1 chart + a short written summary. Email it to the recruiter with the subject `[DA Assessment] <Your name>`. See `test/assessment.md` for full deliverable details.

---

## Questions

If something about the dataset or business context is unclear, write it down in an "Assumptions" section in your submission. We want to see how you handle ambiguity, not whether you guess 100% right.

Have fun :)
