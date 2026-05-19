# 💊 Project 2: Pharmaceutical Sales Dashboard — Power BI

## Overview
An enterprise-grade pharma analytics dashboard tracking drug sales, sales rep performance,
doctor prescriptions, and territory analysis across India.
Covers 25 drugs, 150 doctors, 50 sales reps, 4 channels, 2 years of data.
~58,000 sales records + 1,200 monthly rep target rows.

---

## 📁 Files in This Project

```
project2-pharma/
├── data/
│   ├── dim_drugs.csv             — 25 drugs: therapeutic area, price, Rx/OTC
│   ├── dim_doctors.csv           — 150 doctors with specialization & city
│   ├── dim_sales_reps.csv        — 50 reps with territory and level
│   ├── dim_date.csv              — Full date dimension (2023–2024)
│   ├── fact_pharma_sales.csv     — ~58,000 prescription/sales records
│   └── fact_rep_targets.csv      — Monthly sales targets per rep
├── dax/
│   └── all_measures.dax          — All 50+ DAX measures
└── docs/
    └── PROJECT_GUIDE.md          — This file
```

---

## 🧹 STEP 1: Data Cleaning in Power Query

### For fact_pharma_sales:
1. **Data types:**
   - `Date` → Date
   - `SaleID`, `DrugID`, `DoctorID`, `RepID` → Whole Number
   - `Revenue`, `COGS`, `GrossProfit`, `FinalUnitPrice`, `ListPrice` → Decimal Number
   - `Quantity`, `DiscountPct` → Whole Number
   - `TargetMet` → Whole Number (or Boolean: replace 1→TRUE, 0→FALSE)
   - `Channel` → Text

2. **Add a Profit Flag column (Power Query):**
   - Add Column → Custom Column → Name: `ProfitFlag`
   ```
   = if [GrossProfit] > 0 then "Profitable" else "Loss"
   ```

3. **Validate DiscountPct range:** Filter column, check no values < 0 or > 50

### For fact_rep_targets:
1. `RepID`, `Year`, `Month`, `SalesTarget` → set correct types
2. Add a helper Date column to join with dim_date:
   - Add Column → Custom Column → Name: `TargetDate`
   ```
   = #date([Year], [Month], 1)
   ```
   Set type as Date

### For dim_drugs:
1. All text columns → Text type
2. `DrugID`, `UnitPrice`, `UnitCost` → verify numeric
3. Add a **Margin %** calculated column:
   - Add Column → Custom Column:
   ```
   = ([UnitPrice] - [UnitCost]) / [UnitPrice] * 100
   ```
   Name: `DrugMarginPct`

### For dim_doctors:
- Verify `DoctorID` → Whole Number
- `ExperienceYears` → Whole Number
- Group `ExperienceYears` into brackets:
  - Add Column → Custom Column → Name: `ExperienceBand`
  ```
  = if [ExperienceYears] <= 5 then "Junior (≤5yr)"
    else if [ExperienceYears] <= 15 then "Mid (6-15yr)"
    else "Senior (15+yr)"
  ```

### For dim_sales_reps:
- `ExperienceYears` → Whole Number
- `Level` → Text
- Rename query to `dim_sales_reps`

---

## 🗃️ STEP 2: Data Modelling

Go to **Model View**.

### Relationships to create:

| From Table          | From Column | To Table          | To Column  | Cardinality |
|---------------------|-------------|-------------------|------------|-------------|
| fact_pharma_sales   | DrugID      | dim_drugs         | DrugID     | Many-to-One |
| fact_pharma_sales   | DoctorID    | dim_doctors       | DoctorID   | Many-to-One |
| fact_pharma_sales   | RepID       | dim_sales_reps    | RepID      | Many-to-One |
| fact_pharma_sales   | Date        | dim_date          | Date       | Many-to-One |
| fact_rep_targets    | RepID       | dim_sales_reps    | RepID      | Many-to-One |
| fact_rep_targets    | TargetDate  | dim_date          | Date       | Many-to-One |

> ⚠️ Important: When connecting `fact_rep_targets` to `dim_date`, make sure cross-filter direction is **Single** to avoid ambiguity.

### Mark dim_date as Date Table:
- Click `dim_date` → Table Tools → Mark as Date Table → Column: `Date`

### Create Measures Table:
- Home → Enter Data → Name: `_Measures` → Load
- Right-click each measure → Move to table → `_Measures`

---

## 📊 STEP 3: Dashboard Pages

### Page 1 — Executive KPIs
**Visuals:**
- 5 KPI Cards: Total Revenue | Gross Profit | GP Margin % | Total Prescriptions | Target Achievement %
- Line Chart: Monthly Revenue vs Target (use [Total Revenue] and [Total Target])
- Donut: Rx vs OTC Revenue split
- Clustered Bar: Revenue by Therapeutic Area
- Slicer: Year | Quarter

