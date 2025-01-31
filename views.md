# SQL Views for Sales Data Analysis

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
