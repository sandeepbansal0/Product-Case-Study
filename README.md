# Product-Case-Study


# What drives business growth, profitability, and long-term success?

This case study is a **personal project** that analyzes a fictional business from 2022–2026, using **52 database tables and 120 strategic business questions across 15 analytical domains**.

- 📢 **Analysis:** Using SQL, financial analysis, and data visualization, the project explores revenue performance, customer retention, pricing, product profitability, and market opportunities to uncover actionable insights.
- 📢 **Objective:** Transform complex business data into strategic recommendations that help leadership optimize investments, reduce risks, and drive sustainable growth.



# Business Performance Questions

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
### Results


![](https://paper-attachments.dropboxusercontent.com/s_A13B3434EC2F7B53476EAA3038EE14284FECA31A9A8F827AE859B9C0A2F42694_1794036797651_file.png)



# 


## 2. How concentrated is revenue among the top five products?

*This query identifies the top five revenue-generating products, calculates each product's contribution to total company revenue, and measures their combined revenue concentration from 2022–2026.*


```sql
USE product_intelligence_2022_2026_v4;

WITH product_revenue AS (
    SELECT p.product_name,
           ROUND(SUM(u.recognized_revenue), 2) AS total_revenue
    FROM product_unit_economics_monthly u
    JOIN products p ON u.product_id = p.product_id
    WHERE CAST(u.month_start AS DATE) BETWEEN '2022-01-01' AND '2026-12-31'
    GROUP BY p.product_id, p.product_name
),
ranked AS (
    SELECT *,
           ROW_NUMBER() OVER (ORDER BY total_revenue DESC) AS revenue_rank,
           SUM(total_revenue) OVER () AS company_revenue
    FROM product_revenue
)
SELECT revenue_rank, product_name, total_revenue,
       ROUND(100 * total_revenue / NULLIF(company_revenue, 0), 2) AS revenue_share_pct,
       ROUND(100 * SUM(total_revenue) OVER (ORDER BY revenue_rank)
             / NULLIF(company_revenue, 0), 2) AS cumulative_revenue_share_pct
FROM ranked
WHERE revenue_rank <= 5
ORDER BY revenue_rank;
```

### Results

![Top Five Revenue Concentration](https://paper-attachments.dropboxusercontent.com/s_A13B3434EC2F7B53476EAA3038EE14284FECA31A9A8F827AE859B9C0A2F42694_1794037482135_file.png)








