# 📊 Atliq Hardwares - Financial & Supply Chain Analytics 🚀  

## 📌 Project Overview  
This project focuses on **financial analytics, top customers, products, markets, and supply chain analysis** for Atliq Hardwares. The goal is to **optimize revenue, identify key business drivers, and enhance supply chain efficiency** by leveraging **SQL-based data engineering techniques**.  

### 🔍 Key Analyses  
- ✅ **Financial Performance** - Profit & Loss analysis 📈  
- ✅ **Top N Customers & Products** - Revenue & sales trends 🛒  
- ✅ **Market Performance** - Regional growth insights 🌍  
- ✅ **Supply Chain Forecasting** - Absolute Error calculations 📦  

---

## 📂 Database Schema & Components  

The database **`gdb0041`** contains multiple **tables, views, stored procedures, and functions**, supporting **business intelligence and analytics** for Atliq Hardwares.  

### 🏗️ **Tables**  
| Table Name                  | Description                                  |
|-----------------------------|----------------------------------------------|
| `dim_customer`              | Customer details                            |
| `dim_date`                  | Date & time-based analytics                 |
| `dim_product`               | Product master table                        |
| `fact_act_est`              | Actual vs Estimated financials              |
| `fact_forecast_monthly`     | Monthly sales & supply chain forecast       |
| `fact_freight_cost`         | Shipping & logistics costs                  |
| `fact_gross_price`          | Pricing insights                            |
| `fact_manufacturing_cost`   | Cost analysis                               |
| `fact_post_invoice_deductions` | Discounts & deductions after invoicing  |
| `fact_pre_invoice_deductions`  | Pre-billing adjustments               |
| `fact_sales_monthly`        | Monthly revenue & sales data                |

### 👁️ **Views**  
- `gross_sales` - Aggregate revenue insights  
- `net_sales` - Post-discount sales analysis  
- `sales_postinv_discount` - Sales after invoice deductions  
- `sales_preinv_discount` - Sales before invoice deductions  

### ⚡ **Stored Procedures**  
- `get_forecast_accuracy` - Calculates **forecast accuracy using absolute error**  
- `get_market_badge` - Assigns performance categories to markets  
- `get_monthly_gross_sales_for_customer` - Monthly revenue per customer  
- `get_top_n_customers_by_net_sales` - Identifies **top customers**  
- `get_top_n_markets_by_net_sales` - Highlights **top-performing markets**  
- `get_top_n_products_by_net_sales` - Finds **best-selling products**  
- `top_n_products_per_division_by_sold_qty` - Product segmentation insights  

### 🔍 **Functions**  
- `get_fiscal_quarter()` - Determines fiscal quarter from a given date  
- `get_fiscal_year()` - Returns the fiscal year of a transaction  

---

## 💡 Learnings & SQL Techniques  

Throughout this project, I mastered several **SQL data engineering concepts**, including:  

- ✅ **Views** - Precomputed queries for faster data retrieval  
- ✅ **Stored Procedures** - Automating complex queries for financial analysis  
- ✅ **Functions** - Custom functions for business logic (fiscal calculations)  
- ✅ **CTEs & Subqueries** - Optimized data retrieval for in-depth insights  
- ✅ **Temporary Tables** - Used for **data transformation & intermediate calculations**  
- ✅ **Indexes** - **Unique, Primary, Composite, Normal, Full-text** indexes to improve query performance  
- ✅ **Joins** - **Inner, Left, Right, Full** joins to combine datasets efficiently  

### 📌 **Performance Optimization:**  
- ✅ Implemented **indexes** to **reduce query execution time**  
- ✅ Used **CTEs & Views** to enhance query readability & maintainability  
- ✅ Optimized stored procedures for **faster financial analysis**  

---

## 🔥 Key Business Impact  

📊 **Revenue Growth Insights**  
📈 **Top Markets, Customers & Product Performance**  
📉 **Optimized Forecast Accuracy for Supply Chain**  
💰 **Enhanced Financial Decision-Making with SQL-Powered Dashboards**  

---

## 🛠️ Technologies Used  

- **SQL (MySQL)**  
- **Power BI (for dashboarding)**  
- **Python (for data manipulation where needed)**  

---

## 🔗 Explore the Full Project  

📌 **Check it out on NovyPro:** [Click Here](https://project.novypro.com/6Ts4ch)  

---

🚀 **Feel free to explore, fork, and contribute!**  
