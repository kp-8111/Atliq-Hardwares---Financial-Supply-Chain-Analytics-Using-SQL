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
```

### 2. `get_market_badge`
**Description**: This stored procedure assigns a performance badge (Gold/Silver) to a market based on the total sales volume for a given fiscal year.

**Parameters**:
- `in_market` (VARCHAR): The market to evaluate.
- `in_fiscal_year` (YEAR): The fiscal year for evaluation.
- `out_badge` (VARCHAR): Output parameter for the badge (Gold/Silver).

**Query**:
```sql
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_market_badge`(
    IN in_market VARCHAR(45),
    IN in_fiscal_year YEAR,
    OUT out_badge VARCHAR(45)
)
BEGIN
    DECLARE qty INT DEFAULT 0;
    
    -- Set default market
    IF in_market = "" THEN 
        SET in_market = "india";
    END IF;
    
    -- Retrieve total quantity for the given market and fiscal year
    SELECT 
        SUM(s.sold_quantity) INTO qty
    FROM fact_sales_monthly s
    JOIN dim_customer c
    ON c.customer_code = s.customer_code
    WHERE 
        get_fiscal_year(s.date) = in_fiscal_year AND
        c.market = in_market
    GROUP BY market;
        
    -- Determine market badge
    IF qty > 5000000 THEN
        SET out_badge = "Gold";
    ELSE 
        SET out_badge = "Silver";
    END IF;
END
```

## 3. `get_monthly_gross_sales_for_customer`
**Description:**  
Retrieves monthly gross sales for specified customers by calculating total sales value.

**Parameters:**
- `in_customer_codes` (TEXT): Comma-separated customer codes.

**Query:**
```sql
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_monthly_gross_sales_for_customer`(
    in_customer_codes TEXT
)
BEGIN
    SELECT 
        s.date,
        SUM(s.sold_quantity * g.gross_price) AS total_gross_sales
    FROM fact_sales_monthly s 
    JOIN fact_gross_price g 
    ON
        s.product_code = g.product_code AND
        g.fiscal_year = get_fiscal_year(s.date)
    WHERE
        FIND_IN_SET(s.customer_code, in_customer_codes) > 0
    GROUP BY s.date
    ORDER BY s.date;
END
```
# 4. `get_top_n_markets_by_net_sales` Stored Procedure

**Description**:  
This stored procedure identifies the top N markets by net sales for a given fiscal year.

**Parameters**:
- `in_fiscal_year` (INT): The fiscal year for analysis.
- `in_top_n` (INT): The number of top markets to retrieve.

**Query**:
```sql
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_top_n_markets_by_net_sales`(
    in_fiscal_year INT,
    in_top_n INT
)
BEGIN
    SELECT 
        market,
        ROUND(SUM(net_sales) / 1000000, 2) AS net_sales_mln
    FROM gdb0041.net_sales
    WHERE fiscal_year = in_fiscal_year
    GROUP BY market
    ORDER BY net_sales_mln DESC
    LIMIT in_top_n;
END
```
# 5. `get_top_n_products_by_net_sales` Stored Procedure

**Description**:  
This stored procedure identifies the top N products by net sales for a given fiscal year.

**Parameters**:
- `in_fiscal_year` (INT): The fiscal year for analysis.
- `in_top_n` (INT): The number of top products to retrieve.

**Query**:
```sql
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_top_n_products_by_net_sales`(
    in_fiscal_year INT,
    in_top_n INT
)
BEGIN
    SELECT
        product,
        ROUND(SUM(net_sales) / 1000000, 2) AS net_sales_mln
    FROM gdb0041.net_sales
    WHERE fiscal_year = in_fiscal_year
    GROUP BY product 
    ORDER BY net_sales_mln DESC
    LIMIT in_top_n;
END
```
# 6. `top_n_products_per_division_by_sold_qty` Stored Procedure

**Description**:  
This stored procedure retrieves the top N products per division based on sold quantity for a given fiscal year.

**Parameters**:
- `in_fiscal_year` (INT): The fiscal year for analysis.
- `in_top_n` (INT): The number of top products to retrieve per division.

**Query**:
```sql
CREATE DEFINER=`root`@`localhost` PROCEDURE `top_n_products_per_division_by_sold_qty`(
    in_fiscal_year INT,
    in_top_n INT
)
BEGIN
    WITH cte1 AS (
        SELECT 
            p.division,
            p.product,
            SUM(sold_quantity) AS total_qty
        FROM fact_sales_monthly s
        JOIN dim_product p 
        ON
            p.product_code = s.product_code
        WHERE fiscal_year = in_fiscal_year
        GROUP BY p.product
    ),
    cte2 AS (
        SELECT
            *,
            DENSE_RANK() OVER(PARTITION BY division ORDER BY total_qty DESC) AS drnk
        FROM cte1
    )
    SELECT * FROM cte2 WHERE drnk <= in_top_n;
END
```
# 7. `top_n_products_per_division_by_sold_qty` Stored Procedure

**Description**:  
This stored procedure retrieves the top N products per division based on sold quantity for a given fiscal year.

**Parameters**:
- `in_fiscal_year` (INT): The fiscal year for analysis.
- `in_top_n` (INT): The number of top products to retrieve per division.

**Query**:
```sql
CREATE DEFINER=`root`@`localhost` PROCEDURE `top_n_products_per_division_by_sold_qty`(
    in_fiscal_year INT,
    in_top_n INT
)
BEGIN
    WITH cte1 AS (
        SELECT 
            p.division,
            p.product,
            SUM(sold_quantity) AS total_qty
        FROM fact_sales_monthly s
        JOIN dim_product p 
        ON
            p.product_code = s.product_code
        WHERE fiscal_year = in_fiscal_year
        GROUP BY p.product
    ),
    cte2 AS (
        SELECT
            *,
            DENSE_RANK() OVER(PARTITION BY division ORDER BY total_qty DESC) AS drnk
        FROM cte1
    )
    SELECT * FROM cte2 WHERE drnk <= in_top_n;
END
```

