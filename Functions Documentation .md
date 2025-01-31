##  Functions Documentation

This document provides detailed information about the custom functions used for fiscal data calculations. These functions help determine the fiscal year and fiscal quarter based on a given calendar date. They are essential for accurately managing and analyzing fiscal data within the organization. Below are the descriptions, parameters, and SQL definitions for each function.

## Functions Included:
1. `get_fiscal_quarter` – Determines the fiscal quarter from a given date.
2. `get_fiscal_year` – Returns the fiscal year based on a given date.

Each function is designed to facilitate financial reporting, period-based analysis, and forecasting.

---

### 1. `get_fiscal_quarter`

**Description:**  
This function determines the fiscal quarter based on the given calendar date. It categorizes the date into one of the four fiscal quarters (Q1, Q2, Q3, Q4) depending on the month.

**Parameters:**
- **calendar_date (DATE):** The date for which the fiscal quarter is determined.

**Query:**

```sql
CREATE DEFINER=`root`@`localhost` FUNCTION `get_fiscal_quarter`(
    calendar_date DATE
) RETURNS char(2) CHARSET utf8mb4
    DETERMINISTIC
BEGIN
    DECLARE m TINYINT;
    DECLARE qtr CHAR(2);
    SET m = MONTH(calendar_date);
    
    CASE
        WHEN m IN (9, 10, 11) THEN
            SET qtr = "Q1";
        WHEN m IN (12, 1, 2) THEN
            SET qtr = "Q2";
        WHEN m IN (3, 4, 5) THEN
            SET qtr = "Q3";
        ELSE
            SET qtr = "Q4";
    END CASE;
    
    RETURN qtr;
END
```
### 2. `get_fiscal_year`

**Description:**  
This function returns the fiscal year corresponding to the given calendar date. The fiscal year is calculated by adding 4 months to the given date and extracting the year.

**Parameters:**
- **calendar_date (DATE):** The date for which the fiscal year is determined.

**Query:**

```sql
CREATE DEFINER=`root`@`localhost` FUNCTION `get_fiscal_year`(
    calendar_date DATE
) RETURNS INT
    DETERMINISTIC
BEGIN
    DECLARE fiscal_year INT;
    SET fiscal_year = YEAR(DATE_ADD(calendar_date, INTERVAL 4 MONTH));
    RETURN fiscal_year;
END
```
