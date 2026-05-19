# 🛒 Project 1: Supermarket Sales Dashboard — Power BI

## Overview
A full end-to-end retail analytics dashboard for a multi-city Indian supermarket chain.
Covers 10 stores across 8 cities, 20 products, 500 customers, and 2 years of sales data (2023–2024).
~68,000 transaction rows generated using realistic patterns.

---

## 📁 Files in This Project

```
project1-supermarket/
├── data/
│   ├── dim_products.csv      — 20 products with category, brand, price
│   ├── dim_stores.csv        — 10 stores with location, region, area
│   ├── dim_customers.csv     — 500 customers with segment and demographics
│   ├── dim_date.csv          — Full date dimension (2023–2024)
│   └── fact_sales.csv        — ~68,000 sales transactions
├── dax/
│   └── all_measures.dax      — All 40+ DAX measures
└── docs/
    └── PROJECT_GUIDE.md      — This file
```

---

## 🧹 STEP 1: Data Cleaning in Power Query

Open Power BI Desktop → Home → Transform Data (Power Query Editor)

### For fact_sales:
1. **Set correct data types:**
   - `Date` → Date
   - `SaleID`, `StoreID`, `ProductID`, `CustomerID` → Whole Number
   - `Revenue`, `COGS`, `GrossProfit`, `FinalUnitPrice`, `ListPrice` → Decimal Number
   - `Quantity`, `DiscountPct` → Whole Number
   - `Promotion`, `Channel` → Text

2. **Check for nulls:** Home → Column Quality. Remove any rows with null in SaleID, Date, StoreID, ProductID.

3. **Add a calculated column - Month-Year (optional):**
   - Add Column → Custom Column:
   ```
   = Date.ToText([Date], "MMM-yyyy")
   ```

4. **Rename the query** to `fact_sales` (right-click in left panel)

### For dim_date:
1. Set `Date` column → Data type: Date
2. Set all number columns (Year, MonthNumber, Day, etc.) → Whole Number

### For all dimension tables:
1. Verify ID columns are Whole Number
2. Verify text columns are Text type
3. Rename queries to match: `dim_products`, `dim_stores`, `dim_customers`, `dim_date`

### Click "Close & Apply" when done.

---

## 🗃️ STEP 2: Data Modelling

Go to **Model View** (left icon bar, looks like three boxes connected).

### Relationships to create:

| From Table         | From Column  | To Table       | To Column   | Cardinality |
|--------------------|--------------|----------------|-------------|-------------|
| fact_sales         | ProductID    | dim_products   | ProductID   | Many-to-One |
| fact_sales         | StoreID      | dim_stores     | StoreID     | Many-to-One |
| fact_sales         | CustomerID   | dim_customers  | CustomerID  | Many-to-One |
| fact_sales         | Date         | dim_date       | Date        | Many-to-One |

**How to create relationships:**
- Drag `ProductID` from `fact_sales` onto `ProductID` in `dim_products`
- Repeat for other relationships
- Double-click any relationship line to verify: Cardinality = Many to One (*:1), Cross filter = Single

### Mark dim_date as Date Table:
- Click `dim_date` table → Table Tools → Mark as Date Table → select `Date` column

### Create a Measures Table:
- Home → Enter Data → Name it `_Measures` → Load
- Move all measures here (right-click measure → Move to table)

---

## 📊 STEP 3: Dashboard Pages

### Page 1 — Executive Summary
**Visuals to add:**
- 4 KPI Cards: Total Revenue | Gross Profit | Gross Profit Margin % | Total Transactions
- Line Chart: Revenue by Month (use dim_date[Date] on X-axis, [Total Revenue] and [Revenue PY] as values)
- Bar Chart: Revenue by Region
- Donut Chart: Channel split (In-Store / Online / Click & Collect)
- Slicer: Year (from dim_date)
- Slicer: Region (from dim_stores)

