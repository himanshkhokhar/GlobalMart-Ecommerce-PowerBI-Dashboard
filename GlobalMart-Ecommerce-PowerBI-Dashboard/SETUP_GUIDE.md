# GlobalMart Power BI — Complete Dashboard & Setup Guide

> [!IMPORTANT]
> **THE REPORT IS ALREADY COMPLETELY BUILT!**
> You do NOT need to manually build the visuals, write DAX, or drag-and-drop anything.
> - **Option 1 (Universal .pbix):** Double-click [`GlobalMart_Dashboard.pbix`](GlobalMart_Dashboard.pbix) in Power BI Desktop to open the 5-page populated report with 53 visuals, dark navy theme, and active relationships.
> - **Option 2 (Template .pbit):** Double-click [`GlobalMart_Dashboard.pbit`](GlobalMart_Dashboard.pbit) to load the template into Power BI Desktop.
> - **Option 3 (Project .pbip):** Open [`GlobalMart_Dashboard.pbip`](GlobalMart_Dashboard.pbip) if you have the PBIP preview feature enabled.
>
> The manual sections below are preserved solely for reference, auditing, or custom modifications.

---

## Part 1: Import Data

### 1.1 Open Power BI Desktop
Launch Power BI Desktop. If prompted, skip sign-in for now.

### 1.2 Import All CSV Files

Repeat these steps for each of the 6 CSV files:

1. Click **Home → Get Data → Text/CSV**
2. Browse to `C:\Users\HP\Desktop\powerbi\data\`
3. Select the CSV file and click **Open**
4. In the preview dialog, verify the first row is a header
5. Click **Transform Data** (NOT Load) to open Power Query

### 1.3 Apply Power Query Transformations

In Power Query Editor, for each table:

1. Click **Advanced Editor** (Home tab)
2. Paste the corresponding M script from `powerquery/m_queries.pq`
3. Update the `FolderPath` parameter at the top to your actual data folder path:
   ```
   let FolderPath = "C:\Users\HP\Desktop\powerbi" in FolderPath
   ```
4. Click **Done**, then verify data types are correct in the preview

> **Tip:** Create a query parameter called `FolderPath` (Text type) pointing to your
> data folder. All M queries reference this so you can move the project easily.

### 1.4 Load All Tables
Click **Close & Apply** in Power Query Editor.

---

## Part 2: Build the Data Model

### 2.1 Switch to Model View
Click the **Model** icon in the left sidebar (looks like a diagram).

### 2.2 Create Relationships

Set up these one-to-many relationships:

| From Table      | From Column | To Table   | To Column  | Cardinality |
|-----------------|-------------|------------|------------|-------------|
| DimDate         | DateKey     | FactOrders | DateKey    | 1:M         |
| DimProduct      | ProductKey  | FactOrders | ProductKey | 1:M         |
| DimCustomer     | CustomerKey | FactOrders | CustomerKey| 1:M         |
| DimRegion       | RegionKey   | FactOrders | RegionKey  | 1:M         |
| DimSalesRep     | SalesRepKey | FactOrders | SalesRepKey| 1:M         |

**How to create:** Drag the key column from the dim table onto the fact table column,
or use **Manage Relationships** → **New**.

### 2.3 Mark DimDate as Date Table
1. Click on `DimDate` table
2. Right-click → **Mark as Date Table**
3. Select the `Date` column

### 2.4 Hide Foreign Keys from Report View
Right-click each key column in FactOrders (DateKey, ProductKey, etc.) → **Hide in Report View**

---

## Part 3: Create the Measures Table

### 3.1 Create Empty Measures Table
1. Click **Enter Data** (Home tab)
2. Name the table `_Measures`
3. Delete any columns and click **Load**

### 3.2 Add All Measures
1. Select the `_Measures` table in the Fields pane
2. For each measure in `dax/measures.dax`:
   - Click **New Measure** (Modeling tab)
   - Paste the measure formula
   - Press Enter

> **Shortcut:** You can copy-paste measures in bulk from the `.dax` file.
> Each measure begins with `//` comments explaining the logic.

