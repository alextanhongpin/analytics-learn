The profitability/05_rfm.md file contains SQL queries for RFM (Recency, Frequency, Monetary) analysis. Based on this context, we can draft a query to measure the month-by-month revenue growth.
```sql
-- Month-by-month revenue growth
SELECT
    DATE_TRUNC('month', created_at) AS month,
    SUM(amount) AS total_revenue,
    LAG(SUM(amount)) OVER (ORDER BY DATE_TRUNC('month', created_at)) AS previous_month_revenue,
    (SUM(amount) - LAG(SUM(amount)) OVER (ORDER BY DATE_TRUNC('month', created_at))) / NULLIF(LAG(SUM(amount)) OVER (ORDER BY DATE_TRUNC('month', created_at)), 0) * 100 AS revenue_growth_percentage
FROM
    orders
GROUP BY
    month
ORDER BY
    month;
```
This query calculates the total revenue for each month and the percentage growth compared to the previous month. You can run this query on your database to get the required insights.



A suitable algorithm to check for recurrent purchases is the RFM Analysis (Recency, Frequency, Monetary). This method evaluates how recently a customer has made a purchase, how frequently they make purchases, and how much they spend.

Here is a sample SQL query to identify recurrent purchases using RFM Analysis:
```sql
-- Create a summary table for user transactions
WITH transaction_summary AS (
    SELECT
        user_id,
        COUNT(*) AS frequency,
        MAX(created_at) AS last_purchase_date,
        SUM(amount) AS total_spent
    FROM
        transactions
    GROUP BY
        user_id
),
-- Calculate RFM scores
rfm_scores AS (
    SELECT
        user_id,
        NTILE(4) OVER (ORDER BY MAX(created_at) DESC) AS recency_score,
        NTILE(4) OVER (ORDER BY COUNT(*) DESC) AS frequency_score,
        NTILE(4) OVER (ORDER BY SUM(amount) DESC) AS monetary_score
    FROM
        transactions
    GROUP BY
        user_id
)
-- Select users with high frequency scores indicating recurrent purchases
SELECT
    user_id,
    recency_score,
    frequency_score,
    monetary_score
FROM
    rfm_scores
WHERE
    frequency_score = 4
ORDER BY
    frequency_score DESC;
```
This query creates a summary of user transactions, calculates RFM scores, and selects users with high frequency scores, indicating recurrent purchases.