### Page 2 — Product & Category Analysis
**Visuals to add:**
- Bar Chart (sorted): Top 10 Products by Revenue — use [Product Revenue Rank] to filter ≤ 10
- Treemap: Revenue by Category
- Matrix: Category vs Month with Revenue and Gross Profit Margin %
- Column Chart: Revenue by Brand
- Slicer: Category, Brand

### Page 3 — Store Performance
**Visuals to add:**
- Map Visual: Plot stores using StoreName (latitude/longitude not needed — use location fields)
- Table: Store Name | Revenue | Gross Profit | Revenue Per Sqft | Store Rank
- Bar Chart: Revenue by Store (colored by region)
- KPI: Avg Revenue Per Store

### Page 4 — Customer Insights
**Visuals to add:**
- Pie Chart: Revenue by Customer Segment (Premium/Regular/Budget)
- Stacked Bar: Revenue by AgeGroup and Gender
- KPI Cards: Unique Customers | Revenue Per Customer | Avg Transactions Per Customer
- Column Chart: Transactions by DayType (Weekday/Weekend)

### Page 5 — Promotion & Discount Analysis
**Visuals to add:**
- Bar Chart: Revenue by Promotion Type
- KPI Cards: Total Discount Amount | Discount Rate % | Discount Penetration %
- Scatter Plot: Promotion vs Gross Profit Margin
- Matrix: Promotion Type vs Category — Revenue

---

## 🎨 STEP 4: Formatting Tips

1. **Theme:** Go to View → Themes → Browse for themes → Use a clean dark or corporate theme
2. **Fonts:** Use Segoe UI for body, Segoe UI Bold for headings
3. **Colors:** Pick a brand color (e.g., green #16a34a for a supermarket feel) and use it consistently
4. **KPI Cards:**
   - Turn on "Call out value" for the main number
   - Add "Trend axis" with previous period comparison
5. **Slicers:** Format as dropdown to save space; sync slicers across pages (View → Sync Slicers)
6. **Tooltips:** Add [Gross Profit Margin %] and [Revenue YoY %] to tooltips on all charts

---

## 📐 Data Model Diagram

```
         ┌────────────────┐
         │   dim_date     │
         │ (730 rows)     │
         └───────┬────────┘
                 │ *:1
                 │
┌──────────┐   ┌─┴──────────────┐   ┌──────────────┐
│dim_stores│   │  fact_sales    │   │ dim_products  │
│(10 rows) ├───┤  (~68K rows)   ├───┤  (20 rows)   │
└──────────┘*:1│                │1:*└──────────────┘
               │                │
               └───────┬────────┘
                       │ *:1
               ┌───────┴────────┐
               │ dim_customers  │
               │  (500 rows)    │
               └────────────────┘
```

---

## 💡 Key Business Insights to Highlight

1. **Weekend vs Weekday spike** — use DayType filter on transaction volume
2. **Buy1Get1 promotions** have highest revenue but lowest margin
3. **Snacks and Staples** categories dominate volume
4. **Online channel** growing MoM — compare with same period last year
5. **Premium customers** contribute disproportionate revenue vs their count

---

## 🔗 LinkedIn Post Caption (copy-paste ready)

```
🚀 Power BI Project: Supermarket Sales Dashboard

Built a full retail analytics solution covering:
✅ 2 years | 10 stores | 68K+ transactions
✅ Star schema data model (fact + 4 dims)
✅ 40+ DAX measures — YoY, MTD, Rolling averages
✅ 5-page dashboard: Executive KPIs, Products, Stores, Customers, Promotions

Key skills demonstrated:
📌 Power Query data cleaning & transformation
📌 Dimensional modelling (Star Schema)
📌 Time Intelligence (TOTALYTD, SAMEPERIODLASTYEAR)
📌 Dynamic KPIs with conditional formatting
📌 Cross-page slicer synchronisation

👇 GitHub link in comments

#PowerBI #DataAnalytics #BusinessIntelligence #DataVisualization #PowerQuery #DAX #PortfolioProject #DataScience
```
