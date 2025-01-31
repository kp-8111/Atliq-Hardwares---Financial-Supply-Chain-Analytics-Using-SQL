# Stored Procedures Documentation

This document provides detailed descriptions, parameters, and queries for the stored procedures used in the Atliq Hardwares Analytics project. These procedures are designed to automate repetitive tasks, calculate business insights, and improve decision-making processes.

## 🛠️ Stored Procedures Overview

| Procedure Name | Description |
|----------------|-------------|
| `get_forecast_accuracy` | Calculates forecast accuracy using absolute error for a given fiscal year. |
| `get_market_badge` | Assigns a performance badge (Gold/Silver) to a market based on sales volume for a fiscal year. |
| `get_monthly_gross_sales_for_customer` | Retrieves monthly gross sales for specified customers based on their sold quantity and gross price. |
| `get_top_n_customers_by_net_sales` | Identifies the top N customers by net sales for a given fiscal year and market. |
| `get_top_n_markets_by_net_sales` | Identifies the top N markets by net sales for a given fiscal year. |
| `get_top_n_products_by_net_sales` | Identifies the top N products by net sales for a given fiscal year. |
| `top_n_products_per_division_by_sold_qty` | Retrieves the top N products per division based on sold quantity for a given fiscal year. |

---

## 📝 Stored Procedures Details

### 1. `get_forecast_accuracy`

**Description:**  
This stored procedure calculates the forecast accuracy based on the absolute error for each customer in the specified fiscal year. The accuracy is computed using the difference between forecasted and actual sales quantities. Results are ordered by forecast accuracy.

**Parameters:**  
- **in_fiscal_year (INT):** The fiscal year for which forecast accuracy is calculated.

**Query:**

```sql
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_forecast_accuracy`(
    in_fiscal_year INT
)
BEGIN
    WITH err_table AS (
        SELECT
            s.customer_code,
            SUM(s.sold_quantity) AS total_sold_qty,
            SUM(s.forecast_quantity) AS total_forecast_qty,
            SUM((forecast_quantity - sold_quantity)) AS net_err,
            SUM((forecast_quantity - sold_quantity)) * 100 / SUM(forecast_quantity) AS net_err_pct,
            SUM(ABS(forecast_quantity - sold_quantity)) AS abs_err,
            SUM(ABS(forecast_quantity - sold_quantity)) * 100 / SUM(forecast_quantity) AS abs_err_pct
        FROM gdb0041.fact_act_est s 
        WHERE s.fiscal_year = in_fiscal_year
        GROUP BY customer_code
    )
    SELECT
        e.*,
        c.customer,
        c.market,
        IF(abs_err_pct > 100, 0, 100 - abs_err_pct) AS forecast_accuracy
    FROM err_table e
    JOIN dim_customer c
    USING(customer_code)
    ORDER BY forecast_accuracy DESC;
END
