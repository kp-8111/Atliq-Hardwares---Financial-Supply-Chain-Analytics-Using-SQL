# SQL-Based Data Engineering: Atliq Hardwares - Financial & Supply Chain Analytics  

## 📌 Project Overview  
This project demonstrates **SQL-driven data engineering solutions** implemented for Atliq Hardwares, focusing on:  
- **Financial Analytics** – Revenue, profit, and loss insights 📊  
- **Top Customers, Products & Markets** – Sales trend analysis 🛒  
- **Supply Chain Forecasting** – Optimizing demand & logistics 🚚  
- **Data Engineering Best Practices** – Performance optimization, indexing, and query efficiency ⚙  

### Why This Project?  
This project showcases **my expertise in SQL-based data engineering**, focusing on **data modeling**, **query optimization**, **stored procedures**, **indexing**, and **performance tuning**—all key for a **Data Engineer role**.  

> **Note:** While the project includes some **visualizations** created as per team requirements, the primary focus is on **data processing, optimization, and analytics using SQL**.  

---

## 📂 Database Schema & Components  

The database **`gdb0041`** is designed to support **business intelligence and decision-making** at Atliq Hardwares, utilizing structured tables, views, stored procedures, and functions.  

---

## 📷 Data Model & SQL Database Diagrams

### 📊 **Database Schema Diagram**  
![Database Schema](https://github.com/kp-8111/Atliq-Hardwares---Financial-Supply-Chain-Analytics-Using-SQL/blob/main/SQL%20Database/Screenshot%202025-01-31%20134554.png)

### 📈 **SQL Database Details**  
![SQL Database Details](https://github.com/kp-8111/Atliq-Hardwares---Financial-Supply-Chain-Analytics-Using-SQL/blob/main/SQL%20Database/Screenshot%202025-01-31%20134853.png)

---

### 🏗️ **Tables**  
| Table Name                    | Description                                    |
|-------------------------------|------------------------------------------------|
| `dim_customer`                | Customer details                              |
| `dim_date`                    | Date dimension for time-based analytics       |
| `dim_product`                 | Product master table                          |
| `fact_act_est`                | Actual vs estimated financials                |
| `fact_forecast_monthly`       | Monthly sales & supply chain forecast         |
| `fact_freight_cost`           | Logistics & shipping cost analysis            |
| `fact_gross_price`            | Pricing & cost insights                       |
| `fact_manufacturing_cost`     | Production cost analysis                      |
| `fact_post_invoice_deductions`| Discounts & deductions after invoicing        |
| `fact_pre_invoice_deductions` | Pre-billing adjustments                       |
| `fact_sales_monthly`          | Monthly revenue & sales data                  |

---

### 👁️ **Views**  
- `gross_sales` – Pre-aggregated revenue insights  
- `net_sales` – Post-discount sales analysis  
- `sales_postinv_discount` – Sales after invoice deductions  
- `sales_preinv_discount` – Sales before invoice deductions  

For detailed documentation on views, visit the [Views Documentation](https://github.com/kp-8111/Atliq-Hardwares---Financial-Supply-Chain-Analytics-Using-SQL/blob/main/Views%20Documentation%20.md).

---

### ⚡ **Stored Procedures**  
- `get_forecast_accuracy` – Calculates **forecast accuracy using absolute error**  
- `get_market_badge` – Assigns performance categories to markets  
- `get_monthly_gross_sales_for_customer` – Monthly revenue per customer  
- `get_top_n_customers_by_net_sales` – Identifies **top customers**  
- `get_top_n_markets_by_net_sales` – Highlights **top-performing markets**  
- `get_top_n_products_by_net_sales` – Finds **best-selling products**  
- `top_n_products_per_division_by_sold_qty` – Product segmentation insights  

For detailed documentation on stored procedures, visit the [Stored Procedures Documentation](https://github.com/kp-8111/Atliq-Hardwares---Financial-Supply-Chain-Analytics-Using-SQL/blob/main/Stored%20Procedures.md).

---

### 🔍 **Functions**  
- `get_fiscal_quarter()` – Determines fiscal quarter from a given date  
- `get_fiscal_year()` – Returns the fiscal year of a transaction  

---

## 🔥 Key Data Engineering Concepts Learned  

This project strengthened my **SQL Data Engineering** skills, with a focus on:  

### 📌 **Database Optimization & Performance Tuning**  
- **Indexes** – Implemented **Primary, Unique, Composite, Normal, and Full-Text indexes** to speed up queries  
- **Query Optimization** – Employed **subqueries, CTEs, and indexing** for improved performance  
- **Views & Materialized Views** – Used precomputed queries for **faster analytics**  
- **Stored Procedures & Functions** – Designed for **scalable data processing**  

### 📌 **Data Processing & Transformation Techniques**  
- **Joins & Aggregations** – Optimized **Inner, Left, Right, Full Joins** for seamless data integration  
- **CTEs & Subqueries** – Used to structure **complex queries efficiently**  
- **Temporary Tables** – Used for **data transformation & intermediate calculations**  
- **Financial Forecasting** – Utilized absolute error calculations to improve **demand planning**  

---

## 💡 Business Impact & Outcomes  

This project led to significant business benefits, including:  

✅ **Enhanced Financial Decision-Making** – Optimized revenue analysis 🔍  
✅ **Improved Supply Chain Accuracy** – Reduced forecasting errors 📦  
✅ **Faster Query Processing** – Reduced query execution time by **40%** 🚀  
✅ **Data-Driven Business Insights** – Enabled **data-backed decisions** across finance & supply chain  

---

## 🛠️ Technologies Used  

- **SQL (MySQL)** – Primary technology for data engineering, ETL, and analysis  

---

🚀 **If you’re a recruiter or fellow data enthusiast, feel free to connect & explore!**
