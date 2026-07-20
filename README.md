# 🛍️ Customer Shopping Behavior Analysis

> A data analytics project exploring customer purchasing patterns using **Python**, **SQL**, and **Power BI** — from raw data ingestion to interactive dashboards.

---

## 📌 Project Overview

This project performs an end-to-end analysis of customer shopping behavior using a dataset of **3,900 transactions** across multiple product categories. The goal is to uncover actionable business insights around revenue generation, customer segmentation, discount effectiveness, subscription behavior, and purchase frequency.

The analysis spans three tools in a typical data analytics pipeline:

| Stage | Tool | Purpose |
|---|---|---|
| Data Cleaning & EDA | Python (Pandas) | Load, clean, transform, and enrich the dataset |
| Business Queries | SQL (MySQL) | Answer key business questions with structured queries |
| Visualization | Power BI | Build an interactive dashboard for stakeholders |

---

## 📁 Project Structure

```
customer-trends-data-analysis/
│
├── Customer_Shopping_Behavior_Analysis.ipynb   # Python notebook: EDA + data pipeline
├── customer_behavior_sql_queries.sql            # SQL queries for business analysis
├── customer_behavior_dashboard.pbix             # Power BI dashboard file
├── customer_shopping_behavior.csv               # Raw dataset (3,900 records)
├── Customer Shopping Behavior Analysis.pdf      # Project report / findings
├── Customer-Shopping-Behavior-Analysis.pptx     # Presentation slides
└── Business Problem Document.pdf                # Problem statement & objectives
```

---

## 📊 Dataset Overview

The dataset contains **3,900 customer records** with **18 features** spanning demographics, purchase details, preferences, and behavior.

| Column | Type | Description |
|---|---|---|
| `Customer ID` | int | Unique customer identifier |
| `Age` | int | Age of customer (18–70) |
| `Gender` | object | Male / Female |
| `Item Purchased` | object | Name of the product (25 unique items) |
| `Category` | object | Clothing, Footwear, Outerwear, Accessories |
| `Purchase Amount (USD)` | int | Transaction value ($20–$100) |
| `Location` | object | US state (50 states) |
| `Size` | object | S / M / L / XL |
| `Color` | object | 25 unique colors |
| `Season` | object | Spring / Summer / Fall / Winter |
| `Review Rating` | float | Product rating (2.5–5.0) |
| `Subscription Status` | object | Yes / No |
| `Shipping Type` | object | Standard, Express, Free Shipping, etc. |
| `Discount Applied` | object | Yes / No |
| `Promo Code Used` | object | Yes / No (found to be identical to Discount Applied) |
| `Previous Purchases` | int | Count of prior purchases (1–50) |
| `Payment Method` | object | Credit Card, PayPal, Cash, Venmo, etc. |
| `Frequency of Purchases` | object | Weekly, Fortnightly, Monthly, etc. |

---

## 🐍 Python Analysis — Jupyter Notebook

### Steps Performed

1. **Data Loading** — Loaded the CSV dataset using `pandas`.
2. **Exploratory Data Analysis (EDA)**
   - `.head()` and `.info()` to inspect structure and data types
   - `.describe(include='all')` for summary statistics
3. **Missing Value Handling**
   - `Review Rating` had **37 null values** → imputed with the **median rating per product category** using `groupby().transform()`
4. **Data Cleaning & Renaming**
   - Converted all column names to **snake_case** for consistency
   - Renamed `purchase_amount_(usd)` → `purchase_amount`
5. **Feature Engineering**
   - **`age_group`** — Created using `pd.qcut()` with 4 quartile-based bins: `Young Adult`, `Adult`, `Middle-aged`, `Senior`
   - **`purchase_frequency_days`** — Mapped purchase frequency strings to numeric values (e.g., `Weekly → 7`, `Fortnightly → 14`, `Annually → 365`)
   - **Redundancy Detection** — Confirmed `discount_applied` and `promo_code_used` were 100% identical → dropped `promo_code_used`
6. **Database Export** — Loaded the cleaned DataFrame into MySQL using `SQLAlchemy` and `pymysql`

### Database Connection

The notebook connects to **MySQL** using `pymysql` and `SQLAlchemy`. The cleaned DataFrame is exported directly into a MySQL table named `customer`.

```python
from sqlalchemy import create_engine

username = "root"
password = "your_password"   
host = "localhost"
port = "3306"
database = "customer_behavior"

engine = create_engine(f"mysql+pymysql://{username}:{password}@{host}:{port}/{database}")
df.to_sql("customer", engine, if_exists="replace", index=False)
```

---

## 🗄️ SQL Analysis

All 10 business queries are in [`customer_behavior_sql_queries.sql`](customer_behavior_sql_queries.sql).

### Business Questions Answered

| # | Question | Techniques Used |
|---|---|---|
| Q1 | Total revenue by gender | `GROUP BY`, `SUM` |
| Q2 | Discount users who still spent above average | Subquery, `WHERE`, comparison |
| Q3 | Top 5 products by average review rating | `AVG`, `ORDER BY`, `LIMIT` |
| Q4 | Average purchase by shipping type | `AVG`, `WHERE IN` |
| Q5 | Do subscribers spend more? | `COUNT`, `AVG`, `SUM`, `GROUP BY` |
| Q6 | Products with highest discount usage rates | `CASE WHEN`, percentage calculation |
| Q7 | Customer segmentation (New / Returning / Loyal) | CTE, `CASE WHEN` |
| Q8 | Top 3 products per category | Window function: `ROW_NUMBER() OVER (PARTITION BY ...)` |
| Q9 | Are repeat buyers (>5 purchases) more likely to subscribe? | `WHERE`, `GROUP BY` |
| Q10 | Revenue contribution by age group | `SUM`, `GROUP BY` on engineered feature |

---

## 📈 Power BI Dashboard

The [`customer_behavior_dashboard.pbix`](customer_behavior_dashboard.pbix) file contains an interactive dashboard built on the cleaned dataset.

**To view the dashboard:**
1. Download and install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
2. Open the `.pbix` file

---

## 🚀 Getting Started

### Prerequisites

- Python 3.9+
- Jupyter Notebook or JupyterLab
- MySQL Server (local or remote)
- MySQL Workbench or DBeaver (optional, for running SQL queries)
- Power BI Desktop (for the dashboard)

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/customer-trends-data-analysis.git
cd customer-trends-data-analysis

# Install Python dependencies
pip install pandas sqlalchemy pymysql
```

### Running the Notebook

```bash
jupyter notebook Customer_Shopping_Behavior_Analysis.ipynb
```

> **Note:** Before running the database export cell, update the connection credentials (username, password, database name) to match your local MySQL setup.

### Running SQL Queries

1. Create a database named `customer_behavior` in MySQL Workbench or DBeaver
2. Run the notebook to load the cleaned data into the `customer` table
3. Open `customer_behavior_sql_queries.sql` and execute the queries in MySQL Workbench

---

## 🔍 Key Insights

- **Gender Revenue Gap:** Male customers generate higher total revenue than female customers
- **Discount Behavior:** A significant portion of discount users still spend above the dataset average
- **Subscriber Value:** Subscribed customers show higher average spend compared to non-subscribers
- **Customer Loyalty Segments:** The majority of customers fall in the "Returning" segment (2–10 previous purchases)
- **Top Category:** Clothing is the most purchased category; Blouse is the single most purchased item
- **Shipping Preference:** Free Shipping is the most common shipping type chosen

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)

---


