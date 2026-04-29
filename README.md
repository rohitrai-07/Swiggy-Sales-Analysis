# 🍔 Swiggy Food Delivery — Sales Analysis

> An end-to-end exploratory data analysis project on Swiggy food delivery data,
> built with Python to uncover revenue trends, customer demand patterns, and
> business insights across cities, states, and time periods.

---

## 📌 Table of Contents

- [Project Overview](#project-overview)
- [Objective](#objective)
- [Dataset](#dataset)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Key KPIs](#key-kpis)
- [Analysis & Visualizations](#analysis--visualizations)
- [Key Insights](#key-insights)
- [How to Run](#how-to-run)
- [Connect with Me](#connect-with-me)

---

## Project Overview

This project analyzes a real-world Swiggy food delivery dataset to answer
critical business questions — Which cities generate the most revenue? Which
days see the highest orders? How does revenue split between Veg and Non-Veg
food? What does quarterly performance look like?

The entire analysis is done inside a Jupyter Notebook using Python, covering
data cleaning, KPI computation, time-series analysis, and interactive
visualizations.

---

## Objective

- Compute core business KPIs from raw order data
- Identify revenue trends across months, days of the week, and quarters
- Break down sales by geography (state and city level)
- Classify menu items into Veg vs Non-Veg and compare their revenue contribution
- Deliver actionable insights through clean, interactive charts

---

## Dataset

| Detail | Info |
|---|---|
| File | `swiggy_data.xlsx` |
| Key Columns | `Order Date`, `Price (INR)`, `Rating`, `Rating Count`, `Dish Name`, `City`, `State` |
| Data Type | Food delivery order records |

> **Note:** The dataset is not included in this repository. To run the notebook,
> place your own `swiggy_data.xlsx` file in the same folder and update the file
> path in the notebook.

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Python 3 | Core programming language |
| Pandas | Data manipulation and aggregation |
| NumPy | Numerical operations and conditional classification |
| Matplotlib | Static charts (line, bar) |
| Seaborn | Statistical plotting |
| Plotly Express | Interactive charts (pie, horizontal bar) |
| Jupyter Notebook | Development environment |

---

## Project Structure

```
swiggy-sales-analysis/
│
├── swiggy_sales_analysis.ipynb   ← Main analysis notebook
├── swiggy_data.xlsx              ← Dataset (add your own)
└── README.md                     ← Project documentation
```

---

## Key KPIs

Five core KPIs were calculated to measure overall platform performance:

| KPI | Description |
|---|---|
| **Total Sales (INR)** | Sum of all order values — overall revenue |
| **Average Order Value (AOV)** | Mean spend per order |
| **Average Rating** | Mean customer rating across all orders |
| **Total Rating Count** | Total number of ratings submitted |
| **Total Orders** | Count of all order records in the dataset |

```python
total_sales      = df["Price (INR)"].sum()
avg_order_value  = df["Price (INR)"].mean()
avg_rating       = df["Rating"].mean()
ratings_count    = df["Rating Count"].sum()
total_orders     = len(df)
```

---

## Analysis & Visualizations

### 1. Monthly Sales Trend
Converted `Order Date` to datetime, extracted `YearMonth`, and plotted a
line chart to track how revenue changed month over month.

```python
df["Order Date"] = pd.to_datetime(df["Order Date"])
df["YearMonth"]  = df["Order Date"].dt.to_period("M").astype(str)
monthly_revenue  = df.groupby("YearMonth")["Price (INR)"].sum().reset_index()
```

---

### 2. Daily Sales Trend (Monday – Sunday)
Extracted the day name from the order date and used `.reindex()` to enforce
Monday-to-Sunday ordering, revealing which days drive the highest orders.

```python
df["DayName"]    = df["Order Date"].dt.day_name()
daily_revenue    = df.groupby("DayName")["Price (INR)"].sum().reindex(
    ["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"]
)
```

---

### 3. Veg vs Non-Veg Revenue (Donut Chart)
Used `np.where()` and `str.contains()` with a keyword pattern to classify
every dish as Veg or Non-Veg, then visualized revenue contribution with an
interactive Plotly donut chart.

```python
non_veg_keywords = ["chicken","egg","fish","mutton","prawn","biryani","kebab","kabab"]
pattern          = "|".join(non_veg_keywords)
df["Food Category"] = np.where(
    df["Dish Name"].str.contains(pattern, case=False, na=False), "Non-Veg", "Veg"
)
```

---

### 4. Revenue by State (Horizontal Bar Chart)
Grouped orders by state and sorted by total revenue to identify the
highest-performing states across India.

---

### 5. Quarterly Performance Summary
Extracted the quarter from the order date and aggregated total sales,
average rating, and total orders per quarter using `.agg()`.

```python
df["Quarter"] = df["Order Date"].dt.to_period("Q").astype(str)
quarterly_summary = df.groupby("Quarter").agg(
    Total_Sales  = ("Price (INR)", "sum"),
    Avg_Rating   = ("Rating",      "mean"),
    Total_Orders = ("Order Date",  "count")
)
```

---

### 6. Top 5 Cities by Sales (Horizontal Bar Chart)
Used `.nlargest(5)` to isolate the top-5 revenue-generating cities and
plotted them as a color-highlighted horizontal bar chart using Plotly.

```python
top_5_cities = df.groupby("City")["Price (INR)"].sum().nlargest(5).reset_index()
```

---

## Key Insights

- **Weekend vs Weekday:** Orders peak on weekends, indicating leisure-driven
  food ordering behavior
- **Veg dominates revenue** in most regions, though Non-Veg shows strong
  performance in metro cities
- **Top 5 cities** contribute a disproportionately large share of total
  platform revenue, suggesting high urban concentration
- **Quarterly trends** reveal seasonality — certain quarters consistently
  outperform others, useful for promotional planning
- **State-level analysis** highlights untapped markets where order volumes
  are low but growth potential exists

---

## How to Run

**Step 1 — Clone the repository**
```bash
git clone https://github.com/YOUR-USERNAME/Swiggy-Sales-Analysis.git
cd Swiggy-Sales-Analysis
```

**Step 2 — Install required libraries**
```bash
pip install pandas numpy matplotlib seaborn plotly openpyxl
```

**Step 3 — Add the dataset**

Place your `swiggy_data.xlsx` file in the project folder and update the
file path in the notebook:
```python
df = pd.read_excel("swiggy_data.xlsx")
```

**Step 4 — Open and run the notebook**
```bash
jupyter notebook swiggy_sales_analysis.ipynb
```

Run all cells from top to bottom (`Kernel → Restart & Run All`).

---

## Connect with Me

**Rohit Rai**
- 📧 rohitrai.net07@gmail.com
- 💼 [LinkedIn](https://www.linkedin.com/in/YOUR-LINKEDIN-USERNAME)
- 🐙 [GitHub](https://github.com/YOUR-GITHUB-USERNAME)

---

*If you found this project useful, consider giving it a ⭐ on GitHub!*