### Page 2 — Drug Performance
**Visuals:**
- Horizontal Bar (sorted): Top 10 drugs by Revenue — filter [Drug Revenue Rank] ≤ 10
- Treemap: Revenue by Therapeutic Area → DrugName
- Table: DrugName | Revenue | Units Sold | Margin % | Revenue Rank
- Bar Chart: Revenue by Dosage Form (Tablet/Capsule/Inhaler)
- Slicer: TherapeuticArea | PrescriptionType (Rx/OTC)

### Page 3 — Sales Rep Performance
**Visuals:**
- Table: RepCode | Territory | Level | Revenue | Target | Achievement % | Status
- Gauge Chart: Overall Target Achievement %
- Bar Chart: Revenue by Territory
- Scatter Plot: ExperienceYears (X) vs Revenue (Y) — by RepCode
- KPI Cards: Reps Above Target | Reps Below Target | Top Rep Revenue
- Conditional formatting: Color Rep rows Red/Amber/Green based on achievement

### Page 4 — Doctor & Geographic Analysis
**Visuals:**
- Map: Revenue by City (use dim_doctors[City])
- Bar Chart: Top 15 Doctors by Revenue
- Donut: Revenue by Specialization
- Table: DoctorCode | Specialization | City | Revenue | Prescriptions | Revenue Per Prescription
- Treemap: Revenue by Region → State → City
- Slicer: Specialization | Region | ExperienceBand

### Page 5 — Channel & Trend Analysis
**Visuals:**
- 100% Stacked Bar: Channel mix by Quarter
- KPI Cards: Hospital Pharmacy Revenue | Retail Revenue | Online Revenue | Online %
- Line Chart: Rolling 3-Month Revenue trend
- Bar Chart: Revenue by Month with YoY comparison
- Area Chart: Cumulative YTD Revenue 2023 vs 2024

---

## 🎨 STEP 4: Formatting Tips

1. **Color palette:** Use pharmaceutical blues/teals — primary #0ea5e9, secondary #6366f1
2. **Target Achievement:** Use conditional formatting:
   - ≥100% → Green (#22c55e)
   - 80–99% → Amber (#f59e0b)
   - <80% → Red (#ef4444)
3. **Data Labels:** Always show on bar charts for the pharma domain
4. **Tooltips:** Add Manufacturer, TherapeuticArea, and DrugMarginPct to drug visuals
5. **Bookmarks:** Create a bookmark for "Top 10 Drugs" and "All Drugs" to toggle views

---

## 📐 Data Model Diagram

```
         ┌────────────────┐
         │   dim_date     │
         │  (730 rows)    │
         └───────┬────────┘
                 │ *:1 (×2)
          ┌──────┴───────────────────┐
          │                          │
   ┌──────┴──────────┐    ┌──────────┴──────────┐
   │ fact_pharma_    │    │  fact_rep_targets   │
   │ sales (~58K)    │    │   (1,200 rows)      │
   └──┬──────┬───┬───┘    └──────────┬──────────┘
      │      │   │                   │
     *:1    *:1  *:1                *:1
      │      │   │                   │
  ┌───┴──┐ ┌─┴──────┐  ┌────────┐ ┌─┴──────────┐
  │dim_  │ │dim_    │  │dim_    │ │            │
  │drugs │ │doctors │  │sales_  ├─┘            │
  │(25)  │ │(150)   │  │reps(50)│              │
  └──────┘ └────────┘  └────────┘              │
```

---

## 💡 Key Business Insights to Highlight

1. **Cardiac drugs** generate highest revenue — driven by chronic repeat prescriptions
2. **Online Pharmacy** channel growing fastest — highlight YoY % change
3. **Senior reps** (15+ yrs exp) consistently exceed targets — correlate in scatter plot
4. **South region** dominates volume, but **North** has higher average revenue per prescription
5. **OTC drugs** have better margin % than Rx drugs (no price regulation pressure)
6. **Q4 festive season** bump is visible in both years — important for forecasting

---

## 🔗 LinkedIn Post Caption (copy-paste ready)

```
💊 Power BI Project: Pharmaceutical Sales Intelligence Dashboard

Built a comprehensive pharma analytics solution:
✅ 2 years | 25 drugs | 50 sales reps | 150 doctors | 58K+ records
✅ Star schema with TWO fact tables (sales + targets)
✅ 50+ DAX measures — Target tracking, Rolling averages, Rep rankings
✅ 5-page dashboard: KPIs, Drug Performance, Rep Analysis, Geo Insights, Trends

Key skills demonstrated:
📌 Multi-fact-table data modelling
📌 Power Query: custom columns, data type fixes, derived fields
📌 Time Intelligence (Rolling 3M, YTD, SAMEPERIODLASTYEAR)
📌 Target vs Actuals with dynamic conditional formatting
📌 Geographic analysis using Map visual

This is the kind of reporting pharma companies use for territory management, brand performance tracking, and rep incentive planning.

👇 GitHub repo in comments

#PowerBI #Pharma #DataAnalytics #BusinessIntelligence #DAX #PowerQuery #SalesAnalytics #PortfolioProject #DataVisualization
```