### 3.3 Create the TopN What-If Parameter
1. Go to **Modeling → New Parameter**
2. Name: `TopN`
3. Data Type: Whole Number
4. Minimum: 3, Maximum: 30, Increment: 1, Default: 10
5. Check **Add slicer to this page**

---

## Part 4: Build Dashboard Pages

### Page 1 — Executive Overview

**Background:** Dark navy (#0D1B2A) — set via Format → Page Background → Custom Color

**KPI Cards (top row):** Add 5 Card visuals:
- Total Revenue → Format: `$#,##0.0,,M`
- Total Profit → Format: `$#,##0.0,,M`
- Profit Margin % → Format: `0.0%`
- Total Orders → Format: `#,##0`
- YoY Growth % → Format: `+0.0%;-0.0%;0.0%`

**Revenue Trend (center):** Line chart
- X-Axis: DimDate[Date] (set hierarchy to Year/Quarter/Month)
- Y-Axis: Total Revenue
- Secondary Y: YoY Growth %
- Enable drill-down arrows

**Category Donut (right):** Donut chart
- Legend: DimProduct[Category]
- Values: Total Revenue
- Colors: Set custom palette (see design system below)

**Top 5 Products:** Bar chart
- Y-Axis: DimProduct[ProductName]
- X-Axis: Total Revenue
- Add visual-level filter: Product Revenue Rank <= 5

**Map Visual:** Azure Maps or Bing Maps
- Location: DimRegion[City]
- Latitude: DimRegion[Latitude]
- Longitude: DimRegion[Longitude]
- Bubble Size: Total Revenue

**Period Comparison KPIs:**
- Revenue PY, YoY Revenue Change, YoY Growth %

**Global Slicers (sync across pages):**
- Year slicer: DimDate[Year] (tile style)
- Category slicer: DimProduct[Category] (dropdown)
- Segment slicer: DimCustomer[Segment] (checkbox list)

**Dynamic Title Card:**
- New text box, value: `Overview Title` measure

---

### Page 2 — Deep-Dive Analysis

**Revenue vs Profit Scatter:**
- X: Total Revenue, Y: Profit Margin %
- Details: DimProduct[ProductName]
- Size: Total Units Sold

**Category Waterfall Chart:**
- Category: DimProduct[Category]
- Y: Total Profit

**Segment Stacked Bar:**
- Y-Axis: DimProduct[Category]
- Legend: DimCustomer[Segment]
- X-Axis: Total Revenue

**Sales Rep Leaderboard Table:**
Columns: RepName, Total Revenue, Profit Margin %, Total Orders, SalesRep Revenue Rank
- Enable conditional formatting on Revenue (data bars)
- Sort by Total Revenue descending

**Top-N Product Bar:**
- Add visual-level filter: Is Top N Product = 1
- This reacts to the TopN slicer parameter

---

### Page 3 — Time-Series & Trends

**Monthly Revenue Area Chart:**
- X: DimDate[YearMonth]
- Y: Total Revenue
- Enable drill-down

**YoY Growth Line:**
- X: DimDate[Year]
- Y: YoY Growth %
- Reference line at 0% for context

**Seasonality Matrix:**
- Matrix visual: Rows = DimDate[MonthName], Columns = DimDate[Year]
- Values: Total Revenue
- Enable conditional formatting (color scale: blue scale)

**Quarter-over-Quarter Comparison:**
- Matrix: Rows = DimDate[Quarter], Columns = DimDate[Year]
- Values: Total Revenue, Revenue PY, YoY Growth %

**Rolling Average Line:**
- Line chart with Revenue 3M Rolling Avg vs Total Revenue

---

### Page 4 — Geographic & Segment Analysis

**Map (City Level):**
- Location field: DimRegion[City]
- Size: Total Revenue
- Tooltips: Total Orders, Profit Margin %

**Customer Segment Matrix:**
- Rows: DimCustomer[Segment]
- Columns: DimProduct[Category]
- Values: Total Revenue
- Conditional formatting enabled

**Acquisition Channel Bar:**
- Y-Axis: DimCustomer[AcquisitionChannel]
- X-Axis: Total Revenue, Total Orders (clustered)

**Cohort Chart (Revenue by Acquisition Year):**
- Legend: DimCustomer[AcquisitionYear]
- X-Axis: DimDate[Year]
- Y-Axis: Total Revenue

---

### Page 5 — Data Explorer

**Main Table (All Columns):**
Columns: OrderID, OrderDate, ShipDate, ProductName, Category,
CustomerName, Segment, City, Country, Quantity, UnitPrice,
Discount, Revenue, Profit, ProfitMarginPct, DaysToShip, ReturnFlag

Settings:
- Enable word wrap on ProductName
- Conditional formatting on ProfitMarginPct: Red (<20%), Yellow (20-35%), Green (>35%)
- Enable search/filter via the visual header

**Reset Filters Button:**
1. Create a blank bookmark with no filters active → name "Reset"
2. Insert → Button → Blank → set Action = Bookmark → Reset
3. Style with text "Reset All Filters" and a refresh icon

**Drill-Through Setup:**
1. On this page, add drill-through fields:
   - DimProduct[ProductName]
   - DimCustomer[CustomerName]
2. Right-click any product in Page 2 → Drill through → Data Explorer

---

## Part 5: Navigation & Polish

### 5.1 Create Navigation Menu
1. Insert → Shapes → Rectangle (sidebar, ~120px wide, full height)
2. Fill: #162032 (darker navy)
3. Add Button for each page using Page Navigation action
4. Use icon + label for each button

### 5.2 Sync Slicers Across Pages
1. Click the Year slicer → View → Sync Slicers
2. Check all pages in the sync panel
3. Repeat for Category and Segment slicers

### 5.3 Set Report Theme
1. View → Themes → Customize current theme
2. Set background colors, font family (Segoe UI or Inter)
3. Set primary accent to #00B4D8 (cyan)

### 5.4 Page Tooltips
1. Create a new page → set as Tooltip page in Page settings
2. Add KPI visuals (Revenue, Profit, Orders) in small format
3. Assign this tooltip page to charts in other pages

---

## Design Color Palette

| Purpose         | Color   | Hex       |
|-----------------|---------|-----------|
| Background      | Navy    | `#0D1B2A` |
| Card Surface    | Dark    | `#162032` |
| Primary Accent  | Cyan    | `#00B4D8` |
| Category 1      | Blue    | `#4361EE` |
| Category 2      | Purple  | `#7209B7` |
| Category 3      | Orange  | `#F77F00` |
| Category 4      | Teal    | `#00B4D8` |
| Category 5      | Green   | `#06D6A0` |
| Positive/Up     | Green   | `#4CAF50` |
| Negative/Down   | Red     | `#F44336` |
| Neutral         | Gray    | `#9E9E9E` |
| Text Primary    | White   | `#FFFFFF` |
| Text Secondary  | Silver  | `#B0BEC5` |

---

## Validation Checklist

Before publishing:
- [ ] All 6 tables loaded and relationships active
- [ ] Total Revenue measure = $38,879,420 (no filters)
- [ ] YoY Growth 2022 = ~22.5% (manual check)
- [ ] Date slicer filters all pages
- [ ] Drill-down works on trend charts (Year → Month)
- [ ] Drill-through from products opens Page 5
- [ ] Reset button clears all filters
- [ ] Map loads and shows city bubbles
- [ ] TopN parameter slicer changes top-N bar chart
- [ ] No broken measures (check for BLANK results under edge filters)
