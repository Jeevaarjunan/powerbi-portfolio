<<<<<<< HEAD
# 📊 Power BI Portfolio Projects

> Two end-to-end Power BI projects built from scratch — including synthetic data generation, Power Query cleaning, star schema modelling, DAX measures, and multi-page dashboards.

---

## 🗂️ Projects

| # | Project | Industry | Tables | Records | Pages |
|---|---------|----------|--------|---------|-------|
| 1 | [Supermarket Sales Dashboard](./project1-supermarket/) | Retail | 5 (1 fact, 4 dim) | ~68K | 5 |
| 2 | [Pharmaceutical Sales Dashboard](./project2-pharma/) | Pharma | 6 (2 fact, 4 dim) | ~60K | 5 |

---

## 🛒 Project 1 — Supermarket Sales Dashboard

**Business Context:**
A multi-city Indian supermarket chain with 10 stores wants to track sales performance, product profitability, promotional effectiveness, and customer behaviour.

**Data Model:**
```
fact_sales ──→ dim_products
           ──→ dim_stores
           ──→ dim_customers
           ──→ dim_date
```

**Key Features:**
- Revenue, Gross Profit, Margin % KPIs
- Year-over-Year and Month-over-Month comparisons
- Top products and category analysis
- Store ranking by revenue and revenue per sqft
- Promotion effectiveness analysis (Buy1Get1, Festive, etc.)
- Customer segmentation (Premium / Regular / Budget)
- Channel breakdown (In-Store / Online / Click & Collect)

**DAX Highlights:**
```dax
-- Rolling comparison
Revenue YoY % =
DIVIDE([Total Revenue] - [Revenue PY], [Revenue PY], 0) * 100

-- Category share
Category Revenue Share % =
DIVIDE([Total Revenue],
       CALCULATE([Total Revenue], ALLSELECTED(dim_products[Category])), 0) * 100
```

---

## 💊 Project 2 — Pharmaceutical Sales Dashboard

**Business Context:**
A pharma company tracks drug-wise sales, rep territory performance vs targets, doctor prescription patterns, and channel distribution across India.

**Data Model:**
```
fact_pharma_sales ──→ dim_drugs
                  ──→ dim_doctors
                  ──→ dim_sales_reps
                  ──→ dim_date
fact_rep_targets  ──→ dim_sales_reps
                  ──→ dim_date
```

**Key Features:**
- Two fact tables connected via shared dimension tables
- Sales rep target tracking with dynamic achievement %
- Therapeutic area and drug-level performance
- Doctor segmentation by specialization and experience
- Geographic analysis by city, state, region
- Channel mix: Hospital / Retail / Online pharmacy
- Rolling 3-month and 12-month revenue trends

**DAX Highlights:**
```dax
-- Target achievement
Target Achievement % =
DIVIDE([Total Revenue], [Total Target], 0) * 100

-- Rolling 3 months
Revenue Rolling 3M =
CALCULATE([Total Revenue],
    DATESINPERIOD(dim_date[Date], LASTDATE(dim_date[Date]), -3, MONTH))

-- Dynamic status label
Target Status =
IF([Target Achievement %] >= 100, "✅ On Target",
   IF([Target Achievement %] >= 80, "⚠️ Near Target", "❌ Below Target"))
```

---

## 🛠️ Skills Demonstrated

| Skill | Details |
|-------|---------|
| **Power Query** | Data type fixes, null handling, custom columns, query naming |
| **Data Modelling** | Star schema, multi-fact tables, relationship cardinality |
| **DAX — Basic** | SUM, COUNT, DIVIDE, CALCULATE, FILTER |
| **DAX — Time Intelligence** | TOTALYTD, TOTALMTD, SAMEPERIODLASTYEAR, PREVIOUSMONTH, DATESINPERIOD |
| **DAX — Advanced** | RANKX, TOPN, ALLSELECTED, MAXX, dynamic labels |
| **Visualisations** | KPI cards, line, bar, treemap, scatter, map, donut, gauge, matrix |
| **UX / Design** | Slicer sync, bookmarks, conditional formatting, tooltips |

---

## 📂 How to Use

1. Clone or download this repo
2. Open **Power BI Desktop** (free download from Microsoft)
3. Go to **Home → Get Data → Text/CSV**
4. Load all `.csv` files from the `data/` folder of a project
5. Follow **`docs/PROJECT_GUIDE.md`** for step-by-step setup
6. Copy DAX from `dax/all_measures.dax` into Power BI DAX editor

---

## 👤 About

Built as part of a Power BI learning portfolio to demonstrate real-world data analytics skills.
These projects mirror the kind of dashboards used in actual retail and pharmaceutical companies.

---

*Tools used: Power BI Desktop, Python (data generation), CSV*
=======
# powerbi-portfolio
>>>>>>> 948a48624a5862ecb9eba17880a1a74a2284a488
