# Task 2 - Store Sales and Profit Analysis

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ratwet/ShadowFox/blob/Preview/Task2_Store_Sales_Analysis/Store_Sales_Profit_Analysis_FINAL.ipynb)
[![Level](https://img.shields.io/badge/Level-Intermediate-yellow)]()
[![Framework](https://img.shields.io/badge/Plotly-Interactive-3F4F75?logo=plotly&logoColor=white)]()

[← Back to main README](../README.md)

**Objective:** Analyze retail sales and profit data using Python to identify trends, top-performing categories, and customer-segment contributions that can drive strategic business decisions.

> **Result: 8,399 orders analyzed → ≈ $14.92M in sales, ≈ $1.52M in profit (≈ 10.2% blended margin)**

---

## Table of Contents
- [Problem Statement](#problem-statement)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Key Findings](#key-findings)
- [Visualizations Produced](#visualizations-produced)
- [How to Run](#how-to-run)
- [Tech Stack](#tech-stack)

---

## Problem Statement

Retail businesses generate large volumes of transactional data but often struggle to extract actionable insights. This task loads, cleans, and analyzes a superstore dataset to uncover what drives sales and profitability across product categories, customer segments, and regions.

---

## Dataset

**Public Superstore Sales Dataset** - retail transactional data.

| Property | Value |
|---|---|
| Total records | 8,399 orders |
| Total columns | 21 |
| Key columns | Order Date, Sales, Profit, Discount, Category, Sub-Category, Customer Segment, Region |
| Other columns | Order Priority, Order Quantity, Ship Mode, Unit Price, Shipping Cost, Product Container, Product Base Margin |
| Date range | 2010 – 2013 |
| Source | Publicly available superstore dataset |

---

## Methodology

### 1. Data Preparation
- Converted Order Date and Ship Date columns to datetime format to enable time-series analysis
- Extracted Year, Month, and YearMonth features for temporal grouping
- **Product Base Margin** had 63 missing values (0.75% of rows) - imputed using the category-level median, bringing missing values to **0**

### 2. Outlier Detection - IQR Method
Identified outliers using the Interquartile Range (IQR) technique:

| Column | Lower Bound | Upper Bound | Outliers Found |
|---|---|---|---|
| Sales | -2205.99 | 4058.51 | 1,042 (12.4%) |
| Profit | -452.41 | 531.85 | 1,704 (20.3%) |

Outliers were retained in the dataset, as they represent legitimate high-value bulk orders relevant to business analysis.

### 3. Sales Analysis
- Monthly sales trends to identify seasonal patterns
- Category and sub-category performance using bar charts and treemaps
- Top and bottom performers identified for inventory prioritization

### 4. Profit Analysis
- Sub-category profit ranking to identify most and least profitable products
- Customer segment profitability to guide marketing strategy

### 5. Operational Insights
- Profit margin percentage calculated per category and segment
- Regional performance comparison across all four regions

---

## Key Findings

### Customer Segment Performance
*(sorted by profit margin - verified against notebook output)*

| Customer Segment | Total Sales | Total Profit | Profit Margin % |
|---|---|---|---|
| **Small Business** | 2,788,321 | 315,708.01 | **11.32%** |
| Corporate | 5,498,905 | 599,746.00 | 10.91% |
| Consumer | 3,063,611 | 287,959.94 | 9.40% |
| Home Office | 3,564,764 | 318,354.03 | 8.93% |
| **Total (all segments)** | **14,915,601** | **1,521,767.98** | **10.20%** |

> **Insight:** Small Business has the highest profit margin (11.32%) despite the *lowest* total sales of any segment - making it the most efficient segment per dollar of revenue. Corporate drives the largest absolute profit ($599,746) purely on volume, but a marketing dollar aimed at growing Small Business would likely convert to profit more efficiently than one aimed at Corporate.

---

## Visualizations Produced

- Monthly sales trend line chart (time-series)
- Category sales and profit grouped bar chart
- Sub-category sales treemap
- Sub-category profit horizontal bar chart (red-yellow-green scale)
- Customer segment sales pie chart (donut style)
- Customer segment profit bar chart
- Profit margin percentage bar chart by category
- Regional sales and profit grouped bar chart

All 8 charts are produced using Plotly for interactive exploration directly inside the notebook.

---

## How to Run

1. Open `Store_Sales_Profit_Analysis_FINAL.ipynb` in Google Colab
2. No GPU required - runs on CPU
3. The dataset loads automatically from a public URL - no manual download needed
4. Run all cells: Runtime → Run all

---

## Tech Stack

- Python 3
- Pandas
- NumPy
- Plotly Express and Plotly Graph Objects
- IPython Display
