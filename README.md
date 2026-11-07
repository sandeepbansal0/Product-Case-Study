# Product-Case-Study


# What drives business growth, profitability, and long-term success?

This case study is a **personal project** that analyzes a fictional business from 2022–2026, using **52 database tables and 120 strategic business questions across 15 analytical domains**.

- 📢 **Analysis:** Using SQL, financial analysis, and data visualization, the project explores revenue performance, customer retention, pricing, product profitability, and market opportunities to uncover actionable insights.
- 📢 **Objective:** Transform complex business data into strategic recommendations that help leadership optimize investments, reduce risks, and drive sustainable growth.



### Business Question 1
**What is the total recognized revenue generated between 2022 and 2026?**

```sql
USE product_intelligence_2022_2026_v4;

WITH annual AS (
    SELECT YEAR(CAST(month_start AS DATE)) AS yr,
           SUM(recognized_revenue) AS revenue
    FROM product_unit_economics_monthly
    WHERE CAST(month_start AS DATE) BETWEEN '2022-01-01' AND '2026-12-31'
    GROUP BY yr
)
SELECT COALESCE(CAST(yr AS CHAR), 'TOTAL') AS revenue_year,
       ROUND(SUM(revenue), 2) AS total_revenue
FROM annual
GROUP BY yr WITH ROLLUP;
```


![](https://paper-attachments.dropboxusercontent.com/s_A13B3434EC2F7B53476EAA3038EE14284FECA31A9A8F827AE859B9C0A2F42694_1794036797651_file.png)


