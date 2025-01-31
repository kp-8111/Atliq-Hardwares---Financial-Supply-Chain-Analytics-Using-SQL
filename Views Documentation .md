#  Views Documentation

This repository contains a set of SQL views designed to facilitate sales data analysis. These views are part of a database schema that processes data from multiple tables related to sales transactions, customer information, and product details. The views are created to support business intelligence and data analysis efforts, allowing users to easily analyze gross sales, discounts, and net sales over different fiscal periods.

## Overview of Views

The following views are included in this repository:

1. **`gross_sales`**: This view calculates the total gross sales for each transaction, considering the quantity sold and the gross price of each item.
2. **`sales_preinv_discount`**: This view includes the pre-invoice discount percentage for each sales transaction and calculates the gross sales total with these discounts applied.
3. **`sales_postinv_discount`**: This view calculates post-invoice discounts and derives the net invoice sales by applying these discounts to the gross sales total.
4. **`net_sales`**: The final view in this chain calculates the net sales by applying both pre- and post-invoice discounts to the gross sales total.

Each view builds on the previous one, allowing for a step-by-step breakdown of the sales data from gross sales to the final net sales after discounts. These views are critical for tracking revenue, customer spending behavior, and product performance.

## Use Case

These views are ideal for businesses seeking to gain insights into their sales performance over time, assess the impact of discounts on profitability, and make data-driven decisions to optimize sales strategies.

# 1. `gross_sales` View

This SQL view calculates the total gross sales for each transaction by multiplying the sold quantity with the gross price per item.

```sql
CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
VIEW `gross_sales` AS
    SELECT 
        `s`.`date` AS `date`,
        `s`.`fiscal_year` AS `fiscal_year`,
        `s`.`customer_code` AS `customer_code`,
        `c`.`customer` AS `customer`,
        `c`.`market` AS `market`,
        `s`.`product_code` AS `product_code`,
        `po`.`product` AS `product`,
        `po`.`variant` AS `variant`,
        `s`.`sold_quantity` AS `sold_quantity`,
        `g`.`gross_price` AS `gross_price_per_item`,
        (`s`.`sold_quantity` * `g`.`gross_price`) AS `gross_price_total`
    FROM
        (((`fact_sales_monthly` `s`
        JOIN `dim_customer` `c` ON ((`c`.`customer_code` = `s`.`customer_code`)))
        JOIN `dim_product` `po` ON ((`po`.`product_code` = `s`.`product_code`)))
        JOIN `fact_gross_price` `g` ON (((`g`.`product_code` = `s`.`product_code`)
            AND (`g`.`fiscal_year` = `s`.`fiscal_year`))))
```
# 2. `sales_preinv_discount` View

This SQL view calculates the gross sales total with pre-invoice discounts applied. It also provides the pre-invoice discount percentage for each sales transaction.

```sql
CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
VIEW `sales_preinv_discount` AS
    SELECT 
        `s`.`date` AS `date`,
        `s`.`fiscal_year` AS `fiscal_year`,
        `s`.`customer_code` AS `customer_code`,
        `c`.`market` AS `market`,
        `s`.`product_code` AS `product_code`,
        `p`.`product` AS `product`,
        `p`.`variant` AS `variant`,
        `s`.`sold_quantity` AS `sold_quantity`,
        `g`.`gross_price` AS `gross_price_per_item`,
        (`s`.`sold_quantity` * `g`.`gross_price`) AS `gross_price_total`,
        `pre`.`pre_invoice_discount_pct` AS `pre_invoice_discount_pct`
    FROM
        ((((`fact_sales_monthly` `s`
        JOIN `dim_customer` `c` ON ((`s`.`customer_code` = `c`.`customer_code`)))
        JOIN `dim_product` `p` ON ((`p`.`product_code` = `s`.`product_code`)))
        JOIN `fact_gross_price` `g` ON (((`g`.`product_code` = `s`.`product_code`)
            AND (`g`.`fiscal_year` = `s`.`fiscal_year`))))
        JOIN `fact_pre_invoice_deductions` `pre` ON (((`s`.`customer_code` = `pre`.`customer_code`)
            AND (`pre`.`fiscal_year` = `s`.`fiscal_year`))))
```
# 3. `sales_postinv_discount` View

This SQL view calculates post-invoice discounts and derives the net invoice sales by applying these discounts to the gross sales total.

```sql
CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
VIEW `sales_postinv_discount` AS
    SELECT 
        `s`.`date` AS `date`,
        `s`.`fiscal_year` AS `fiscal_year`,
        `s`.`customer_code` AS `customer_code`,
        `s`.`market` AS `market`,
        `s`.`product_code` AS `product_code`,
        `s`.`product` AS `product`,
        `s`.`variant` AS `variant`,
        `s`.`sold_quantity` AS `sold_quantity`,
        `s`.`gross_price_total` AS `gross_price_total`,
        `s`.`pre_invoice_discount_pct` AS `pre_invoice_discount_pct`,
        (`s`.`gross_price_total` - (`s`.`pre_invoice_discount_pct` * `s`.`gross_price_total`)) AS `net_invoice_sales`,
        (`po`.`discounts_pct` + `po`.`other_deductions_pct`) AS `post_invoice_discount_pct`
    FROM
        (`sales_preinv_discount` `s`
        JOIN `fact_post_invoice_deductions` `po` ON (((`s`.`product_code` = `po`.`product_code`)
            AND (`s`.`customer_code` = `po`.`customer_code`)
            AND (`s`.`date` = `po`.`date`))))
```
# 4. `net_sales` View

This SQL view calculates the final net sales by applying both pre- and post-invoice discounts to the gross sales total.

```sql
CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
VIEW `net_sales` AS
    SELECT 
        `sales_postinv_discount`.`date` AS `date`,
        `sales_postinv_discount`.`fiscal_year` AS `fiscal_year`,
        `sales_postinv_discount`.`customer_code` AS `customer_code`,
        `sales_postinv_discount`.`market` AS `market`,
        `sales_postinv_discount`.`product_code` AS `product_code`,
        `sales_postinv_discount`.`product` AS `product`,
        `sales_postinv_discount`.`variant` AS `variant`,
        `sales_postinv_discount`.`sold_quantity` AS `sold_quantity`,
        `sales_postinv_discount`.`gross_price_total` AS `gross_price_total`,
        `sales_postinv_discount`.`pre_invoice_discount_pct` AS `pre_invoice_discount_pct`,
        `sales_postinv_discount`.`net_invoice_sales` AS `net_invoice_sales`,
        `sales_postinv_discount`.`post_invoice_discount_pct` AS `post_invoice_discount_pct`,
        (`sales_postinv_discount`.`net_invoice_sales` * (1 - `sales_postinv_discount`.`post_invoice_discount_pct`)) AS `net_sales`
    FROM
        `sales_postinv_discount`
```
