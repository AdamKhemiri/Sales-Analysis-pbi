# Sales Analysis – Power BI

App-like **Sales Analysis Report** built in Power BI, covering units sold, sales, discounts, costs, and profit across **time, segments, countries, products, and managers**.

The report is designed for non-technical business users: intuitive navigation, clear KPIs, and guided exploration via drillthrough and tooltips.

---
## 🔗 Report Access

- **Live report:** [https://app.powerbi.com/view?r=eyJrIjoiY2YyNjkxMWYtMmEzNS00MTUzLWFhZDctNTQ2MmM4MTI1MzE0IiwidCI6ImIyZDQ1OTA3LTUwOWItNDQ5Ni05YmQzLTVhNjBjN2YwZjM4MCIsImMiOjN9]

---

## 🎯 Business Objectives

This report helps answer questions such as:

- How are **Units Sold, Gross Sales, Net Sales, COGs, and Profit** trending over time?
- Which **customer segments** and **countries** drive most sales and profit?
- Which **products** generate the best volume and margin?
- How does performance differ **by manager**?
- Where do we see **margin pressure**, negative YoY change, or unusually high discounts?

---

## 📊 Key KPIs

Global KPI cards at the top of the main page:

- **Units Sold** – total quantity sold.
- **Gross Sales** – sales before discounts.
- **Discounts** – total discount amount granted.
- **Net Sales** – Gross Sales minus Discounts.
- **Cost of Goods (COGs)** – cost of goods sold.
- **Profit** – Net Sales minus COGs.
- **Profit Margin %** – Profit / Net Sales.
- **Profit YoY %** – year-over-year change in profit.
- **# of Distinct Segments / Countries** – quick sense of portfolio breadth.

All KPIs respond to slicers (date, segment, country, product, manager) and cross-filtering from other visuals.

---

## 🧭 Report Pages & Navigation

The report uses an **app-like layout** with a left navigation bar and page-level buttons.

### 1. Home

- Hero landing page with a **Sales Analysis Report** banner.
- Right-hand text cards summarizing:
  - Units Sold
  - Net Sales
  - Cost of Goods
  - Net Profit
- Social icons at the bottom for a branded, “product-like” feel.

### 2. Sales Analysis (Main Hub)

Primary working area with:

- **Header slicer** (e.g., segment/manager selection).
- **Top KPI cards** for all core metrics.
- **Tabs / buttons** to focus on:
  - Period
  - Segment
  - Country
  - Product

Each view shows:

- 3–4 visuals (Units Sold, Net Sales, Profit, COGs) for that perspective.
- A **detailed table** at the bottom with Units Sold, Gross Sales, Discounts, Net Sales, COGs, Profit, Profit Margin.

Examples:

- **Segment view:** performance by Government, Enterprise, Midmarket, Channel Partners, Small Business.
- **Country view:** Canada, France, USA, Mexico, Germany comparison.
- **Product view:** top products by volume, sales, and margin.

### 3. Manager View (via slicer)

- Top-left “Welcome” card shows the **selected manager’s photo and name**.
- All KPIs and visuals automatically filter to that manager.
- Dynamic greeting using DAX (e.g., “Welcome Bob!”).

This demonstrates using a **manager dimension** and user-friendly personalization without full RLS.

### 4. Help Center

- Dedicated **Help** page with three tiles:
  - **Overview** – what data is in the report.
  - **Navigation** – how to move between pages and use slicers.
  - **How to Use** – examples of business questions to explore.
- Written for non-technical users to reduce the need for training.

### 5. Drillthrough Pages

From visuals on the Sales Analysis page, users can **right-click → Drill through** to detailed pages for:

- Units Sold
- Net Sales
- Net Profit
- Cost of Goods

Each drillthrough page contains row-level detail and extra breakdowns (e.g., by product or manager) to explain anomalies seen in the summary view.

### 6. Tooltip Pages

Custom tooltip pages for key metrics:

- Units Sold – Tooltip  
- Net Sales – Tooltip  
- Net Profit – Tooltip  
- Cost of Goods – Tooltip  

Tooltips are styled to match the main theme and provide **extra context on hover** without leaving the page.

---

## 🧱 Data Model (Star Schema)

The model uses a clean star schema with one fact table, two dimensions, and one utility table.

**Fact table**

- **`FACT_SALES`** – grain: *order line*  
  - Measures and columns for sales, discounts, COGs, profit, discount bands, and YearMonth.
  - Foreign keys to `DIM_DATE` and `DIM_MANAGERS`.

**Dimensions**

- **`DIM_DATE`**
  - Standard calendar attributes: Year, Quarter, Month, Month (Short), YearMonth.
  - **Reporting Period** and **Year Rank** (e.g., 1 = Current Year, 2 = Prior Year) to simplify CY/PY logic.

- **`DIM_MANAGERS`**
  - Manager ID, Manager Name, First Name, Last Name.
  - Territory (region / country responsibility).
  - Image URL used to display manager photos inside the report.

**Utility**

- **`LAST_REFRESHED`**
  - Single column table holding **Date Last Refreshed** for displaying the data refresh timestamp.

👉 For full column-level details, see **`Data_Dictionary.md`** in this repository.

---

## 🧮 DAX Highlights

All measures are stored in a dedicated **`*Measures`** table for cleanliness.

Core measure groups include:

- **Sales & Profit**
  - `M_Units Sold`
  - `M_Gross Sales`
  - `M_Discounts`
  - `M_Net Sales`
  - `M_Cost Of Goods`
  - `M_Profit`
  - `M_Profit Margin` and formatted variants
- **Time Intelligence**
  - `M_Profit CY`, `M_Profit PY`, `M_Profit YOY%`
  - `M_Units Sold CY`, `M_Units Sold PY`
  - Implemented using `DIM_DATE[Year Rank]` instead of hard-coded years.
- **Profile KPIs**
  - `M_# of Distinct Countries`
  - `M_# of Distinct Segments`
- **Cosmetic / UX Helpers**
  - `M_Date/Time Last Refreshed` – text-friendly refresh stamp.
  - `M_Transparent` – used to control visibility/overlay.
  - `M_User Greeting`, `M_User Greeting Help`, `M_Welcome_All` – personalized greeting and background behavior based on selected manager.
  - `M_Discount Band Colors` – returns hex codes based on discount band (High, Medium, Low, None) for consistent color rules.

Example (simplified) YOY logic:

```DAX
M_Profit CY =
CALCULATE ( [M_Profit], DIM_DATE[Year Rank] = "1" )

M_Profit PY =
CALCULATE ( [M_Profit], DIM_DATE[Year Rank] = "2" )

M_Profit YOY% =
IFERROR (
    ([M_Profit CY] - [M_Profit PY]) / ABS ( [M_Profit PY] ),
    1
)
