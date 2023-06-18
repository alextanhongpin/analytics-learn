# Percentile


## Prompt
what does percentile means? how is it useful for analytics? can you ELI5?

## Response from Bard

Sure. A percentile is a measure of where a specific value falls within a distribution of data. It is calculated by dividing the data into 100 equal parts, and then finding the value that falls within the desired percentile. For example, the 50th percentile is the median, which means that half of the data values are above it and half are below it.

Percentiles are useful for analytics because they can be used to compare different data sets or to identify outliers. For example, if you have two data sets of test scores, you can use percentiles to see which data set has the higher average score. You can also use percentiles to identify students who are performing significantly below or above average.

Here is an example of how percentiles can be used in analytics. Let's say you have a data set of customer satisfaction scores. You can use percentiles to see what percentage of customers are satisfied with your product or service. You can also use percentiles to identify customers who are likely to churn, or cancel their subscription.

Percentiles can be a bit difficult to understand at first, but they are a powerful tool for analytics. Once you understand how they work, you can use them to gain valuable insights into your data.

Here is an ELI5 explanation of percentiles:

Imagine you have a class of 20 students. You want to know how well each student did on a test. You could rank the students from highest to lowest score, but that wouldn't tell you how well each student did relative to the rest of the class.

Percentiles can help you with this. A percentile tells you what percentage of students scored lower than a particular student. For example, if a student scores in the 75th percentile, that means that 75% of the students in the class scored lower than them.

Percentiles can be used to compare different data sets or to identify outliers. For example, if you have two data sets of test scores, you can use percentiles to see which data set has the higher average score. You can also use percentiles to identify students who are performing significantly below or above average.

Percentiles are a valuable tool for analytics. Once you understand how they work, you can use them to gain valuable insights into your data.




## Queries

We use the table created in [02_conversion_rate](02_conversion_rate.md).


```sql
-- Discrete takes the lowest, continuous takes the middle point.
with numbers as (
	select 1 as n
	union
	select 2 as n
)
select
	percentile_disc(0.5) within group (order by n),
	percentile_cont(0.5) within group (order by n)
from numbers;

-- Another example.
with numbers as (
	select 1 as n
		union
	select 1 as n
		union
	select 2 as n
		union
	select 5 as n
		union
	select 100 as n
)
select
	percentile_disc(0.5) within group (order by n),
	percentile_cont(0.5) within group (order by n)
from numbers;
```

Metrics:
```sql
select
	min(amount),
	max(amount),
	avg(amount)
from orders;
```


Different ways to calculate percentile. One is using `ntile`, where we break the dataset into 100 almost equal buckets. We can then select the percentile that we are interesed in.


```sql
with buckets as (
	select
		amount as n,
		ntile(100) over (order by amount) as bin --- Split into 100 equal buckets
	from orders
)
select
	min(n),
	max(n),
	bin || '% purchases is less than ' || max(n) as percentile,
	count(bin) as bins_count
from buckets
where bin in (50, 75, 90, 95, 99, 100)
group by bin
order by bin;
```

Alternatively, we can use the `percentile_disc` or `percentile_cont` method. Below, we used `percentile_disc` because `percentile_cont` gives the continuous value, but it might be misleading because we might not have orders with that amount.

```sql
select
	percentile_disc(0.5) within group (order by amount),
	percentile_disc(0.75) within group (order by amount),
	percentile_disc(0.90) within group (order by amount),
	percentile_disc(0.95) within group (order by amount),
	percentile_disc(1.0) within group (order by amount)
from orders;
```


We can validate by checking the percentage of orders that is above for the given percentile:

```sql
-- From the percentile above, we can deduce that 99% has purchase less than 988.20$.
-- Only 1% should have purchases about 988.20.
-- We can use this query to validate it.
select count(*) filter (where amount > 98820)::numeric / count(*) * 100 as percent
from orders;
```


## Ecommerce Dataset

Seed using `ecommerce.ipynb`.

```sql
select *
from orders
limit 5;

select
	min("UnitPrice"),
	max("UnitPrice")
from orders;


-- How many percentage of the orders are free?
select
	count(*) filter (where "UnitPrice" = 0)::numeric / count(*) * 100 as percentage_free
from orders;


-- Find top-n stock codes.
select
	"StockCode",
	count(*) as total,
	(array_agg(distinct "Description")) as unique_descriptions
from orders
group by "StockCode"
order by count(*) desc
limit 10;

-- Find top purchasers.
select
	"CustomerID",
	count(*) as total
from orders
group by "CustomerID"
order by count(*) desc
limit 10;

select sum("UnitPrice" * "Quantity"), "CustomerID", ntile(100) over (partition by "CustomerID" order by sum("UnitPrice" * "Quantity"))
from orders
group by "CustomerID"
order by 1;

-- Find the bins for the total cost of the order.
with bins as (
	select
		"UnitPrice" * "Quantity" as total,
		ntile(100) over (order by "UnitPrice" * "Quantity") bin
	from orders
)
select round(max(total)::numeric, 2) as total, bin
from bins
where bin in (50, 75, 90, 95, 99, 100)
group by bin
order by bin;
-- 50% have orders more than 9.90
-- 25% have orders more than 17.7
-- 10% have orders more than 32.6

select count(*)
from orders
where "UnitPrice" * "Quantity" > 183.6;
select count(*) from orders;

select 5279::numeric / 531283 * 100;


with total_purchases_per_user as (
	select
		"CustomerID",
		sum("UnitPrice" * "Quantity") as total
	from orders
	group by "CustomerID"
),
bins as (
	select
		total,
		ntile(100) over (order by total) bin
	from total_purchases_per_user
	order by 1
)
select
	round(min(total)::numeric, 2),
	round(max(total)::numeric, 2),
	bin,
	count(*),
	sum(count(*)) over (order by bin) as total_users
from bins
where bin in (50, 75, 90, 95, 99, 100)
group by bin
order by bin;

-- less than 10% has total purchase order more than 3679.97

select
distinct "CustomerID"
from orders;


with total_purchases_per_user as (
	select
		"CustomerID",
		sum("UnitPrice" * "Quantity") as total
	from orders
	group by "CustomerID"
)
select *
from total_purchases_per_user
where total > 3448.29 and total <= 3679.97
order by total;
```
