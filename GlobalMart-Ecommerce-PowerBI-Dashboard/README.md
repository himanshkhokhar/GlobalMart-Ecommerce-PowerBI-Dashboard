# GlobalMart Retail Analytics — Power BI Dashboard

![GlobalMart Dashboard](screenshots/overview_preview.png)

## Overview

A **premium, 5-page Power BI analytics dashboard** built on a synthetic GlobalMart
multi-channel retail dataset (50,000 orders, 2021–2024). Designed to demonstrate
professional-grade BI development skills for a portfolio.

| Metric           | Value               |
|------------------|---------------------|
| Total Orders     | 50,000              |
| Total Revenue    | ~$38.9 M            |
| Profit Margin    | 36.6%               |
| Date Range       | Jan 2021 – Dec 2024 |
| Products         | 62 across 5 categories |
| Customers        | 3,500               |
| Cities           | 25 across 4 countries |

---

## Project Structure

```
powerbi/
├── data/                        # CSV source files
│   ├── fact_orders.csv          # 50,000 order records (fact table)
│   ├── dim_date.csv             # Date dimension (1,461 rows)
│   ├── dim_product.csv          # Product dimension (62 products)
│   ├── dim_customer.csv         # Customer dimension (3,500 customers)
│   ├── dim_region.csv           # Region/geo dimension (25 cities)
│   └── dim_salesrep.csv         # Sales rep dimension (15 reps)
├── dax/
│   └── measures.dax             # All DAX measure definitions (annotated)
├── powerquery/
│   └── m_queries.pq             # Power Query M scripts for all tables
├── docs/
│   ├── DATA_DICTIONARY.md       # Column definitions for every table
│   ├── DATA_MODEL.md            # Relationship diagram + schema docs
│   └── INSIGHTS.md              # Key analytical findings
├── screenshots/                 # Dashboard page screenshots
├── generate_dataset.py          # Original dataset generation script
├── generate_facts.py            # Fast vectorized fact table generator
├── validate_data.py             # Data quality validation script
├── README.md                    # This file
└── SETUP_GUIDE.md               # Step-by-step Power BI setup
```

---

## Quick Start

You can open the report immediately with all 5 pages and 53 visuals pre-configured:

### Option 1: Open the Power BI Document (.pbix) — Universal & Recommended
1. Double-click **`GlobalMart_Dashboard.pbix`** in this folder.
2. Microsoft Power BI Desktop opens directly with all 5 report pages fully rendered in the premium Dark Navy theme.
3. All 53 visuals, KPI cards, trend charts, donut charts, and detail tables are pre-loaded.

### Option 2: Open the Power BI Template (.pbit)
1. Double-click **`GlobalMart_Dashboard.pbit`**.
2. Power BI Desktop loads the template and prompts you to refresh the data model.
3. Click **Load / Refresh** — all tables, measures, relationships, and visual pages load automatically from the `data/` folder.

### Option 3: Open the Power BI Project (.pbip)
1. Double-click **`GlobalMart_Dashboard.pbip`**.
2. Requires "Power BI Project (.pbip) save option" enabled under Power BI Desktop Options → Preview Features.

---

## Deliverables & Architecture

```
powerbi/
├── GlobalMart_Dashboard.pbip          # Power BI Project Launcher (Modern PBIP format)
├── GlobalMart_Dashboard.Report/       # Native Report Definition
│   ├── definition.pbir                # Model pointer
│   └── report.json                    # Full 5-page visual definitions (53 visuals)
├── GlobalMart_Dashboard.SemanticModel/ # Star Schema Data Model
│   ├── definition.pbism               # Model metadata
│   └── model.bim                      # TMSL Model: 6 tables, 40+ measures, 5 relationships, M queries
├── GlobalMart_Dashboard.pbit          # Power BI Template ZIP Archive (all pages & visuals)
├── data/                              # CSV source files
│   ├── fact_orders.csv                # 50,000 order records (fact table)
│   ├── dim_date.csv                   # Date dimension (1,461 rows)
│   ├── dim_product.csv                # Product dimension (62 products)
│   ├── dim_customer.csv               # Customer dimension (3,500 customers)
│   ├── dim_region.csv                 # Region/geo dimension (25 cities)
│   └── dim_salesrep.csv               # Sales rep dimension (15 reps)
├── dax/
│   └── measures.dax                   # All DAX measure definitions (annotated)
├── powerquery/
│   └── m_queries.pq                   # Power Query M scripts for all tables
├── docs/
│   ├── DATA_DICTIONARY.md             # Column definitions for every table
│   ├── DATA_MODEL.md                  # Relationship diagram + schema docs
│   └── INSIGHTS.md                    # Key analytical findings
├── screenshots/                       # Dashboard page screenshots
├── generate_dataset.py                # Original dataset generation script
├── generate_facts.py                  # Fast vectorized fact table generator
├── generate_powerbi_full_project.py   # Full PBIP & PBIT report generator script
├── validate_data.py                   # Data quality validation script
├── README.md                          # This file
└── SETUP_GUIDE.md                     # Step-by-step Power BI setup & customization guide
```

---

## Dashboard Pages

| Page | Name | Purpose |
|------|------|---------|
| 1 | Executive Overview | KPIs, trends, top performers |
| 2 | Deep-Dive Analysis | Product/segment breakdowns, rankings |
| 3 | Time-Series & Trends | Monthly/quarterly trends, YoY, seasonality |
| 4 | Geographic & Segment | Map, customer segmentation, channel mix |
| 5 | Data Explorer | Searchable order table, drill-through |

---

## Key Interactions to Try

1. **Global Year Slicer** — Filter all pages to a single year
2. **Category Filter** — Drill into Technology, Furniture, etc.
3. **Drill Down on Charts** — Year → Quarter → Month in trend charts
4. **Drill-Through** — Right-click a product or customer → "See Details"
5. **Top-N Parameter** — Adjust the slider to show Top 5/10/20 products
6. **Reset Filters Button** — Click to clear all active slicers
7. **Navigation Menu** — Click page buttons in the left nav panel
8. **Tooltip Hover** — Hover over data points for rich tooltips
9. **Cross-Filter** — Click a category segment to filter all related visuals

---

## Verified Analytical Findings

1. **Revenue grew 24% YoY in 2024** ($10.6M → $13.2M), driven by Technology
2. **Technology drives 64.7%** of total revenue despite being only 1 of 5 categories
3. **Q4 consistently peaks** — November alone represents ~12% of annual orders
4. **Corporate segment** generates the highest revenue ($17.6M, 45.2%) due to bulk orders
5. **Profit margin is stable** at 36–37% across all years, indicating good cost discipline
6. **Return rate is 3.5%** — highest in Appliances (pricing expectations mismatch)

---

## Dataset Notes

This is a **synthetic dataset** generated to demonstrate realistic retail patterns.
It does not represent any real company's data. All names, orders, and financials
are procedurally generated with controlled random seeds for reproducibility.

**Seed**: 42 — re-running `generate_dataset.py` produces identical output.

---

## License

This project and its synthetic dataset are released under the **MIT License**.
Free to use for portfolio, educational, and demonstration purposes.
